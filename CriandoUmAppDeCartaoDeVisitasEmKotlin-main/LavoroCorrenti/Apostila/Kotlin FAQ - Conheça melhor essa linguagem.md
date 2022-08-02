# Kotlin: FAQ - Conheça melhor essa linguagem

## Entenda melhor a linguagem Kotlin e a relação dela com outras tecnologias como a JVM, iOS e JavaScript.

Por que eu devo ler este artigo:Este artigo é adequado para programadores que desejam conhecer aspectos gerais sobre Kotlin, tais como o que é, onde e como é utilizado. Com isso eles poderão se decidirem sobre essa linguagem.

Tire sua dúvidaMarcar como concluídoAnotar

[Artigos](https://www.devmedia.com.br/artigos/)[Kotlin](https://www.devmedia.com.br/artigos/kotlin)Kotlin: FAQ - Conheça melhor essa linguagem

### O que é Kotlin?

Kotlin é uma linguagem de tipagem estática. Por esse motivo, o tipo de uma variável sempre será checado antes do programa entrar em execução. Por exemplo, caso o programador escreva uma operação como “3” + 5 a compilação falhará. Isso ocorre porque para o compilador “3” é uma string e 5 é um número e, nesse contexto, a conversão entre esses tipos não será realizada automaticamente, sem o consentimento explícito do programador.

Em geral, linguagens de tipagem estática oferecem duas vantagens ao preço de um maior controle por parte do programador sobre o que ele está fazendo. A primeira delas é a segurança e a segunda a performance.

Quanto à segurança, linguagens como Kotlin tendem a deixar mais óbvios para o programador os erros que ele pode ter cometido involuntariamente durante a escrita do código, alertando sobre eles mais cedo, durante a compilação, em lugar de deixá-los para serem observados durante a execução do programa. Em contrapartida, linguagens de tipagem estática são mais rigorosas, não permitindo por exemplo que uma variável troque de tipo, como vemos no **Código 1**. Contudo, isso acaba sendo benéfico, uma vez que evita defeitos não intencionais e que podem levar a falhas ou ao comportamento imprevisível do sistema.

```
var num = 123
num = ‘123’
```

**Código 1**. Exemplo de defeito onde uma variável trocaria de tipo

Em algumas linguagens, a operação “3” + 5 seria possível, produzindo os resultados "35", texto, ou 35, número. Ao fazer isso, uma vez que o tipo do resultado depende de alguns fatores avaliados durante a execução do programa, essas linguagens tornam possível o comportamento imprevisível do sistema, levando a anomalias como NaN (o resultado de uma operação aritmética que não produz um número), entre outras. Em linguagens como Kotlin, esses problemas são diagnosticados pelo compilador e revolvidos mais cedo.

![Às vezes é melhor falhar antes](https://devmedia.com.br/arquivos/Salas/Linguagem/kotlin/40754/40754.jpg)**Figura 1**. Às vezes é melhor falhar antes

Além disso, o conhecimento prévio dos tipos permite a otimização do código de máquina. Por esse motivo linguagens de tipagem estática tendem a ser mais performáticas, uma vez que a checagem dos tipos não precisa ser feita enquanto o programa está em execução.

### Para o que Kotlin é utilizada?

Kotlin possui basicamente três alvos: a JVM, JavaScript e o nativo.

Ao ser compatível com a JVM (a máquina virtual do Java), Kotlin se torna viável como linguagem para aplicações web, Android e Desktop, assim como Java. Isso também quer dizer que podemos utilizar Java e Kotlin em um mesmo projeto. Mais abaixo neste artigo temos o exemplo de um código Spring escrito em Kotlin.

Estima-se que com Kotlin a quantidade de código escrito cai cerca de 40% em relação ao Java. Por exemplo, no código abaixo temos uma mesma classe escrita em Java e Kotlin, note a diferença.

```
// Pessoa.java
public class PessoaJava {

    private String nome;
    private String sobrenome;

    public String getNome() {
        return nome;
    }

    public void setNome(String nome) {
        this.nome = nome;
    }

    public String getSobrenome() {
        return sobrenome;
    }

    public void setSobrenome(String sobrenome) {
        this.sobrenome = sobrenome;
    }
}

// Pessoa.kt
data class Pessoa(val nome: String, val sobrenome: String)
```

**Código 2**. Adicionando um elemento em um documento HTML

Kotlin possui classes e data classes. A diferença é que data classes entregam muito mais a partir de um código conciso. A partir do código data class Pessoa(val nome: String, val sobrenome: String) serão gerados, por exemplo, os métodos equals() e hashCode() automaticamente.

No Android, em especial, diversas alterações na API original foram distribuídas através da biblioteca [KTX](https://www.youtube.com/watch?v=TLksQq8T5v4&t=1219s) para que programadores Kotlin ficassem mais à vontade ao utilizar os recursos dessa linguagem.

Além da JVM, Kotlin também pode ser compilada para JavaScript através do Kotlin/Js. Além disso, ela possui uma biblioteca nativa chamada kotlinx.html, que torna a programação de aplicações front-end web mais fáceis de serem escritas com essa linguagem.

Por exemplo, o **Código 3** contém um exemplo que cria uma div e a adiciona ao documento dinamicamente, utilizando kotlinx.html.

```
val div = document.create.div {
    p {
        +"Olá mundo com Kotlin/Js"
    }

    img {
        src = "http://devmedia.com.br/logo"
    }
}

document.getElementById("root")?.appendChild(div)
```

**Código 3**. Adicionando um elemento em um documento HTML

[Kotlin Native](https://www.youtube.com/watch?v=h9GX5uUWiB8), o terceiro e último alvo, é um projeto em andamento que procura viabilizar a utilização do Kotlin em plataformas mais restritivas quanto a execução de máquinas virtuais, tais como o iOS. Atualmente esse projeto está disponível em estágio beta e o seu lançamento oficial ainda não possuía data quando este artigo foi escrito.

### Kotlin é orientada a objetos ou funcional?

Grande parte das linguagens de programação são ao mesmo tempo funcionais e orientadas a objetos atualmente e Kotlin não foge a essa regra, suportando ambos os paradigmas.

Em Kotlin é possível criar classes utilizando herança e polimorfismo, como vemos no **Código 4**. Contudo, no caso da herança existem certas restrições, algumas delas comuns a outras linguagens orientadas a objetos. Por exemplo, uma classe só pode herdar de uma única outra classe, mas pode implementar múltiplas interfaces.

```
class Bacon : Topping(), Addable, Removable
```

**Código 4**. Classe que extende uma outra e implementa duas interfaces

Ainda em relação a herança, classes base devem receber o modificador open (**Código 5**) ou não será possível para outras classes herdarem dela. Classes abstratas são open por padrão.

```
open class Topping()

abstract class Topping()
```

**Código 5**. Duas formas de fazer com que a classe Topping possa ser herdada por outra

Kotlin implementa a herança dessa forma porque entende que uma classe só deve ser herdada por outra se ela foi criada para isso. Apesar da herança ser um poderoso mecanismo para a transmissão de comportamento de um objeto para outro, evitando a repetição de código, ela também pode gerar problemas e aumentar o acoplamento acidentalmente quando utilizada da forma errada.

Em seu livro Effective Java, Joshua Bloch fala sobre esse princípio dando como exemplo os problemas encontrados pelos desenvolvedores da API de coleções do Java. Em um dos casos as classes pareciam ter um número errado de itens e começaram a falhar porque seus métodos não foram pensados para serem sobrescritos nas classes filhas. Esse é um sério caso em que a herança foi utilizada para reduzir a duplicação de código, contudo, como não foi previsto pelos criadores das classes originais que elas seriam utilizadas como classes base, o resultado foi diferente do esperado.

Falando sobre a programação funcional, Kotlin incorpora diversos recursos desse paradigma, sendo funções como valores de primeira classe um dos mais utilizados. Em Kotlin, funções podem ser armazenadas em variáveis e estruturas de dados, transmitidas via parâmetros e retornadas em funções de alta ordem.

Uma função de alta ordem é aquela que recebe outra função como parâmetro ou retorna uma função.

A função fold, utilizado em coleções, é um bom exemplo disso. Ela funciona como um [reducer](https://www.youtube.com/watch?v=zTZNpBP-OJI) recebendo dois parâmetros: primeiro o valor inicial e o segundo uma função acumuladora a ser invocada para cada item da coleção e vai somar os valores deles.

Por exemplo, considerando a coleção val itens = doubleArrayOf(110.0, 2255.0, 34.99), podemos utilizar fold para somar os valores 110.0, 2255.0, 34.99 de acordo com o **Código 6**.

```
val soma = itens.fold(0.0, { acc, i ->
    acc + i
})
```

**Código 6**. Utilizando a função fold para somar os itens em uma coleção

Neste exemplo, 0.0 é o valor inicial passado para a função acumuladora e que será utilizado como ponto de partida pelo acumulador acc. O segundo parâmetro de fold, { acc, i -> acc + i }, é essa função. Apesar da sintaxe aqui ser um pouco diferente do que vemos em outras linguagens, os parâmetros da função { acc, i -> acc + i } estão antes do sinal ->, separados por vírgula (acc, i), e o corpo após ele (acc + i).

Os parâmetros da função acumuladora são acc, valor acumulado, e i, item atual na coleção. Com esses dados ela realiza a operação acc + i e retorna esse valor. Em Kotlin, em muitos casos não precisamos utilizar a palavra-reservada return explicitamente, uma vez que as funções escritas dessa forma costumam retornar automaticamente a última instrução contida nelas. Por isso, o resultado da instrução acc + i é retornado e é utilizado por fold para aumentar o valor de acc e isso vai se repetindo para cada item na coleção

### Kotlin é difícil de aprender?

Kotlin possui uma sintaxe concisa, o que a torna fácil de aprender no início. Contudo, quanto menos código temos que escrever, de mais regras precisamos nos lembrar. Por isso ela possui uma carga idiomática forte, o que quer dizer que se passa boa parte do tempo aprendendo de que forma devemos escrever um código em Kotlin, após passados os primeiros momentos com a sua sintaxe básica.

Por exemplo, observe os **Códigos 7** e **8**. Ambos fazem a mesma coisa: verificam se nome é nulo e se for imprimem null, do contrário imprimem o valor de nome. O **Código 7** faz isso de forma mais familiar e o **Código 8** realiza essa operação utilizando mais recursos do Kotlin.

```
val nome: String? = null

if (nome == null) {
    println(null)
} else {
    println(nome)
}
```

**Código 7**. Verifica se nome é nulo e imprime null ou nome

```
val nome: String? = null

nome?.let {
    println(null)
} ?: println(nome)
```

**Código 8**. Verifica se nome é nulo e imprime null ou nome

Funções de escopo como let são muito utilizadas na programação em Kotlin, o que acaba mudando a forma do código escrito nessa linguagem quando o comparamos com outras.

Por exemplo, no **Código 9** é possível ver um exemplo de como alterar o estado de uma entidade em um código Kotlin extraído de uma aplicação Spring MVC/Data JPA. Não se preocupe em entendê-lo completamente, já que ele exige certo conhecimento sobre um framework específico, repare apenas na ordem das instruções, sobre as quais falamos mais abaixo.

```
return repository.findById(id)?.let { entity ->
    if (entity.status == Status.ACTIVE) {
        entity.status = Status.INACTIVE

        repository.save(entity).let {
            ResponseEntity.ok().body(assembler.toModel(entity))
        }
    }

    ResponseEntity.status(HttpStatus.METHOD_NOT_ALLOWED)
            .body(VndErrors.VndError("Method not allowed", "You can't inactivate an entity in this state"))
} ?: throw ResponseStatusException(HttpStatus.NOT_FOUND, "Resource not found")
```

**Código 9**. Alteração de estado de uma entidade em uma aplicação Spring/Kotlin

Primeiro usamos return e depois encadeamos uma sequência de instruções dentro de funções de escopo como let. O trecho ?.let na primeira linha significa se findById não retornar null e o trecho ?: na última linha significa “se foi retornado null”. Dentro de let a última instrução é sempre retornada, por isso vamos return apenas na primeira linha.

### Conclusão

A melhor forma de se chegar a algum lugar é dando um passo de cada vez. Então, se esse artigo instigou a sua curiosidade sobre Kotlin, siga para a [primeira aula dessa matéria](https://www.devmedia.com.br/carreira/linguagens/kotlin/). Lá você verá passa a passo como preparar o seu PC para executar código Kotlin, bem como escrever seus próprios programas nessa linguagem.