# Vue.js 3

Framework Javascript para criação de interfaces de usuário web

## 1) Utilizando Vue CLI

Instalação (necessário Node.js)
```
npm install -g @vue/cli
```

Criando um novo projeto
```
vue create <nome_projeto>
```

Rodando o projeto
```
cd <nome_projeto>
npm run serve
``` 

A aplicação inicia por padrão em http://localhost:8080


## 2) Single-File Components (SFC)

Encapsula o template, a lógica e o estilo em um único arquivo (extensão .vue)

O componente principal da aplicação é o "App.vue"

Estrutura básica de um componente
```html
<template>
  <h1> Olá Mundo </h1>
</template>

<script>
  export default {
    name: 'App'
  }
</script>

<style>
  h1 {
    font-weight: bold;
  }
</style>
```

### Utilizando outro componente
Além de inserir a tag no template é necessário importar o componente e declará-lo no javascript
```html
<template>
  <PrimeiroComponente/>
</template>

<script>
  import PrimeiroCompoente from '.src/components/PrimeiroCompoente.vue'
  
  export default {
    name: 'App'
	components: {
	  PrimeiroComponente
	}
  }
</script>
``` 

## 3) Data Biding

### Exibindo dados
Adicionar o nome da váriável entre chaves duplas. O valor é atualizado sempre que a variável for alterada.
```html
<template>
  <h1> {{ message }} </h1>
</template>

<script>
  export default {
    name: 'App',
	data() {
	  return {
	    message: 'Olá Mundo!'
	  }
	}
  }
</script>
```
