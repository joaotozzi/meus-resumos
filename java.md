# Java

Extensão do arquivo: .java

IDEs:
- Eclipse
- IntelliJ IDEA
- NetBeans

## 1) Sintaxe básica

### Hello World em Java
```java
class MeuPrograma {
    public static void main(String[] args) {
        System.out.println("Hello World!");
    }
}
```

### Comentários
```java
//comentário de uma linha

/*
comentário de várias linhas
*/
```
### Imprimindo na tela
```java
System.out.println("Bom dia " + nome);
```

### Pacotes e imports
```java
//definindo o pacote da classe (tudo em letra minúscula)
package br.com.nomedaempresa.nomedoprojeto.subpacote;

//importando uma classe pública de outro pacote
import java.util.ArrayList;

//importando um pacote inteiro
import java.util.*;
```
### Variáveis
```java
//tipos primitivos principais
int idade = 15;
double pi = 3.14;
boolean pago = true;
char letra = 'a';

//String é uma classe
String nome = "João";
```
Existem também os tipos primitivos byte, short, long e float

### Casting
```java
int valor = (int) 3.14;
```
A conversão de valores para uma variável maior é feita implicitamente

### Constantes (modificador "final")
```java
final int constante = 42;
```

### Array
O tamanho de um array é fixo, não pode ser alterado.
```java
//declarando um array
int[] valores = new int[5];

//acessando uma posição do array
int valor = valores[0];

//consultando o tamanho de um array
int tamanho = valores.lenght;
```
### Estruturas de decisão

If e else
```java
if (idade > 18) {
    //conteúdo do if
}else{
    //conteúdo do else
}
```

Switch
```java
switch(valor) {
  case 1:
    //bloco de código
    break;
  case 2:
    //bloco de código
    break;
  default:
    //bloco de código
}
```

Operador ternário

(expressão) ? retornoVerdadeiro : retornoFalso;
```java
b = (a > 0) ? 1 : 2;
```

### Estruturas de repetição

While
```java
int i = 0;
while (i < 5) {
  //bloco de código
  i++;
}
```

For
```java
for (int i = 0; i < 5; i++) {
  //bloco de código
}
```

Foreach (para percorrer uma lista)
```java
for(String item : lista) {
    //bloco de código
}
```

## 2) Orientação a objetos

### Classe
```java
public class Conta{
    //atributos
    private double saldo;
    private String titular;
    
    //construtor
    public Conta(){
       //...
    }
    
    //métodos
    public deposita(double valor){
        this.saldo += valor;
    }
    
    //métodos getters e setters
}
```

Instanciando um objeto da classe
```java
Conta conta = new Conta();
```

### Herança
```java
public class Gerente extends Funcionario{
    
    public Gerente (){
        //chama o construtor da classe pai
        super();
    }
    
    //indica que o método da classe pai foi sobrescrito
    @Override
    public void metodoExemplo (){
        //...
    }
}
```
### Classe abstrata
```java
```
### Classe anônima
```java
```
### Interface
```java
```
## 3) java.util

List

## 4) Tratamento de exceções

## 5) java.lang

Métodos da classe String

Classe Object

Wrappers

## 6) Java 8

## 7) Concorrência e threads

```java
```
