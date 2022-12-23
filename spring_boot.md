# Spring Boot

Spring initializr (https://start.spring.io/)

Dependências: 
- Spring Web
- Spring Data JPA
- Driver do Banco de Dados (MySQL, Oracle...)
- Spring Boot Devtools: permite restart automático da aplicação durante o desenvolvimento
- Lombok: para não ser necessário escrever getters, setters e construtores
- Validation
- Flyway Migration: para controlar os scripts de migração de banco de dados

## ) Formas de organização do código
Por recurso: Um pacote para cada funcionalidade. Inclui as classes controller, DTO e repository no mesmo pacote.

Por camada (Package by Layer): Agrupa models, controllers e DTOs em pacotes específicos.


## ) Controller Rest

### Anotações da classe controller

```java
@RestController
@RequestMapping("/pagamentos")
public class PagamentoController {

    @Autowired
    private PagamentoService service;
    
    //métodos GET, POST, PUT, DELETE...

}
```
### Métodos da classe controller

A implentação das regras de negócio fica nos métodos na camada Service

GET - Listar recursos com paginação:
```java
@GetMapping
public Page<PagamentoDto> listar(@PageableDefault(size = 10) Pageable paginacao) {
    return service.obterTodos(paginacao);
}
```

GET - Consultar um recurso:
```java
@GetMapping("/{id}")
public ResponseEntity<PagamentoDto> detalhar(@PathVariable @NotNull Long id) {
    PagamentoDto dto = service.obterPorId(id);

    return ResponseEntity.ok(dto);
}
```

POST - Criar um recurso:
```java
@PostMapping
public ResponseEntity<PagamentoDto> cadastrar(@RequestBody @Valid PagamentoDto dto, UriComponentsBuilder uriBuilder) {
    PagamentoDto pagamento = service.criarPagamento(dto);
    URI endereco = uriBuilder.path("/pagamentos/{id}").buildAndExpand(pagamento.getId()).toUri();

    return ResponseEntity.created(endereco).body(pagamento);
}
```

PUT - Atualizar um recurso com URI conhecido:
```java
@PutMapping("/{id}")
public ResponseEntity<PagamentoDto> atualizar(@PathVariable @NotNull Long id, @RequestBody @Valid PagamentoDto dto) {
    PagamentoDto atualizado = service.atualizarPagamento(id, dto);
    return ResponseEntity.ok(atualizado);
}
```

DELETE - Apagar um recurso:
```java
@DeleteMapping("/{id}")
public ResponseEntity<PagamentoDto> remover(@PathVariable @NotNull Long id) {
    service.excluirPagamento(id);
    return ResponseEntity.noContent().build();
}
```

Outra opção de anotação para os métodos:
```java
@RequestMapping(value = "/coffees", method = RequestMethod.GET)
```

Anotações para acessar valores recebidos na requisição:
- @PathVariable - acessa o valor passaddo na URL
- @RequestBody - acessa o body enviado na requisição
- @RequestParam - acessa o valor passado como parâmetro na requisição (após o "?" da URL)


## ) Response Entity
Objeto que representa o response HTTP completo (status code, headers e o body) que é retornado na requisição.

```java
return new ResponseEntity<>(coffee, HttpStatus.CREATED);
```

```java
return ResponseEntity.ok(coffee);
```

```java
return ResponseEntity.notFound().build();
```


## ) Padrão Data Transfer Object (DTO)
Padrão arquitetural introduzido por Martin Fowler (livro EAA). Uma classe que representa os dados recebidos/enviados pela api, para desacoplar da entidade que representa a tabela do banco de dados.

```java
@PostMapping
public ResponseEntity<UsuarioRespostaDTO> salvar(@RequestBody UsuarioDTO usuarioDTO){
    Usuario usuario = mapper.toUsuario(usuarioDTO);
    //segue a lógica do método
}
```

A conversão DTO -> Entidade deve ser feita em uma classe separada mapper ou converter.

Converter uma lista de objetos em uma lista de DTOs:
```java
return usuarios.stream().map(mapper::toDto).collect(toList());
``` 


## ) Banco de dados (Model)

Propriedades necessárias no arquivo application.properties (banco MySQL)
```
spring.datasource.url=jdbc:mysql://localhost:3306/nomedobanco
spring.datasource.username=<usuario>
spring.datasource.password=<senha>
```


### Classe entidade (mapeia uma tabela do banco)

```java
@Entity
@Table(name = "pagamentos")
@Getter
@Setter
@AllArgsConstructor
@NoArgsConstructor
public class Pagamento {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @NotNull
    @Positive
    private BigDecimal valor;

    @NotBlank
    @Size(max=100)
    private String nome;

    @NotBlank
    @Size(max=19)
    private String numero;

    @NotBlank
    @Size(max=7)
    private String expiracao;

    @NotBlank
    @Size(min=3, max=3)
    private String codigo;

    @NotNull
    @Enumerated(EnumType.STRING)
    private Status status;

    @NotNull
    private Long pedidoId;

    @NotNull
    private Long formaDePagamentoId;
}
```

### Repository
Interface que já vem com vários métodos implementados que facilitam a criação do CRUD básico 

<classe da entidade, tipo do id>

```java
public interface PagamentoRepositoy extends JpaRepository<Pagamento, Long> {}
```

Alguns métodos já existentes na interface repository:
- coffeeRepository.findAll()
- coffeeRepository.findById(id)
- coffeeRepository.save(coffee)
- coffeeRepository.deleteById(id)


## ) Injeção de Dependências @Autowired
Muito usado para injetar interfaces repository e services

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

## ) Service

Classes usadas para que as regras de manipulação não fiquem no controller



## ) Arquivo application.properties

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
@Value("${greeting-name: João}")
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

