# Kotlin: Tour pela linguagem

## Esse artigo é uma introdução objetiva ao Kotlin para quem já programa e quer começar a escrever códigos com essa linguagem.

Por que eu devo ler este artigo:Kotlin é uma das linguagens mais utilizadas atualmente em aplicações web, mobile e desktop, dada a sua adoção por duas grandes empresas, a Google e a Pivotal, que mantém o projeto Spring. Este artigo é uma visão geral da sintaxe dessa linguagem e aborda elementos como variáveis, constantes, estruturas de controle de fluxo e funções.

[Artigos](https://www.devmedia.com.br/artigos/)[Kotlin](https://www.devmedia.com.br/artigos/kotlin)Kotlin: Tour pela linguagem

Kotlin é um dos projetos que mais crescem atualmente. Está presente no Android, iOS como alternativa ao Swift para a criação de frameworks nativos, e recebeu a sua própria biblioteca de concorrência, a Coroutines. Atualmente com o Spring Boot 2.2 também foi completamente integrada pela Pivotal aos frameworks da família Spring. Neste artigo falaremos sobre a sintaxe dela de forma resumida, mas suficiente para que alguém com um pouco de experiência em programação possa começar a escrever os seus próprios códigos com Kotlin.

### Ponto de entrada do programa

Em uma aplicação Kotlin a primeira função a ser invocada se chama main. Uma das formas de escrever essa função é a que vemos no **Código 1**.

```
fun main() {
      println(“Hello world”)
  }
```

**Código 1**. Função main sem parâmetro

A função println() imprime uma mensagem no terminal.

Falaremos mais sobre funções adiante.

### Comentários

Em Kotlin temos dois tipos de comentário: os de uma única linha e os de bloco.

Comentários de uma única linha (**Código 2**) são iniciados com // e se encerram no final da linha.

```
fun main() {
      // Utiliza a função println() para imprimir um texto no terminal
      println(“Hello world”)
  }
```

**Código 2**. Comentário de única linha