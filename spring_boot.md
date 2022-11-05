# Spring Boot

Spring initializr (https://start.spring.io/)

Dependência básica: Spring Web

## 1) API Rest

### Classe controller

```java
@RestController
@RequestMapping("/")
public class RestController(){}
```
### Métodos da classe controller

```java
@RequestMapping(value = "/coffees", method = RequestMethod.GET)
```

GET - Consultar um recurso:
```java
 @GetMapping("/coffees/{id}") 
 Optional<Coffee> getCoffeeById(@PathVariable String id) {} 
```

POST - Criar um recurso:
```java
@PostMapping("/coffees)
ResponseEntity<Coffee> postCoffee(@RequestBody Coffee coffee) {
	//...
	return new ResponseEntity(coffee, HttpStatus.CREATED);
}
```

PUT - Atualizar um recurso com URI conhecido (se existir atualiza, senão cria):
```java
@PutMapping("/coffees/{id}")
ResponseEntity<Coffee> putCoffee(@PathVariable String id, @RequestBody Coffee coffee) {
	//...
	return new ResponseEntity(coffee, HttpStatus.OK);
}
```

DELETE - Apagar um recurso:
```java
@DeleteMapping("/coffees/{id}")
void deleteCoffee(@PathVariable String id){}
```

## 2) Banco de dados

Dependências: 
- spring-boot-starter-data-jpa (org.springframework.boot)
- Driver do banco: h2 (com.h2database)

### Classe entidade (mapeia uma tabela do banco)

```java
@Entity
class Coffee{

	@Id
	private String id;
	
	//other columns...
	
	//getters and setters
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

## 3) Acessando informações do arquivo application.properties

### @Value

application.properties
```java
greeting-name=Mariane
```

Lendo uma propriedade (o valor padrão é opcional)
```java
@Value("${greeting-name: Peter}")
private String name;
```

### @ConfigurationProperties

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
	
	//getters and setters
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

