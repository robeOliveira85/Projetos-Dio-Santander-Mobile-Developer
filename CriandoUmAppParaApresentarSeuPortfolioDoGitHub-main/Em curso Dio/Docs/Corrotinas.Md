# Corrotinas do Kotlin no Android

 







Uma *corrotina* é um padrão de projeto de simultaneidade que você pode usar no Android para simplificar o código que é executado de forma assíncrona. [Corrotinas](https://kotlinlang.org/docs/reference/coroutines/coroutines-guide.html) foram adicionadas ao Kotlin na versão 1.3 e são baseadas em conceitos estabelecidos de outras linguagens (link em inglês).

No Android, as corrotinas ajudam a gerenciar tarefas de longa duração que podem bloquear a linha de execução principal e fazer com que seu app pare de responder. Mais de 50% dos desenvolvedores profissionais que usam corrotinas notaram um aumento na produtividade. Este tópico descreve como você pode usar corrotinas do Kotlin para resolver esses problemas, permitindo criar um código de app mais simples e conciso.

## Recursos

As corrotinas são nossa solução recomendada para programação assíncrona no Android. Os recursos notáveis incluem o seguinte:

- **Leve**: é possível executar muitas corrotinas em uma única linha de execução devido à compatibilidade com a [*suspensão*](https://kotlinlang.org/docs/reference/coroutines/basics.html) (link em inglês), que não bloqueia a linha de execução em que a corrotina está sendo executada. A suspensão economiza memória em vez de bloquear, oferecendo compatibilidade com muitas operações simultâneas.
- **Menos vazamentos de memória**: use [*simultaneidade estruturada*](https://kotlinlang.org/docs/reference/coroutines/basics.html#structured-concurrency) (link em inglês) para executar operações em um escopo.
- **Suporte de cancelamento integrado**: o [cancelamento](https://kotlinlang.org/docs/reference/coroutines/cancellation-and-timeouts.html) (link em inglês) é propagado automaticamente por meio da hierarquia da corrotina em execução.
- **Integração com o Jetpack**: muitas bibliotecas do Jetpack incluem [extensões](https://developer.android.com/kotlin/ktx?hl=pt-br) que oferecem compatibilidade total com corrotinas. Algumas bibliotecas também fornecem o próprio [escopo de corrotina](https://developer.android.com/topic/libraries/architecture/coroutines?hl=pt-br) que pode ser usado para simultaneidade estruturada.

## Visão geral dos exemplos

Com base no [Guia para a arquitetura do app](https://developer.android.com/jetpack/docs/guide?hl=pt-br), os exemplos neste tópico fazem uma solicitação de rede e retornam o resultado para a linha de execução principal, onde o app pode exibir o resultado para o usuário.

Especificamente, o componente de arquitetura [`ViewModel`](https://developer.android.com/topic/libraries/architecture/viewmodel?hl=pt-br) chama a camada repositória na linha de execução principal para acionar a solicitação de rede. Este guia itera várias soluções que usam corrotinas para manter a linha de execução principal desbloqueada.

`ViewModel` inclui um conjunto de extensões KTX que funcionam diretamente com corrotinas. Essas extensões são uma [biblioteca `lifecycle-viewmodel-ktx`](https://developer.android.com/kotlin/ktx?hl=pt-br#viewmodel) e são usadas neste guia.

## Informações de dependência

Para usar corrotinas no seu projeto Android, adicione a seguinte dependência ao arquivo `build.gradle` do app:

[Groovy](https://developer.android.com/kotlin/coroutines?hl=pt-br#groovy)[Kotlin](https://developer.android.com/kotlin/coroutines?hl=pt-br#kotlin)

```kotlin
dependencies {
    implementation("org.jetbrains.kotlinx:kotlinx-coroutines-android:1.3.9")
}
```

## Executar em uma linha de execução em segundo plano

Fazer uma solicitação de rede na linha de execução principal faz com que ela aguarde ou *fique bloqueada*, até receber uma resposta. Como a linha de execução está bloqueada, o SO não pode chamar `onDraw()`, o que faz com que seu aplicativo congele e potencialmente leve a uma caixa de diálogo "O app não está respondendo" (ANR, na sigla em inglês). Para uma melhor experiência do usuário, vamos executar essa operação em uma linha de execução em segundo plano.

Primeiro, confira a classe `Repository` e veja como ela está fazendo a solicitação de rede:

```kotlin
sealed class Result<out R> {
    data class Success<out T>(val data: T) : Result<T>()
    data class Error(val exception: Exception) : Result<Nothing>()
}

class LoginRepository(private val responseParser: LoginResponseParser) {
    private const val loginUrl = "https://example.com/login"

    // Function that makes the network request, blocking the current thread
    fun makeLoginRequest(
        jsonBody: String
    ): Result<LoginResponse> {
        val url = URL(loginUrl)
        (url.openConnection() as? HttpURLConnection)?.run {
            requestMethod = "POST"
            setRequestProperty("Content-Type", "application/json; utf-8")
            setRequestProperty("Accept", "application/json")
            doOutput = true
            outputStream.write(jsonBody.toByteArray())
            return Result.Success(responseParser.parse(inputStream))
        }
        return Result.Error(Exception("Cannot open HttpURLConnection"))
    }
}
```

`makeLoginRequest` é síncrono e bloqueia a linha de execução de chamada. Para modelar a resposta da solicitação de rede, temos nossa própria classe `Result`.

O `ViewModel` aciona a solicitação de rede quando o usuário clica, por exemplo, em um botão:

```kotlin
class LoginViewModel(
    private val loginRepository: LoginRepository
): ViewModel() {

    fun login(username: String, token: String) {
        val jsonBody = "{ username: \"$username\", token: \"$token\"}"
        loginRepository.makeLoginRequest(jsonBody)
    }
}
```

Com o código anterior, `LoginViewModel` está bloqueando a linha de execução de IU ao fazer a solicitação de rede. A solução mais simples para retirar a execução da linha de execução principal é criar uma nova corrotina e executar a solicitação de rede em uma linha de execução de E/S:

```kotlin
class LoginViewModel(
    private val loginRepository: LoginRepository
): ViewModel() {

    fun login(username: String, token: String) {
        // Create a new coroutine to move the execution off the UI thread
        viewModelScope.launch(Dispatchers.IO) {
            val jsonBody = "{ username: \"$username\", token: \"$token\"}"
            loginRepository.makeLoginRequest(jsonBody)
        }
    }
}
```

Vamos separar o código de corrotinas na função `login`:

- `viewModelScope` é um `CoroutineScope` predefinido que está incluído nas extensões KTX `ViewModel`. Todas as corrotinas precisam ser executadas em um escopo. Um `CoroutineScope` gerencia uma ou mais corrotinas relacionadas.
- `launch` é uma função que cria uma corrotina e envia a execução do corpo funcional para o agente correspondente.
- `Dispatchers.IO` indica que essa corrotina deve ser executada em uma linha de execução reservada para operações de E/S.

A função `login` é executada da seguinte maneira:

- O aplicativo chama a função `login` da camada `View` na linha de execução principal.
- `launch` cria uma nova corrotina, e a solicitação de rede é feita independentemente em uma linha de execução reservada para operações de E/S.
- Enquanto a corrotina está em execução, a função `login` continua a execução e retorna, possivelmente antes que a solicitação de rede seja concluída. Para simplificar, a resposta da rede é ignorada por enquanto.

Como essa corrotina é iniciada com `viewModelScope`, ela é executada no escopo do `ViewModel`. Se o `ViewModel` for destruído porque o usuário está navegando para fora da tela, `viewModelScope` será cancelado automaticamente, e todas as corrotinas em execução também serão canceladas.

Um problema com o exemplo anterior é que qualquer coisa que chame `makeLoginRequest` precisa se lembrar de mover explicitamente a execução para fora da linha de execução principal. Vamos ver como podemos modificar o `Repository` para resolver esse problema.

## Usar corrotinas para a segurança principal

Consideramos uma função *muito segura* quando ela não bloqueia atualizações da IU na linha de execução principal. A função `makeLoginRequest` não é muito segura, porque chamar `makeLoginRequest` da linha de execução principal bloqueia a IU. Use a função `withContext()` da biblioteca de corrotinas para mover a execução de uma corrotina para uma linha de execução diferente:

```kotlin
class LoginRepository(...) {
    ...
    suspend fun makeLoginRequest(
        jsonBody: String
    ): Result<LoginResponse> {

        // Move the execution of the coroutine to the I/O dispatcher
        return withContext(Dispatchers.IO) {
            // Blocking network request code
        }
    }
}
```

`withContext(Dispatchers.IO)` move a execução da corrotina para uma linha de execução de E/S, tornando nossa função de chamada muito segura e permitindo que a IU seja atualizada conforme necessário.

`makeLoginRequest` também é marcado com a palavra-chave `suspend`. Essa palavra-chave é a maneira do Kotlin impor uma função a ser chamada de dentro de uma corrotina.

**Observação:** para facilitar os testes, recomendamos injetar `Dispatchers` em uma camada `Repository`. Para saber mais, veja [Testar corrotinas no Android](https://www.youtube.com/watch?v=KMb0Fs8rCRs&hl=pt-br) (vídeo legendado).

No exemplo a seguir, a corrotina é criada no `LoginViewModel`. À medida que `makeLoginRequest` move a execução para fora da linha de execução principal, a corrotina na função `login` agora pode ser executada na linha de execução principal:

```kotlin
class LoginViewModel(
    private val loginRepository: LoginRepository
): ViewModel() {

    fun login(username: String, token: String) {

        // Create a new coroutine on the UI thread
        viewModelScope.launch {
            val jsonBody = "{ username: \"$username\", token: \"$token\"}"

            // Make the network call and suspend execution until it finishes
            val result = loginRepository.makeLoginRequest(jsonBody)

            // Display result of the network request to the user
            when (result) {
                is Result.Success<LoginResponse> -> // Happy path
                else -> // Show error in UI
            }
        }
    }
}
```

Observe que a corrotina ainda é necessária aqui, já que `makeLoginRequest` é uma função `suspend`, e todas as funções `suspend` precisam ser executadas em uma corrotina.

Esse código é diferente do exemplo `login` anterior de algumas maneiras:

- `launch` não usa um parâmetro `Dispatchers.IO`. Quando você não transmite um `Dispatcher` para `launch`, todas as corrotinas iniciadas em `viewModelScope` são executadas na linha de execução principal.
- O resultado da solicitação de rede agora é processado para exibir a IU de sucesso ou falha.

A função de login agora é executada da seguinte maneira:

- O aplicativo chama a função `login()` da camada `View` na linha de execução principal.
- `launch` cria uma nova corrotina para fazer a solicitação de rede na linha de execução principal, e a corrotina inicia a execução.
- Dentro da corrotina, a chamada para `loginRepository.makeLoginRequest()` agora *suspende* a execução futura da corrotina até que o bloco `withContext` em `makeLoginRequest()` seja concluído.
- Quando o bloco `withContext` termina, a corrotina em `login()` retoma a execução *na linha de execução principal* com o resultado da solicitação de rede.

**Observação:** para se comunicar com o `View` da camada `ViewModel`, use `LiveData` conforme recomendado no [Guia para a arquitetura do app](https://developer.android.com/jetpack/docs/guide?hl=pt-br). Ao seguir esse padrão, o código no `ViewModel` é executado na linha de execução principal, para que você possa chamar a função `setValue()` do `MutableLiveData` diretamente.

## Como processar exceções

Para processar exceções que a camada `Repository` pode gerar, use o [suporte integrado para exceções](https://kotlinlang.org/docs/reference/exceptions.html) do Kotlin (link em inglês). No exemplo a seguir, usamos um bloco `try-catch`:

```kotlin
class LoginViewModel(
    private val loginRepository: LoginRepository
): ViewModel() {

    fun makeLoginRequest(username: String, token: String) {
        viewModelScope.launch {
            val jsonBody = "{ username: \"$username\", token: \"$token\"}"
            val result = try {
                loginRepository.makeLoginRequest(jsonBody)
            } catch(e: Exception) {
                Result.Error(Exception("Network request failed"))
            }
            when (result) {
                is Result.Success<LoginResponse> -> // Happy path
                else -> // Show error in UI
            }
        }
    }
}
```

Neste exemplo, qualquer exceção inesperada gerada pela chamada `makeLoginRequest()` é tratada como um erro na IU.

## Outros recursos de corrotinas

Para ter uma visão mais detalhada das corrotinas no Android, consulte [Melhorar o desempenho do app com corrotinas de Kotlin](https://developer.android.com/kotlin/coroutines/coroutines-adv?hl=pt-br).

Para ver mais recursos de corrotinas, consulte os seguintes links (em inglês):

- [Visão geral de corrotinas (JetBrains)](https://kotlinlang.org/docs/reference/coroutines-overview.html)
- [Sua primeira corrotina com Kotlin](https://kotlinlang.org/docs/tutorials/coroutines/coroutines-basic-jvm.html)
- [Outros recursos para corrotinas e fluxo Kotlin](https://developer.android.com/kotlin/coroutines/additional-resources?hl=pt-br)