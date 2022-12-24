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

Modificadores de acesso:
- public: acessível em qualquer lugar
- protected: acessível às classes do mesmo pacote e através de herança
- default (sem modificador explícito): acessível apenas para classes do mesmo pacote
- private: acessível apenas pela própria classe

### Atributo de classe (static)
```java
class Carro{
    public static int numeroDeCarros;
    ...
}

//o atributo ou método pode ser acessado diretamente da definição da classe
System.out.println(Carro.numeroDeCarros);
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
Uma classe que não pode ser instanciada (serve para definir métodos e atributos que são herdados por outras classes)

Uma classe abstrata pode possuir métodos "concretos"

Método abstrato: Um método que obrigatóriamente precisa ser implentado pela classe filha

```java
public abstract class ClasseAbstrata {
 	
	//um método abstrato exige que a classe seja abstrata
      public abstract void metodoAbstrato();

	//outros métodos e atributos
}
```
### Classe anônima
Permite a declaração e a instanciação de uma classe em uma única expressão. A classe não possui nome.

Indicada para quando for necessário usar uma classe local apenas uma vez.
```java
//pode dar 'new' em uma classe ou interface
Executavel acao = new Executavel() {
    @Override
    public void run() {
        ...
    }
};
```

### Interface
Possui apenas o "contrato" dos métodos que precisam ser implementados. O uso de interface ao invés de herança é amplamente aconselhado
```java
public interface Autenticavel {
    //contém apenas a definição do método
    void autenticar(int variavel); 
}

//a classe precisa implementar o método definido da interface
public class Usuario implements Autenticavel{
	void autenticar (int variavel){
	     //implementação do método
      }
}
```
Todas as variáveis declaradas na interface precisam ser "public static final"

No Java 8 é possível adicionar uma implementação padrão para o método de uma interface usando o modificador "default"

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
