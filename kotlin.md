# Kotlin

Linguagem da JetBrains que roda na JVM e pode ser usada como uma alternativa ao Java para desenvolvimento Android. 
Possui total interoperabilidade com Java (todos os códigos escritos em Java podem ser usados/chamados em Kotlin).


### Plataforma online para executar código em Kotlin 
[https://try.kotlinlang.org](https://try.kotlinlang.org)

## 1) Estrutura básica
Não é obrigatório usar ";" ao final da linha.

### Extensão do arquivo
.kt

### Ponto de partida de um programa Kotlin (função main)
```kotlin
fun main(args: Array<String>){
    //resto do código...
}
```

### Imprimir no console
```kotlin
println("Hello World!")
```

### Comentários
```kotlin
//comentário de uma linha

/*
comentário
com várias linhas
*/
```

## 2) Variáveis
Declaração de variáveis usa a palavra-chave "var". É obrigatório que a variável tenha um valor inicial.
```kotlin
var idade:Int = 22
```
Não é necessário informar o tipo (inferred type)
```kotlin
var idade = 22
```

Tipos de variáveis:
* Byte (numérico de 8 bits)
* Short (numérico de 16 bits)
* Int (numérico de 32 bits)
* Long numérico de 64 bits)
* Float
* Double
* Char
* String
* Boolean

Variáveis imutáveis (constantes) usam a palavra-chave "val"
```kotlin
val pi = 3.1415926
```

### Proteção contra nulos (null safety)
Por padrão, nenhuma variável ou objeto pode ter valor nulo.
Ao adicionar o operador "?" a variável passa a aceitar valor nulo, mas o compilador obriga fazer validação antes de usá-la.
```kotlin
var nomeUsuario :String? = null

if (nomeUsuario	!= null){
    println(nomeUsuario.length)
}
``` 

A validação pode ser substituída pelo operador "?" (safe call), que ignora a chamada se a variável for nula
```
println(nomeUsuario?.length)
```

### Variáveis são objetos
Tudo é um objeto em Kotlin. Todas as variáveis e tipos possuem propriedades e métodos.
```kotlin
val x: Int = 10
var y: Double =	x.toDouble()
```

### Facilitando a legibilidade de variáveis numéricas
Pode ser usado underline para separar os milhares
```kotlin
val umMilhao = 1_000_000
```
## 2) Strings

### Template
Usar "$" para inserir o valor de uma variável em uma string
```kotlin
val nomeUsuario = "João"				
val saudacao = "Bem-vindo, $nomeUsuario"
```

### String com mais de uma linha
```kotlin
val text = """
    Exemplo de texto
    com mais de uma
    linha
"""
```

## 3) Operações booleanas
Operadores booleanos:
* E - &&
* OU - ||
* NEGAÇÃO - !

### Funções de operações booleanas
```kotlin
val b1 = true
val b2 = false

val c1 = b1.and(b2)
val c2 = b1.or(b2)
val c3 = b1.not()
```

## 4) Listas e Arrays
Definindo uma array (tamanho imutável)
```kotlin
val arrayInt :Array<Int> = arrayOf(1, 2, 3, 4)
```
Acessando uma posição (inicia na posição 0)
```kotlin
val x = arrayInt[2]	
```
Lista de tamanho mutável
```kotlin
val lista = mutableListOf(1,2,3,4)	
lista.add(5)
```
Lista com tamanho e valores imutáveis
```kotlin
val lista = listOf(1,2,3,4)
```

### Funções úteis de listas
Retornar o primeiro elemento da lista
```kotlin
val item = lista.first()
```
Retornar o último elemento da lista
```kotlin
val item = lista.last()
```
Aplicar um filtro específico na lista
```kotlin
val numerosPares = lista.filter { it % 2 == 0 }
```

## 5) Estruturas de decisão

If e Else
```kotlin
if(senha == "123"){
    //conteúdo do if
}else{
    //conteúdo do else
}
```
When (Equivalente ao switch do Java)
É possível verificar múltiplos valores ou até um intervalo de valores (in 1..10)
```kotlin
when (x) {
    0, 1 -> print("x == 0 ou x == 1")
	else ->	print("x tem outro valor")
}
```

## 6) Estruturas de repetição
For
```kotlin
val lista = listOf(1,2,3,4)
for(i in lista)	{
    println("Item: $i")
}
```
For com índice e valor
```kotlin
val lista = listOf(1,2,3,4)
for((indice, valor) in lista.withIndex()){
    println("índice: $indice valor: $valor")
}
```

While
```kotlin
var x = 0	
while (x < 10) {
    println(x.toString())
    x++
}
```

## 7) Funções
Utiliza a palavra-chave "fun". Para funções que não precisam de retorno, informar o retorno como "Unit" ou omitir o tipo de retorno.
```kotlin
fun somar(n1: Int, n2: Int): Int{
    return n1 + n2	
}

val resultado = somar(5, 7)
```

### Single-Expression Functions
Função em uma linha. Não é preciso usar a palavre "return"
```kotlin
fun somar(n1: Int, n2: Int) = n1 + n2	
```

## 8) Orientação a Objetos

```kotlin
class Carro {
    var cor: String = ""
    var modelo:String =	""
	
    fun	acelerar(){
        println("Acelerando")
    }
	
    fun	frear(){
        println("freando")
    }
}

val c = Carro()
c.cor = "Azul"
c.modelo = "Nissan 350z"
c.acelerar()
```

### Herança
A classe pai precisa ser definida como "open class" para poder ser herdada
```kotlin
open class Carro{
    //implemantação da classe pai
}

class CarroEspecial : Carro(){
    //implementação da classe filha
}
```

### Classe de Dados (Data Class)
Classes que não possuem métodos, apenas propriedades, podem ser definidas de forma simplificada
```kotlin
data class Usuario(var nome:String, var email:String, var senha:String)

val usuario = Usuario("João", "joao@email.com", "123")
```
