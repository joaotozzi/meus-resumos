# Arquitetura de Microsserviços

Microsserviços = serviços indenpendentes

### Limitações de aplicações monolíticas
* Demora no deploy
* Falhas podem derrubar o sistema todo
* Um projeto usa apenas uma tecnologia/linguagem

### Benefícios dos microsserviços
* Facilita mudança/manutenção
* Permite times menores e autônomos
* Possibilita o reuso de serviços em outros projetos
* Permite ter diversidade tecnológica e experimentação
* Maior isolamento de falhas
* Escalabilidade independente e flexível

### Desvantagens dos microsserviços
* Maior complexidade de desenvolvimento e infra
* Debug é mais complexo
* Comunicação entre serviços deve ser bem pensada para permitir monitoramento e ratreio de falhas
* Cada serviço usando uma tecnologia diferente pode ser um problema
* Monitoramento é mais complexo (e crucial)

## 1) Monolith First (Martin Fowler)
Uma abordagem muito utilizada é iniciar o projeto como monolito e só depois ir quebrando alguns módulos/partes em microsserviços

## 2) Tipos de microsserviços

### Data service
Fornece acesso direto a uma determinada fonte de dados
### Bussines service
Um aglomerado de data services. Além de expor dados também possui regras/lógica
### Translation service
Serviço de tradução que recebe requisições internas e as transforma em algo que uma api externa entenda
### Edge service
Permite fornecer um tipo de serviço para cada tipo de cliente
