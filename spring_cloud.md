# Spring Cloud

O [Spring Cloud](https://spring.io/projects/spring-cloud) fornece várias ferramentas para implementação facilitada de alguns padrões comuns em sistemas distribuídos.

## 1) Spring Cloud Netflix

### Service Discovery (Eureka Server)
Um catálogo com o endereço de todos os microsserviços registrados nele. 
É uma aplicação java (spring boot) simples, apenas com algumas configurações.

Dependência:
- spring-cloud-starter-netflix-eureka-server


A anotação @EnableEurekaServer é necessária na classe principal da aplicação
```java
@SpringBootApplication
@EnableEurekaServer
public class ServerApplication {
    public static void main(String[] args) {
        SpringApplication.run(ServerApplication.class, args);
    }
}
```

Configurações necessárias no arquivo application.properties:
```
#É importante definir uma porta para o Eureka Server
server.port=8081

#Configurações para não incluir o service discovery nele mesmo 
eureka.client.register-with-eureka=false
eureka.client.fetch-registry=false

#nome da aplicação
spring.application.name=server

eureka.client.serviceUrl.defaultZone=http://localhost:8081/eureka
```

### Configurações nos microsserviços clientes

Configurações para incluir um microsserviço na listagem do service discovery

Dependência:
- spring-cloud-starter-netflix-eureka-client

A anotação @EnableEurekaClient é necessária na classe principal da aplicação
```java
@SpringBootApplication
@EnableEurekaClient
public class PagamentosApplication {
    public static void main(String[] args) {
        SpringApplication.run(PagamentosApplication.class, args);
    }
}
```

Configurações necessárias no arquivo application.properties:
```
#nome do microsserviço
spring.application.name=pagamentos-ms

#deixa que o Eureka Server controle a porta em que a aplicação ficará disponível
server.port=0

eureka.client.serviceUrl.defaultZone=http://localhost:8081/eureka
```

Cada vez que uma instância do microsserviço é iniciada, o Eureka indica uma porta aleatória para o serviço

## 2) Spring Cloud Gateway

Centraliza o acesso aos diversos microsserviços da aplicação (mas pode se tornar um ponto de falha)

Uma aplicação java (Spring Boot) com as dependências:
- spring-cloud-starter-gateway
- spring-cloud-starter-netflix-eureka-client

A anotação @EnableEurekaClient é necessária na classe principal da aplicação
```java
@SpringBootApplication
@EnableEurekaClient
public class GatewayApplication {
    public static void main(String[] args) {
        SpringApplication.run(GatewayApplication.class, args);
    }
}
```

Configurações necessárias no arquivo application.properties:
```
#É importante ter uma porta definida
server.port=8082

#nome da aplicação
spring.application.name=gateway

eureka.client.serviceUrl.defaultZone=http://localhost:8081/eureka
spring.cloud.gateway.discovery.locator.enabled=true

#define que o nome das aplicações sejam em letra minúscula
spring.cloud.gateway.discovery.locator.LowerCaseServiceId=true
```

Assim, bastar chamar a url e porta do gateway  + /{nome do microsserviço}/{endpoint} para acessar as outras apis.

### Balanceamento de carga

O gateway faz automaticamente o balanceamento de carga das requisições entre as instâncias do microsserviço que estiverem ativas

Para isso, é necessário adicionar no aplication.properties do microsserviço a informação do id da instância:
```
#nome do microsserviço
spring.application.name=pagamentos-ms

eureka.instance.instance-id=${spring.application.name}:${random.int}
```

## 3) Spring Cloud OpenFeign

Um cliente HTTP para fazer integrações de backend para backend através de anotações

Dependência:
- spring-cloud-starter-openfeign

É necessário adicionar na classe principal a anotação @EnableFeignClients:
```java
@SpringBootApplication
@EnableFeignClients
public class PagamentosApplication {
    public static void main(String[] args) {
        SpringApplication.run(PagamentosApplication.class, args);
    }
}
```

A configuração da requisição HTTP para a outra api é feita em uma interface, adicionando algumas anotações:
```java
//informar o nome do microsserviço que está sendo chamado como está definido na variável spring.application.name
@FeignClient("pedidos-ms")
public interface PedidoClient {
    //informa o tipo da requisição e o endpoint que deve ser chamado
    @RequestMapping(method = RequestMethod.PUT, value = "/pedidos/{id}/pago")
    void atualizaPagamento(@PathVariable Long id);
}
```

A interface pode ser injetada no service e o método pode ser usado para acessar o endpoint do outro microsserviço
```java
@Autowired
private PedidoClient pedido;

//...

pedido.atualizaPagamento(pagamento.get().getPedidoId());
```


## 4) Tratamento de erros (Circuit Breaker e Fallback)

Padrão de projeto para tratar falhas na integração entre os serviços. As configurações são feitas no serviço cliente, que consome uma outra api.

### Circuit Breaker (disjuntor)

Tem três estados:
- Closed: Quando tudo está funcionando normalmente.
- Open: Se o número de falhas chegar a um limiar determinado, o Circuit Breaker não executa mais a chamada ao outro serviço e retorna um erro (ou executa o método fallback).
- Half Open: Após um período definido, a aplicação testa se o problema original ainda ocorre. Se uma falha ocorrer, o estado é alternado para Open novamente. Se for bem-sucedido, volta ao normal (Closed).

Dependências:
	- resilience4j-spring-boot2
	- spring-boot-starter-aop
    
O método do controller, que faz internamente a chamada a outra api, precisa ser anotada com @CircuitBreaker:
```java
@PatchMapping("/{id}/confirmar")
@CircuitBreaker(name = "atualizaPedido", fallbackMethod = "pagamentoAutorizadoComIntegracaoPendente")
public void confirmarPagamento(@PathVariable @NotNull Long id){
    service.confirmarPagamento(id);
}
```
	
Configurações necessárias no application.properties:
```
#define a quantidade de requisições necessárias para voltar ao estado Open/Closed
resilience4j.circuitbreaker.instances.atualizaPedido.slidingWindowSize=3

#mínimo de chamadas até o circuitbreaker entrar em ação
resilience4j.circuitbreaker.instances.atualizaPedido.minimunNumberOfCalls=2

#uma vez que o estado mudou para aberto, quanto tempo vai ficar em aberto
resilience4j.circuitbreaker.instances.atualizaPedido.waitDurationInOpenState=50s 
```

### Fallback

Em caso de falha e acionamento do Circuit Breaker, um método "plano B" pode ser chamado ao invés de apenas retornar um erro.

O método de fallback é indicado na anotação @CircuitBreaker

Esse método pode, por exemplo, deixar o registro em um estado pendente que depois pode ser resolvido de alguma forma.

O método de fallback precisa ter o mesmo retorno e receber os mesmos parâmetros que o método do controller correspondente. Pode ser acrescentado um parâmetro adicional (Exception e) para caso o método de fallback não funcione.

```java
@PatchMapping("/{id}/confirmar")
@CircuitBreaker(name = "atualizaPedido", fallbackMethod = "pagamentoAutorizadoComIntegracaoPendente")
public void confirmarPagamento(@PathVariable @NotNull Long id){
    service.confirmarPagamento(id);
}

public void pagamentoAutorizadoComIntegracaoPendente(Long id, Exception e){
    service.alteraStatus(id);
}
```
