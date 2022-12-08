# Desenvolvimento Android com Kotlin

IDE: [Android Studio](https://developer.android.com/studio)

## 1) Activity

Activity = uma "tela" da aplicação

A activity principal é normalmente chamada de MainActivity

### Arquivos que compõem uma Activity
* java/{nome do pacote}/NomeActivity - arquivo Kotlin/Java
* res/layout/activity_nome.xml - arquivo XML de layout


### Exemplo de Activity - Arquivo Kotlin
É necessário herdar a classe Activity e sobrescrever o método de callback "onCreate"
```kotlin
import android.app.Activity
import android.os.Bundle

class MainActivity : Activity() {

    override fun onCreate (savedInstanceState : Bundle?) {
        super.onCreate(savedInstanceState)

	//definindo o conteúdo da tela
        setContentView( R.layout.activity_main )
    }
}
```

### Exemplo de Activity - Arquivo XML
Exemplo apenas com um componente TextView com o texto "Hello Kotlin"
```xml
<?xml version="1.0" encoding="utf-8"?>
<TextView xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:text="Hello Kotlin"
    />
```

Métodos de callback herdados da classe Activity:
* onCreate
* onStart
* onResume
* onPause
* onStop
* onRestart
* onDestroy

## 2) Classe R
Classe que facilita o acesso aos recursos do projeto

```kotlin
//acessar o layout activity_main.xml 
R.layout.activity_main

//acessar a imagem background.png
R.drawable.background

//acessar a cor azul
R.color.azul
```

## 3) Componentes visuais (Views)
Componentes visuais utilizados para construir as telas do aplicativo. Todos os componentes são derivados da classe View

TextView - para visualização de texto
```xml
<TextView	
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="Texto exemplo"
    />
```

EditText - para receber texto do usuário
```xml
<EditText
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:hint="Nome de usuário"
    />
```

Button - botão
```xml
<Button
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="cadastrar"
    />
```

### Altura e Largura das Views
É obrigatório definir altura (android:layout_height) e largura (android:layout_width) dos componentes

Pode ser definido como:
* Um valor fixo de densidade de pixels (ex: 130dp)
* wrap_content (largura/altura do conteúdo)
* match_parent (usa todo o espaço disponível)

## 4) Layouts

### LinearLayout
Agrupa os componentes dentro dele de forma linear, um ao lado do outro (na horizontal ou na vertical)
```xml
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:orientation="vertical">

<TextView ... />
	
<Button ... />

</LinearLayout>
```
