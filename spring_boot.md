# Spring Boot

Spring initializr (https://start.spring.io/)

Dependências:
- Spring Web

## 1) API Rest

### Anotações para classes controller
- @RestController
- @RequestMapping("/")

### Anotações para métodos de um controller

@RequestMapping(value = "/coffees", method = RequestMethod.GET)

Consultar recursos:
```java
 @GetMapping("/coffees/{id}") 
 Optional<Coffee> getCoffeeById(@PathVariable String id) {} 
```

Criar recursos:
```java
@PostMapping("/coffees)
ResponseEntity<Coffee> postCoffee(@RequestBody Coffee coffee) {
	//...
	return new ResponseEntity(coffee, HttpStatus.CREATED);
}
```

Atualizar recursos existentes, com URIs conhecidas.
Se o recurso não existir, ele deve ser criado:
```java
@PutMapping("/coffees/{id}")
ResponseEntity<Coffee> putCoffee(@PathVariable String id, @RequestBody Coffee coffee) {
	//...
	return new ResponseEntity(coffee, HttpStatus.OK);
}
```

Excluir um recurso:
```java
@DeleteMapping("/coffees/{id}")
void deleteCoffee(@PathVariable String id){}
```

## 2) Acesso a Banco de Dados

Dependências: 
- spring-boot-starter-data-jpa (org.springframework.boot)
- driver do banco de dados. Ex.: h2 (com.h2database)

### Mapeando as tabelas do BD

```java
@Entity
class Coffee{

	@Id
	private String id;
	
	//outras colunas...
	
	//necessita de um contrutor sem argumentos, getters e setters
}
```

### Interfaces Repository
Declaração da interface repository
<Objeto que mapeia a tabela, tipo da chave primária>
```java
interface CoffeeRepository extends CrudRepository<Coffee, String> {}
```

Injetando a dependência no controller:
```java
@RestController
class NomeDoController{
	private final CoffeeRepository coffeeRepository;
	
	public NomeDoController(CoffeeRepository coffeeRepository) {
		this.coffeeRepository = coffeeRepository;
	}
	
	//métodos do controller...
}
```

Alguns métodos existentes:
- coffeeRepository.findAll()
- coffeeRepository.findById(id)
- coffeeRepository.save(coffee)
- coffeeRepository.deleteById(id)