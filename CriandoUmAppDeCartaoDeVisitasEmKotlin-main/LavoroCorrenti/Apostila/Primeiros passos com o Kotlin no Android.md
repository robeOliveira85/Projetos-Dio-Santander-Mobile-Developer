# Primeiros passos com o Kotlin no Android

 







O [Android Studio](https://developer.android.com/studio?hl=pt) tem total compatibilidade com o Kotlin, assim você pode [criar novos projetos](https://developer.android.com/studio/projects/create-project?hl=pt) com arquivos Kotlin, [adicionar arquivos Kotlin](https://developer.android.com/kotlin/add-kotlin?hl=pt) ao seu projeto e [converter código de linguagem Java em Kotlin](https://developer.android.com/kotlin/add-kotlin?hl=pt#convert). Você pode usar todas as ferramentas atuais do Android Studio com o código Kotlin, incluindo conclusão de código, verificação de lint, refatoração, depuração e muito mais.



Não conhece a linguagem Kotlin? Confira estes links:

- [Aprender a linguagem Kotlin](https://developer.android.com/kotlin/learn?hl=pt): um curso intensivo de 30 minutos sobre as noções básicas de Kotlin.
- [Amostras de Kotlin](https://developer.android.com/samples?language=kotlin&hl=pt): uma biblioteca cada vez maior de amostras de apps para Android criados com Kotlin.
- [Outros recursos do Kotlin](https://developer.android.com/kotlin/resources?hl=pt): um conjunto selecionado de recursos com tudo sobre o Kotlin, incluindo amostras, codelabs, vídeos, livros e muito mais.

## Adicionar Kotlin a um app

Para adquirir habilidades e confiança no uso do Kotlin, recomendamos a seguinte abordagem:

1. Comece [criando testes](https://developer.android.com/studio/test?hl=pt#add_a_new_test) em Kotlin. Os testes são úteis para verificar a regressão de código e adicionam um nível de confiança durante a refatoração de código. Eles são úteis principalmente para a [conversão de código Java em Kotlin](https://developer.android.com/kotlin/add-kotlin?hl=pt#convert). Como os testes não são incluídos no app durante o empacotamento, eles são seguros para adicionar Kotlin à codebase.
2. Escreva um novo código em Kotlin. Antes de converter o código Java para o Kotlin, tente [adicionar pequenas partes do novo código Kotlin ao app](https://developer.android.com/kotlin/add-kotlin?hl=pt). Comece com uma classe pequena ou uma função auxiliar de nível superior. Adicione as anotações relevantes ao código Kotlin para garantir a interoperabilidade adequada com o código Java.
3. Atualize um código já existente para Kotlin. Quando você estiver confortável com a criação de código em Kotlin, converta um código Java para Kotlin. Extraia pequenas partes de funcionalidade Java e converta em classes e funções de nível superior do Kotlin.

O Android Studio também inclui um [conversor de código](https://developer.android.com/kotlin/add-kotlin?hl=pt#convert) que converte o código de um arquivo Java no Kotlin. Você também pode converter código Java para um arquivo Kotlin por meio da área de transferência.

## APIs do Android e exemplos de Kotlin

O Kotlin fornece [interoperabilidade completa com a linguagem Java](https://kotlinlang.org/docs/reference/java-interop.html), portanto, a chamada de APIs do Android costuma ser exatamente igual ao código Java correspondente. A diferença é que agora você pode combinar essas chamadas de método com os recursos de sintaxe do Kotlin.

Muitas APIs do Android estão disponíveis com referências idiomáticas em Kotlin. Para mais informações, consulte o [guia KTX](https://developer.android.com/kotlin/ktx?hl=pt) e a [documentação de referência do Kotlin no Android ](https://developer.android.com/reference/kotlin?hl=pt).

Veja alguns exemplos de como funciona uma chamada de APIs do Android no Kotlin, em comparação com o mesmo código na linguagem Java:

### Declarar uma atividade

[Kotlin](https://developer.android.com/kotlin/get-started?hl=pt#kotlin)[Java](https://developer.android.com/kotlin/get-started?hl=pt#java)

```java
class MyActivity : AppCompatActivity() {
  override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity)
  }
}
```

### Criar um listener de clique

[Kotlin](https://developer.android.com/kotlin/get-started?hl=pt#kotlin)[Java](https://developer.android.com/kotlin/get-started?hl=pt#java)

```kotlin
val fab = findViewById(R.id.fab) as FloatingActionButton
fab.setOnClickListener {
  ...
}
```

### Criar um listener de clique de item

[Kotlin](https://developer.android.com/kotlin/get-started?hl=pt#kotlin)[Java](https://developer.android.com/kotlin/get-started?hl=pt#java)

```kotlin
private val onNavigationItemSelectedListener
    = BottomNavigationView.OnNavigationItemSelectedListener { item ->
  when (item.itemId) {
    R.id.navigation_home -> {
      textMessage.setText(R.string.title_home)
      return@OnNavigationItemSelectedListener true
    }
    R.id.navigation_dashboard -> {
      textMessage.setText(R.string.title_dashboard)
      return@OnNavigationItemSelectedListener true
    }
 }
 false
}
```

## Práticas recomendadas

À medida que você ganhar fluência em Kotlin, siga estas diretrizes:

- Prefira a legibilidade, em vez de minimizar linhas de código. É muito fácil exagerar com a sintaxe mais leve do Kotlin.
- É recomendável estabelecer convenções e expressões idiomáticas de codificação que funcionem melhor para sua equipe. Os guias de estilo [Kotlin](http://kotlinlang.org/docs/reference/coding-conventions.html) (link em inglês) e [Kotlin para Android](https://android.github.io/kotlin-guides/) oferecem conselhos sobre como formatar o código Kotlin.