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

Estrutura básica de um componente:
```html
<template>

</template>

<script>

</script>

<style>

</style>
```

Todo conteúdo do template deve estar envolvido em um única tag (div, por exemplo).

### Utilizando outro componente
Além de inserir a tag no template é necessário importar o componente e declará-lo no javascript
```html
<template>
  <PrimeiroComponente/>
</template>

<script>
  import PrimeiroComponente from '.src/components/PrimeiroComponente.vue'
  
  export default {
    name: 'App'
	components: {
	  PrimeiroComponente
	}
  }
</script>
``` 

## 3) Data Biding

Mustache/Interpolação - Exibe o valor da variável (o texto mudará de forma reativa sempre que o valor da variável for alterado)
```
<p> {{ message }}</p>
```

v-bind - para fazer o data biding em atributos de tags html (pode ser abreviado para ":" no lugar de "v-bind:")
```
<div v-bind:id="'list-' + dynamicId"></div>
```

v-model - para interligar os dados de formulários (as alterações no input são refletidas na variável e em todos os lugares que usam aquele valor)
```
<input v-model="message" placeholder="Me edite">
<p>A mensagem é: {{ message }}</p>
```

## 4) Diretivas

If
```
<p v-if="seen">Agora você me viu</p>
```

For
```
<div v-for="(item, index) in items"></div>
```

v-on - dispara uma função quando um determinado evento acontece (pode ser abreviado para "@" no lugar de "v-on:") 
```
<a v-on:click="doSomething"> ... </a>
```

## 5) Javascript so Componente

### Options API
variáveis são definidas dentro da função data.
```
<script>
//imports

export default {
  name: "App",
  components: { CardUser, Loading },
  data(){
    return {
	  username: "",
	  showCard: false
	}
  },
  methods: {
    getAllUserData() {
	  this.showCard = true;
	}
  }
}
</script>
```

### Composition API
Disponível no Vue.js 3.
```
<script setup>
  import { ref } from 'vue';
  //outros imports
  
  const userName = ref("");
  let showCard = ref(false);
  
  function getAllUserData() {
	showCard.value = true;
  }
</script>
```

## 6) Vue Router


## 7) Requisições HTTP com AXIOS