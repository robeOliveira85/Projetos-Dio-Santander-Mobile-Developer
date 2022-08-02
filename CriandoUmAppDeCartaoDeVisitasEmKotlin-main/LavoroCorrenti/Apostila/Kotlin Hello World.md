# Kotlin: Hello World

## Este artigo é um passo a passo sobre como criar um primeiro projeto Kotlin no IntelliJ IDEA que imprime a mensagem Olá mundo! no terminal.

Por que eu devo ler este artigo:O IntelliJ IDEA é um ambiente de desenvolvimento totalmente funcional para Kotlin. Por isso, caso você já tenha experiência com essa IDE, basta adicionar um primeiro arquivo Kotlin em seu projeto com a opção New → Kotlin File/Class para receber toda a infraestrutura necessária para programar com essa linguagem. Não sendo esse o caso, neste artigo explicarei em detalhes como criar um projeto e **executar um primeiro código em Kotlin**.

Tire sua dúvidaMarcar como concluídoAnotar

[Artigos](https://www.devmedia.com.br/artigos/)[Kotlin](https://www.devmedia.com.br/artigos/kotlin)Kotlin: Hello World

Neste artigo veremos passo a passo como criar um primeiro projeto Kotlin no IntelliJ IDEA e executar um código que imprime a mensagem Olá mundo! no terminal.

IntelliJ IDEA é uma IDE da JetBrains reconhecida pela comunidade de programadores e é base para o Android Studio, da Google. Dado que **Kotlin é a principal linguagem para a programação nativa para Android**, juntamente com [Java](https://www.devmedia.com.br/guia/linguagem-java/38169), escolhemos a IntelliJ IDEA como ambiente de estudos para essa linguagem.

Atualmente, o IntelliJ IDEA é distribuído gratuitamente em sua versão Community. No link abaixo você pode ver um passo a passo de como baixá-la, bem como realizar as configurações iniciais:
[Preparando o ambiente de desenvolvimento](https://www.devmedia.com.br/curso/linguagem-java-hello-word/367)

### Criando um projeto Kotlin no IntelliJ IDEA

Para criar um novo projeto na IntelliJ IDEA Community Edition (**Figura 1**) abra a IDE e clique no botão Create New Projeto.

![Tela de boas-vindas](https://www.devmedia.com.br/arquivos/Artigos/40670/fig1.png)**Figura 1**. Tela de boas-vindas

Na tela que se abrirá (**Figura 2**), selecione Kotlin na lista ao lado esquerdo para que se abram os tipos de projeto que podem ser criados com essa linguagem. Escolha a opção JVM | IDEA para criar um projeto Kotlin para a JVM.

Atualmente o código Kotlin pode ser compilado tendo como alvo a [JVM](https://www.devmedia.com.br/introducao-ao-java-virtual-machine-jvm/27624), através da geração de bytecode, projetos web, onde Kotlin se transforma em [JavaScript](https://www.devmedia.com.br/guia/javascript/34372) após ser transpilado, ou mesmo uma plataforma nativa como Windows, Linux ou MacOS.

![Tela de seleção do tipo de projeto](https://www.devmedia.com.br/arquivos/Artigos/40670/fig2.png)**Figura 2**. Tela de seleção do tipo de projeto

Nesta nova tela dê um nome para o projeto, aqui usamos Hello World com Kotlin (**Figura 3**), escolha a versão do SDK e clique em **Finish**.

![tela de seleção do SDK](https://www.devmedia.com.br/arquivos/Artigos/40670/fig3.png)**Figura 3**. Tela de seleção do SDK

Caso nenhum SDK seja listado será preciso baixar o JDK e instalá-lo. Para isso faça o download [aqui](https://www.oracle.com/technetwork/java/javase/downloads/jdk13-downloads-5672538.html).

Após fazer isso, clique sobre o botão **…** ao lado da caixa de seleção do SDK e selecione o local onde o JDK foi instalado, conforme a **Figura 4**.

![Seleção do JDK](https://www.devmedia.com.br/arquivos/Artigos/40670/image4.png)**Figura 4**. Seleção do JDK

Após criar o projeto, o IntelliJ IDEA abrirá o editor exibindo uma dica e duas mensagem. A mensagem mais abaixo na **Figura 5** apenas comunica a adição do suporte ao Kotlin no projeto. O alerta acima dessa mensagem diz respeito a performance da IDE em relação ao Windows Defender, onde ela recomenda adicionar a pasta dos projetos na lista de exceções de escaneamento em tempo real.

![Editor aberto após o projeto ter sido criado](https://www.devmedia.com.br/arquivos/Artigos/40670/fig4.png)**Figura 5**. Editor aberto após o projeto ter sido criado

Para realizar essa adição clique no link **Fix…** na mensagem e depois no botão **Configure Automatically**, em destaque na **Figura 6**. Uma janela deverá bloquear o Windows até que a mudança seja confirmada.

![Configuração de performance para o Windows Defender](https://www.devmedia.com.br/arquivos/Artigos/40670/fig5.png)**Figura 6**. Configuração de performance para o Windows Defender

Para adicionar um novo arquivo ao projeto clique com o botão direito sobre a pasta **src** (**Figura 7**) e escolha a opção **New → Kotlin File/Class** .

![Adição de um novo arquivo no projeto](https://www.devmedia.com.br/arquivos/Artigos/40670/fig6.png)**Figura 7**. Adição de um novo arquivo no projeto

No diálogo que se abrirá escolha a opção **File** e de um nome para o arquivo. Não é necessário informar a extensão do arquivo, apenas o nome dele, conforme demonstra a **Figura 8**.

![Informar o nome do arquivo](https://www.devmedia.com.br/arquivos/Artigos/40670/fig7.png)**Figura 8**. Informar o nome do arquivo

A IDE abrirá o arquivo no editor após tê-lo criado (**Figura 9**).

![Editor aberto aguardando o código](https://www.devmedia.com.br/arquivos/Artigos/40670/fig8.png)**Figura 9**. Editor aberto aguardando o código

Nesse momento estamos prontos para escrever um primeiro código.

### Escrevendo e executando um código

Copie e cole o **Código 1** abaixo no editor para criar a função main que exibirá no terminal a mensagem Olá mundo!.

```
fun main() {
    println("Olá mundo!")
}
```

**Código 1**. Olá mundo com Kotlin

É na função main que se inicia a execução de um programa Kotlin.

Para executar o código temos diversas opções:

- Podemos usar o atalho **Ctrl + Shift + F10**;

- Clicar sobre o **Yellow Bulb 💡** (**Figura 8**) e selecionar a intention **Run Programa.kt**.

- ![Executando o código com o Yellow Bulb](https://www.devmedia.com.br/arquivos/Artigos/40670/fig9.png)**Figura 8**. Executando o código com o Yellow Bulb

- Clicar com o botão direito sobre o nome da função (

  Figura 9

  ) e escolher a opção

   

  Run programaKt

  .

  ![Executando o código com o menu de contexto](https://www.devmedia.com.br/arquivos/Artigos/40670/fig10.png)**Figura 9**. Executando o código com o menu de contexto

A compilação do código deve demorar um instante e pode ser acompanhada na barra de progresso na parte inferior direita da IDE (**Figura 10**).

![Compilação do programa](https://www.devmedia.com.br/arquivos/Artigos/40670/fig11.png)**Figura 10**. Compilação do programa

Após a compilação do programa ele será executado e a mensagem Olá mundo! será impressa no terminal (**Figura 11**).

![Mensagem Olá mundo! exibida no terminal](https://www.devmedia.com.br/arquivos/Artigos/40670/fig12.png)**Figura 11**. Mensagem Olá mundo! exibida no terminal

Com isso criamos e executamos um primeiro código Kotlin em um ambiente de desenvolvimento corretamente configurado.

### Conclusão

A partir daqui, os próximos passos envolvem aprender a **sintaxe da linguagem Kotlin**, o que você pode fazer acessando essa matéria: