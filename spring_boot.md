# Spring Boot

Spring initializr (https://start.spring.io/)

Dependências: 
- Spring Web
- Spring Data JPA
- Driver do Banco de Dados (MySQL, Oracle...)


## 1) Controller Rest

### Anotações da classe controller

```java
@RestController
@RequestMapping("/")
public class RestController(){

    //métodos GET, POST, PUT, DELETE...

}
```
### Métodos da classe controller

GET - Consultar um recurso:
```java
 @GetMapping("/coffees/{id}") 
 public Optional<Coffee> getCoffeeById(@PathVariable String id) {} 
```

POST - Criar um recurso:
```java
@PostMapping("/coffees)
public ResponseEntity<Coffee> postCoffee(@RequestBody Coffee coffee) {}
```

PUT - Atualizar um recurso com URI conhecido (se existir atualiza, senão cria):
```java
@PutMapping("/coffees/{id}")
public ResponseEntity<Coffee> putCoffee(@PathVariable String id, @RequestBody Coffee coffee) {}
```

DELETE - Apagar um recurso:
```java
@DeleteMapping("/coffees/{id}")
public void deleteCoffee(@PathVariable String id){}
```

Outra opção de anotação para os métodos:
```java
@RequestMapping(value = "/coffees", method = RequestMethod.GET)
```

### Anotações para acessar valores recebidos na requisição
@PathVariable - acessa o valor passaddo na URL
@RequestBody - acessa o body enviado na requisição
@RequestParam - acessa o valor passado como parâmetro na requisição (após o "?" da URL)


## 2) Response Entity
Objeto que representa o response HTTP completo (status code, headers e o body) que é retornado na requisição.

```java
return new ResponseEntity<String>(coffee, HttpStatus.CREATED);
```

```java
return ResponseEntity.ok(coffee);
```

```java
return ResponseEntity.notFound().build();
```


## 3) Padrão Data Transfer Object (DTO)
Padrão arquitetural introduzido por Martin Fowler (livro EAA) 
Uma classe que representa os dados recebidos/enviados pela api, para desacoplar da entidade que representa a tabela do banco de dados.

```java
@PostMapping
public ResponseEntity<UsuarioRespostaDTO> salvar(@RequestBody UsuarioDTO usuarioDTO){
	Usuario usuario = mapper.toUsuario(usuarioDTO);
	//segue a lógica do método
}
```

A conversão DTO -> Entidade deve ser feita em uma classe separa mapper ou converter.

Converter uma lista de objetos em uma lista de DTOs:
```java
return usuarios.stream().map(mapper::toDto).collect(toList());
``` 


## 4) Banco de dados

Propriedades necessárias no arquivo application.properties (banco MySQL)
```
spring.datasource.url=jdbc:mysql://localhost:3306/nomedobanco
spring.datasource.username=<usuario>
spring.datasource.password=<senha>
```


### Classe entidade (mapeia uma tabela do banco)

```java
@Entity
class Coffee{

	@Id
	@GeneratedValue(strategy=GenerationType.IDENTITY)
	private String id;
	
	//outras colunas...
	
	//getters e setters
}
```



### Repository
Interface que já vem com vários métodos implementados
<classe da entidade, tipo do id>
```java
interface CoffeeRepository extends CrudRepository<Coffee, String> {}
```

Injeção de dependência no Controller:
```java
@RestController
class RestController{
	private final CoffeeRepository coffeeRepository;
	
	public RestController(CoffeeRepository coffeeRepository) {
		this.coffeeRepository = coffeeRepository;
	}
	
	//Controller Methods...
}
```

Alguns métodos já existentes na interface repository:
- coffeeRepository.findAll()
- coffeeRepository.findById(id)
- coffeeRepository.save(coffee)
- coffeeRepository.deleteById(id)


## 5) Injeção de Dependências @Autowired
Muito usado para injetar interfaces repository

```java
public class RestController{
	@Autowired
	private CoffeeRepository coffeeRepository;
}
```

Injeção no construtor (não necessita da anotação @Autowired a partir do Spring 4.3):
```java
public class RestController{
	private final CoffeeRepository coffeeRepository;

	public RestController(CoffeeRepository coffeeRepository) {
		this.coffeeRepository = coffeeRepository;
	}
}
```

## 6) Arquivo application.properties

Apontando para uma variável de ambiente (externa ao código) e definindo um valor default
```java
server.port=${PORT:8080}
```

### Injetando propriedades com @Value

application.properties
```java
greeting-name=Mariane
```

Lendo uma propriedade (o valor padrão é opcional)
```java
@Value("${greeting-name: Peter}")
private String name;
```

### Acessando informações com @ConfigurationProperties

application.properties
```java
greeting.name=Mariane
greeting.coffee=${greeting.name} is drinking Cafe Cereza
```

Classe de configuração que lê todas as propriedades que iniciam com o prefixo definido
```java
@Configuration
@ConfigurationProperties(prefix = "greeting")
public class Greeting {
	private String name;
	private String coffee;
	
	//getters e setters
}
```

Para usar as propriedades, basta injetar a classe de configuração:
```java
@RestController
@RequestMapping("/greeting")
public class GreetingController {

	private final Greeting greeting;

	public GreetingController(Greeting greeting){
		this.greeting = greeting;
	}

	@GetMapping
	String getGreeting() {
		return greeting.getName();
	}
}
```

