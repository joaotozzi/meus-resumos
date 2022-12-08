# Desenvolvimento Android com Kotlin

IDE: [Android Studio](https://developer.android.com/studio)

## 1) Activity

Activity = uma "tela" da aplicação

A activity principal é normalmente chamada de MainActivity

### Arquivos que compõem uma Activity
* java/<nome do pacote>/NomeActivity - arquivo Kotlin/Java
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
