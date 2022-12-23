# Spring Cloud

O [Spring Cloud](https://spring.io/projects/spring-cloud) fornece várias ferramentas para implementação facilitada de alguns padrões comuns em sistemas distribuídos.

## 1) Spring Cloud Netflix

### Service Discovery (Eureka Server)
Um catálogo com o endereço de todos os microsserviços registrados nele

Uma aplicação java (spring boot) simples. São necessárias apenas essas poucas configurações.

Dependência:
- spring-cloud-starter-netflix-eureka-server

A anotação @EnableEurekaServer é necessária na classe principal da aplicação
```
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

### Configuração nos microsserviços clientes

Configurações para incluir um microsserviço na listagem do service discovery

Dependência:
- spring-cloud-starter-netflix-eureka-client

A anotação @EnableEurekaClient é necessária na classe principal da aplicação
```java
@SpringBootApplication
@EnableEurekaClient
public class ServerApplication {
    public static void main(String[] args) {
        SpringApplication.run(ServerApplication.class, args);
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

Criar um projeto Spring Boot com as dependências:
- spring-cloud-starter-gateway
- spring-cloud-starter-netflix-eureka-client

A anotação @EnableEurekaClient é necessária na classe principal da aplicação
```java
@SpringBootApplication
@EnableEurekaClient
public class ServerApplication {
    public static void main(String[] args) {
        SpringApplication.run(ServerApplication.class, args);
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

É necessário adicionar no aplication.properties do microsserviço a informação do id da instância:
```
#nome do microsserviço
spring.application.name=pagamentos-ms

eureka.instance.instance-id=${spring.application.name}:${random.int}
```

## 3) Spring Cloud OpenFeign



## 4) Tratamento de erros (Circuit Breaker e Fallback)
