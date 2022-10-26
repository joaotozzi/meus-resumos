# Spring Boot

Spring initializr (https://start.spring.io/)

Dependências básicas:
- Spring Web

## 1) API Rest

### Anotações na classe controller

```java
@RestController
@RequestMapping("/")
public class RestController(){}
```
### Anotações nos métodos da classe controller

```java
@RequestMapping(value = "/coffees", method = RequestMethod.GET)
```

Consultar um recurso (GET):
```java
 @GetMapping("/coffees/{id}") 
 Optional<Coffee> getCoffeeById(@PathVariable String id) {} 
```

Criar um recurso (POST):
```java
@PostMapping("/coffees)
ResponseEntity<Coffee> postCoffee(@RequestBody Coffee coffee) {
	//...
	return new ResponseEntity(coffee, HttpStatus.CREATED);
}
```

Atualizar um recurso com URI conhecido (PUT).
Se existir um recurso com o id especificado, atualiza, senão cria o recurso:
```java
@PutMapping("/coffees/{id}")
ResponseEntity<Coffee> putCoffee(@PathVariable String id, @RequestBody Coffee coffee) {
	//...
	return new ResponseEntity(coffee, HttpStatus.OK);
}
```

Apagar um recurso (DELETE):
```java
@DeleteMapping("/coffees/{id}")
void deleteCoffee(@PathVariable String id){}
```

## 2) Acesso a banco de dados

Dependências: 
- spring-boot-starter-data-jpa (org.springframework.boot)
- Driver do banco de dados. Ex.: h2 (com.h2database)

### Mapeando um tabela para uma classe

```java
@Entity
class Coffee{

	@Id
	private String id;
	
	//other columns...
	
	//getters and setters
}
```

### Interface Repository
Declarando uma interface repository (já vem com vários métodos implementados)
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

## 3) Acessando propriedades definidas no arquivo application.properties

### @Value

application.properties
```java
greeting-name=Mariane
greeting-coffee=${greeting-name} is drinking Café Cereza 
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

