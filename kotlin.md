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
