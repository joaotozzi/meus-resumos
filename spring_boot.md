# Spring Boot

Uma ferramenta para criação facilitada de aplicações Java que utilizam o framework Spring.

[Spring initializr](https://start.spring.io/): Ferramenta web para a geração de projetos spring boot

Dependências comuns: 
- Spring Web
- Spring Data JPA
- Driver do Banco de Dados (MySQL, Oracle...)
- Spring Boot Devtools (ferramentas de desenvolvedor)
- Lombok (getters, setters e construtores automáticos)
- Validation
- Flyway Migration (controle de scripts de migração de banco de dados)

## 1) Métodos de modularização
Package by Layer: As classes são colocadas no pacote da camada arquitetônica a que pertencem.
```
── br.com.nomedaempresa.nomedoprojeto
    └── controller
        ├── PagamentoController
        └── PedidoController
    └── model
        ├── Pagamento   
        └── Pedido
    └── repository
        ├── PagamentoRepository   
        └── PedidoRepository
    └── service
        ├── PagamentoService
        └── PedidoService
```
Package by Feature: Cada pacote possui todas as classes necessárias para a funcionalidade
```
├── br.com.nomedaempresa.nomedoprojeto
    └── pagamento
        ├── Pagamento
        ├── PagamentoController
        ├── PagamentoRepository        
        └── PagamentoService
    └── pedido
        ├── Pedido   
        ├── PedidoController
        ├── PedidoRepository
        └── PedidoService
```


## 2) Banco de dados (Model)

Propriedades de configuração necessárias no arquivo application.properties (banco MySQL)
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

### Lombok

Biblioteca java que usa anotações para gerar automaticamente métodos getters, setters e construtores
- @Getter
- @Setter
- @AllArgsConstructor
- @NoArgsConstructor

### Bean Validation

Especificação que permite validar objetos com base nas retrições adicionadas na classe de modelo (classe Entity)
- @NotNull
- @Positive
- @NotBlank
- @Size(min=3, max=3)

Para usar essa validação basta anotar o parâmetro de um método com @Valid

### Repository
Interface que já possui vários métodos implementados que facilitam a criação de um CRUD básico 

```java
// informa <classe da entidade, tipo do id>
public interface PagamentoRepositoy extends JpaRepository<Pagamento, Long> {

}
```

Alguns métodos já existentes na interface repository:
- coffeeRepository.findAll()
- coffeeRepository.findById(id)
- coffeeRepository.save(coffee)
- coffeeRepository.deleteById(id)


### Migrations (Flyway)

Flyway é uma ferramenta para atualizar o banco de dados sem a necessidade de executar os scripts sql manualmente.

Migrations: Histórico de tudo que aconteceu na base de dados

Os scripts devem ficar na pasta resources/db.migration

Padrão de nomeclatura dos scripts:

V{numero}__{nome_do_comando}.sql

Ex.: V1__criar_tabela_pagamentos.sql

Cada alteração do banco deve ser feita em um novo arquivo (nunca editar um script já existente)

É criada uma tabela flyway_schema_history contendo o histórico de todos os scripts executados, para que o flyway não execute novamentes os scripts antigos a cada novo deploy 

## 3) Controller

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

A anotação @PageableDefault injeta um objeto Pageable no método controller, permitindo a paginação automática do resultado
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

Quando o endpoint retorna um código 201(created) deve ser devolvida a URI do recurso que acabou de ser criado. Isso é feito com o uso do UriComponentsBuilder.
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
@RequestMapping(value = "/{id}", method = RequestMethod.GET)
```

### Anotações para acessar valores recebidos na requisição
- @PathVariable - acessa o valor passaddo na URL
- @RequestBody - acessa o body enviado na requisição.
- @RequestParam - acessa o valor passado como parâmetro na requisição (após o "?" da URL)

### Response Entity
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


### Padrão Data Transfer Object (DTO)
Padrão arquitetural introduzido por Martin Fowler (livro EAA). Uma classe que representa os dados recebidos/enviados pela api, para desacoplar da entidade que representa a tabela do banco de dados.

```java
@PostMapping
public ResponseEntity<UsuarioRespostaDTO> salvar(@RequestBody UsuarioDTO usuarioDTO){
    Usuario usuario = mapper.toUsuario(usuarioDTO);
    //segue a lógica do método
}
```

A conversão DTO -> Entidade deve ser feita em uma classe separada mapper ou converter.

### Injeção de dependências
Padrão de projeto que ajuda deixar o código com baixo acoplamento. O frameworrk fica responsável por injetar os objetos necessários

Injeção de dependência com @Autowired:
```java
@Autowired
private PagamentoService service;
```

Injeção no construtor (não necessita da anotação @Autowired a partir do Spring 4.3):
```java
public class PagamentoController{
    
    private final PagamentoService service;

    public PagamentoController(PagamentoService service) {
        this.service = service;
    }
}
```

## 4) Service

Classes usadas para que as regras de manipulação não fiquem no controller. Recebem a anotação @Service

```java
@Service
public class PagamentoService {

    @Autowired
    private PagamentoRepositoy repository;

    @Autowired
    private ModelMapper modelMapper;

    public Page<PagamentoDto> obterTodos(Pageable paginacao) {
        return repository
                .findAll(paginacao)
                .map(p -> modelMapper.map(p, PagamentoDto.class));
    }

    public PagamentoDto obterPorId(Long id) {
        Pagamento pagamento = repository.findById(id)
                .orElseThrow(() -> new EntityNotFoundException());

        return modelMapper.map(pagamento, PagamentoDto.class);
    }

    public PagamentoDto criarPagamento(PagamentoDto dto) {
        Pagamento pagamento = modelMapper.map(dto, Pagamento.class);
        pagamento.setStatus(Status.CRIADO);
        repository.save(pagamento);

        return modelMapper.map(pagamento, PagamentoDto.class);
    }

    public PagamentoDto atualizarPagamento(Long id, PagamentoDto dto) {
        Pagamento pagamento = modelMapper.map(dto, Pagamento.class);
        pagamento.setId(id);
        pagamento = repository.save(pagamento);
        return modelMapper.map(pagamento, PagamentoDto.class);
    }

    public void excluirPagamento(Long id) {
        repository.deleteById(id);
    }
}
```

### ModelMapper

A conversão automática entre DTO e entidade pode ser feita com a dependência ModelMapper

Os nomes dos atributos devem ser os mesmos nas duas classes

É necessário indicar para o Spring que o modelMapper é um bean, para poder injetá-lo como dependência na classe service
```java
@Configuration
public class Configuracao {

    @Bean
    public ModelMapper obterModelMapper() {
        return new ModelMapper();
    }
}
```

Converter Entity -> DTO:
```java
modelMapper.map(pagamento, PagamentoDto.class);
```

Converter DTO -> Entity:
```java
modelMapper.map(dto, Pagamento.class);
```

## 5) Variáveis de ambiente

Apontando para uma variável de ambiente (externa ao código) e definindo um valor default:
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

