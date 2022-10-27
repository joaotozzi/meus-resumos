# HTML 5

## 1) Estrutura básica de uma página html

O conteúdo que é exibido pelo navegador fica dentro da tag body
```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>Nome da página<title>
</head>
<body>
  <h1>Hello World</h1>
</body>
</html>
```

## 2) Tags de formatação do conteúdo da página (dentro da tag body)

Título (de h1 a h6 por ordem de importância)
```html
<h1>Titulo</h1>
```

Parágrafo
```html
<p>Isso é um parágrafo</p>
```

Comentário (não são exibidos no navegador)
```html
<!-- exemplo de comentário -->
```

Inserir imagem
```html
<img src="imagem.jpg" alt="Texto alternativo">
```

Link
```html
<a href='http://google.com'>Link para o Google</a>
```

Definindo uma seção/divisão do documento html
```html
<div class="container"></div>
```

## 3) Refenciando arquivos javascript e CSS

### Arquivos de estilo (CSS)

Fazer referência a um arquivo .css externo
```html
<head>
  <link rel="stylesheet" href="styles.css">
</head>
```

Definindo o CSS dentro do arquivo HTML
```html
<head>
<style>
  h1 {color:red;}
  p {color:blue;}
</style>
</head>
```

Definindo o estilo na própria tag HTML (atributo style)
```html
<h1 style="color:blue;text-align:center">Título</h1>
```

### Arquivos javascript

Fazer referência a um arquivo .js externo
```html
<script src="script.js"></script>
```

Incluindo código JS dentro do arquivo HTML
```html
<script>
	document.getElementById("demo").innerHTML = "Hello JavaScript!";
</script>
```

## 4) Listas

Lista não ordenada
```html
<ul>
  <li>Coffee</li>
  <li>Tea</li>
  <li>Milk</li>
</ul>
```

Lista ordenada
```html
<ol>
  <li>Coffee</li>
  <li>Tea</li>
  <li>Milk</li>
</ol>
```

## 5) Formulários

Exemplo de formulário básico
```html
<form action="/pagina-processa-formulario" method="post">
  <div>
    <label for="nome">Nome:</label>
    <input type="text" id="nome">
  </div>
  <div>
    <label for="email">E-mail:</label>
    <input type="email" id="email">
  </div>
  <div>
    <input type="submit" value="Enviar">
  </div>
</form>
```

### Inputs de formulário

Texto
```html
  <label for="nome">First name:</label>
  <input type="text" id="nome" name="Nome">
```

Radio Buttons (compatilham o mesmo nome no atributo "name")
```html
  <input type="radio" id="html" name="fav_language" value="HTML">
  <label for="html">HTML</label><br>

  <input type="radio" id="css" name="fav_language" value="CSS">
  <label for="css">CSS</label><br>

  <input type="radio" id="javascript" name="fav_language" value="JavaScript">
  <label for="javascript">JavaScript</label>
```

Outros tipos:
- type="password"
- type="email"
- type="date"
- type="checkbox"


### Botão
```html
<button type="button">Clique aqui</button>
```

## 6) Acessibilidade

### Tags semânticas (dão significado para o conteúdo do html para o navegador)

Section - um agrupamento temático de conteúdo, normalmente com um título (um capítulo, informação de contato...)
```html
<section>
	<h1>WWF</h1>
	<p>The World Wide Fund for Nature (WWF) is an international organization working on issues regarding the conservation, research and restoration of the environment, formerly named the World Wildlife Fund. WWF was founded in 1961.</p>
</section>
```

Article - um conteúdo independente e autocontido, que faz sentido por si só, e deve ser possível distribuí-lo independentemente do resto do site (posts de um blog, comentários)
```html
<article>
	<h2>Google Chrome</h2>
	<p>Google Chrome is a web browser developed by Google, released in 2008. Chrome is the world's most popular web browser today!</p>
</article>
```

Header - para armazenar conteúdo introdutório
```html
<article>
  <header>
    <h1>What Does WWF Do?</h1>
    <p>WWF's mission:</p>
  </header>
  <p>WWF's mission is to stop the degradation of our planet's natural environment,
  and build a future in which humans live in harmony with nature.</p>
</article>
```

Footer - rodapé de um documento ou seção
```html
<footer>
  <p>Author: Hege Refsnes</p>
  <p><a href="mailto:hege@example.com">hege@example.com</a></p>
</footer>
```

Nav - para conter os conjuntos de links de navegação principais
```html
<nav>
  <a href="/html/">HTML</a> |
  <a href="/css/">CSS</a> |
  <a href="/js/">JavaScript</a> |
  <a href="/jquery/">jQuery</a>
</nav>
```

Aside - conteúdo além do conteúdo principal (uma barra lateral)
```html
<p>My family and I visited The Epcot center this summer. The weather was nice, and Epcot was amazing! I had a great summer together with my family!</p>

<aside>
<h4>Epcot Center</h4>
<p>Epcot is a theme park at Walt Disney World Resort featuring exciting attractions, international pavilions, award-winning fireworks and seasonal special events.</p>
</aside>
```

### figure and figcaption
```html
<figure>
  <img src="pic_trulli.jpg" alt="Trulli">
  <figcaption>Fig1. - Trulli, Puglia, Italy.</figcaption>
</figure>
```
