# Kotlin: Funções

## Mesmo um objeto complexo é composto por pequenas funções e Kotlin oferece diversos recursos para escrevermos elas, os quais serão abordadas neste artigo.

Por que eu devo ler este artigo:Kotlin é uma linguagem que foi apresentada ao mundo ao ser adotada pela Google para o sistema operacional Android e, posteriormente, pela Pivotal para o Spring. Logo em suas primeiras versões ela mostrou estar à altura das necessidades de grandes aplicações, oferecendo uma sintaxe concisa e com diversos recursos. Hoje estudaremos aqueles relacionados às funções.

Tire sua dúvidaMarcar como concluídoAnotar

[Artigos](https://www.devmedia.com.br/artigos/)[Kotlin](https://www.devmedia.com.br/artigos/kotlin)Kotlin: Funções

Uma função representa uma pequena rotina dentro de um programa.

### Ponto de entrada

O ponto de entrada de uma aplicação Kotlin é a função main. Abaixo, no **Código 1**, vemos um exemplo de como escrevê-la.

```
fun main(args: Array<String>) {
  println("Olá mundo")
}
```

**Código 1**. Declaração da função main

Apesar de simples, esse código revela muito da sintaxe do Kotlin e vamos destacar alguns detalhes:

1. Devemos usar a palavra-chave fun sempre que precisarmos de uma função ou método de classe.
2. O nome de uma variável é declarado antes do seu tipo de dado.
3. É permitida a declaração de funções em nível de arquivo, sem que elas precisem estar dentro de uma classe.

Ao executar esse código a função println exibirá na tela o texto Olá mundo.

### Parâmetros

Kotlin usa uma notação para parâmetros chamada **pascal notation**, que possui a sintaxe nome : tipo. A função main descrita no **Código 1** possui apenas um parâmetro, cujo nome é args por padrão.