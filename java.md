# Java

Extensão do arquivo: .java

IDEs:
- Eclipse
- IntelliJ IDEA
- NetBeans

## 1) Sintaxe básica

Hello World em Java
```java
class MeuPrograma {
    public static void main(String[] args) {
        System.out.println("Hello World!");
    }
}
```

Comentários
```java
//comentário de uma linha

/*
comentário de várias linhas
*/
```
Imprimindo na tela
```java
System.out.println("Bom dia " + nome);
```

Pacotes e imports
```java
//definindo o pacote da classe
package br.com.nomedaempresa.nomedoprojeto.subpacote;

//importando uma classe pública de outro pacote
import java.util.ArrayList

//importando um pacote inteiro
import java.util.*;
```
Variáveis
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

Array (tamanho fixo)
```java
//declarando um array
int[] valores = new int[5];

//acessando uma posição do array
int valor = valores[0];

//consultando o tamanho de um array
int tamanho = valores.lenght;
```
### Estruturas de decisão

### Estruturas de repetição

## 2) Orientação a objetos

Classe

Herança

Classe abstrata

Classe anônima

Interface

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
