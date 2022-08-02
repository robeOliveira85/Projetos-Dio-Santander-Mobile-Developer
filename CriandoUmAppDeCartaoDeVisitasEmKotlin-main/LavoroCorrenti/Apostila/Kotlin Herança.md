# Kotlin: Herança

## Aqui você aprenderá como usar herança em Kotlin, algo que vemos a todo o momento ao lidar com linguagens orientadas a objetos e que permite criar novos tipos a partir de classes existentes, promovendo a reutilização de código.

Por que eu devo ler este artigo:Ao ler esse artigo você aprenderá **como usar herança em Kotlin** e estará mais bem preparado para escrever aplicações orientadas a objetos com essa linguagem, bem como entender o código presente em diversos frameworks e bibliotecas.



Tire sua dúvidaMarcar como concluídoAnotar

[Artigos](https://www.devmedia.com.br/artigos/)[Kotlin](https://www.devmedia.com.br/artigos/kotlin)Kotlin: Herança

#### Kotlin: Herança

- [Herança em Kotlin](https://www.devmedia.com.br/kotlin-heranca/40346#1)
- [Construtores secundários](https://www.devmedia.com.br/kotlin-heranca/40346#2)
- [Propriedades no Kotlin](https://www.devmedia.com.br/kotlin-heranca/40346#3)
- [Acessando uma implementação da superclasse](https://www.devmedia.com.br/kotlin-heranca/40346#4)
- [Classes abstratas e interfaces](https://www.devmedia.com.br/kotlin-heranca/40346#5)

#### Artigos a Série Kotlin

- [Kotlin: Classes](https://www.devmedia.com.br/kotlin-classes/40345)

**Herança é um dos três pilares da orientação a objetos**. É através desse recurso que podemos estender ou modificar o comportamento de um código existente, promovendo a reutilização. Nesse processo a classe existente é chamada de superclasse e a classe a ser criada se chama classe derivada ou subclasse. Uma subclasse herda todos os membros presentes na superclasse por padrão.

**A herança pode ocorrer em forma de generalização e especificação**. Quando um conjunto de classes, geralmente concretas, exigem uma superclasse, dizemos que as características que elas compartilham serão combinadas em uma superclasse generalizada. Considerando as classes hamburger e pizza, ambas produtos do cardápio de uma lanchonete, pensar que elas se generalizam na superclasse Food é logicamente plausível. De outra forma, pensar que Food se especializa em toppings, um tipo de alimento que geralmente acompanha um outro alimento, é natural. Apresentar como utilizar esses mecanismos em Kotlin é o objeto deste artigo.

### Herança

Toda *classe em Kotlin* possui um supertipo chamado Any, que possui apenas um pequeno número de funções, as quais são equals(), hashCode() e toString(). Dessa forma, ambas as declarações abaixo são sintaticamente equivalentes:

```
class Food(val price: Double)

class Food(val price: Double) : Any()
```

Para declarar um supertipo explicitamente devemos colocá-lo no cabeçalho da classe após dois pontos. Por padrão, classes em Kotlin são final, se nenhum outro modificador for utilizado em sua declaração. Isso impede que qualquer classes seja usada como supertipo de uma outra classe. Assim sendo, é necessário que a superclasse utilize o modificador open como primeiro passo para que uma outra classe herde da mesma.

```
open class Food(val price: Double)

class Hamburger(price: Double) : Food(price)
```

Kotlin não suporta herança múltipla, motivo pelo qual apenas uma classe pode ser incluída na lista de supertipos de uma outra classe.

Observe que no construtor primário da classe Hamburger não utilizamos a palavra reservada val antes do parâmetro price, o que fará com que a propriedade price em Food não seja sobrescrita. De fato, com essa sintaxe apenas recebemos um parâmetro no construtor primário de Hamburger e delegamos para o construtor primário de Food.

Note que embora a propriedade price seja final em Food, ela pode ser acessada através de uma instância de Hamburger, dessa forma:

```
val hamburger: Hamburger = Hamburger(2.89)

println("Total: ${hamburger.price}")
```

Esse é o comportamento natural para o modificador final. Caso seja necessário impedir que subclasses de Food acessem uma propriedade de uma superclasse, será preciso declará-la com um outro nível de acesso, usando os modificadores protected ou private.

### Construtores secundários

Na ocasião da superclasse não declarar um construtor primário, cada um dos seus construtores secundários deve ser inicializado pela subclasse utilizando a palavra-chave super em lugar de this, assim como demonstrado abaixo.

```
open class Food {
    open val price: Double

    constructor(price: Double) {
        this.price = price
    }
}

class Hamburger : Food {
    constructor(price: Double) : super(price)
}
```

A seguir, analisamos um novo exemplo, o qual evidencia o fato de uma subclasse poder referenciar quaisquer construtores secundários da superclasse.

```
open class Food {
    open var price: Double = 0.0
    open var name: String = ""

    constructor(price: Double) {
        this.price = price
    }

    constructor(price: Double, name: String) {
        this.price = price
        this.name = name
    }
}

class Hamburger : Food {
    constructor(price: Double) : super(price)

    constructor(price: Double, name: String) : super(price, name)
}
```

Dessa vez, na listagem de supertipos da classe Hamburger usamos a sintaxe Food em lugar de Food(), porque nesse contexto Food não possui um construtor primário.

### Propriedades

É possível sobrescrever uma propriedade da superclasse na subclasse utilizando a palavra reservada override. Contudo, assim como as classes, propriedades em Kotlin também são final por definição e não podem ser sobrescritas ou herdadas sem a substituição explícita do modificador final por open. Vemos um exemplo disso no código abaixo.

```
open class Food(open val price: Double)

class Hamburger(override val price: Double) : Food(price)
```

Mesmo sobrescrevendo a propriedade price na subclasse é necessário delegar ao construtor da superclasse.

Uma subclasse pode incluir em seu construtor primário suas próprias propriedades. Considere que além de Hamburger a aplicação também contenha outros subtipos de Food, os quais seguem a sua própria cadeia de especificação, a fim de implementar um sistema de pedido no qual o cliente possa escolher adicionar para a sua comida. Poderíamos modelar a classe Topping da seguinte forma:

```
open class Topping(price: Double, private val food: Food) : Food(price) {

    override val price: Double = price
        get() = field + food.price
}
```

A classe Topping, além de ser uma especificação de Food, também recebe um objeto do tipo Food como parâmetro em seu construtor. Uma vez que não desejamos sobrescrever a propriedade price, tratamos de receber um parâmetro no construtor, cujo valor é transferido para a propriedade através de delegação. Contudo, food é uma propriedade que existe apenas na subclasse e, por esse motivo, usamos o modificador private e a palavra reservada val para criar essa propriedade.

Apesar de a propriedade price não ser sobrescrita na subclasse, o seu getter é redefinido para retornar a instância de price no próprio objeto somada àquela presente na propriedade food.

Na linguagem Kotlin, o conceito de propriedade torna desnecessário escrever getters e setters, pois implicitamente os dados de um objeto são encapsulados através de métodos acessores implícitos. Contudo, se desejarmos sobrescrever o comportamento padrão de um método de acesso podemos usar essa sintaxe:

```
var <propriedade>[: <tipo>] [= <valor>]
    [<getter>]
    [<setter>]
```

Assim como Food, uma vez que Topping também é modificada com open, ela também pode ser utilizada como um supertipo. A título de exemplo, vamos declarar os subtipos Jalapeno e Mushroom, ambos derivados de Topping.

```
class Jalapeno(price: Double, food: Food) : Topping(price, food)

class Mushroom(price: Double, food: Food) : Topping(price, food)
```

Feito isso, podemos instanciar um objeto a partir da composição de toppings e foods, usando até mesmo o supertipo como tipo para o objeto criado, tal qual apresentado a seguir.

```
val food: Food = Mushroom(
    .10,
    Jalapeno(
        .15,
        Jalapeno(
            .15,
            Hamburger(2.99)
        )
    )
)
```

Em tempo de execução, o compilador saberá qual implementação da propriedade price invocar em uma relação de herança. Dessa forma, podemos passar uma instância de Hamburger em lugar de um Food, em primeiro lugar porque Hamburger é um Food. Num segundo momento, ocorrerá um processo chamado late binding, no qual a JVM saberá que tipo do objeto é Hamburger, executando assim a versão sobrecarregada da propriedade price desse objeto.

Assim, ao executar a linha abaixo, o valor apresentado no terminal será a soma de todas propriedades price:



```
println("Total: ${food.price}") // Total: 3.39
```

### Acessando uma implementação da superclasse

A palavra reservada super pode ser utilizada também para acessar funções da superclasse. No exemplo abaixo, usamos esse recurso para oferecer na subclasse uma função que calcula o valor de um alimento, considerando tanto uma taxa quanto um desconto.

```
open class Food(open val price: Double) {

    open fun calculate(fee: Double): Double {
        return price * fee
    }
}

class Hamburger(override val price: Double) : Food(price) {

    fun calculate(fee: Double, discount: Double): Double {
        return super.calculate(fee) - discount
    }
}
```

É importante notar que a função calculate(fee: Double, discount: Double) existe apenas na subclasse. Sendo assim, caso haja uma tentativa de invocá-lo a partir de uma instância de Food um erro será emitido pelo compilador com a seguinte mensagem.



```
val food: Food = Mushroom(
    .10,
    Jalapeno(
        .15,
        Jalapeno(
            .15,
            Hamburger(2.99)
        )
    )
)

println("Total: ${food.calculate(1.2, 0.2)}")

Error:(38, 43) Kotlin: Too many arguments for public open fun calculate(fee: Double):
Double defined in Food
```

### Classes abstratas e interfaces

Uma classe abstrata, assim como os seus métodos abstratos, é open por padrão. Ao herdar de uma classe abstrata, todos os métodos abstratos da superclasse devem ser implementados na subclasse, a não ser que a mesma também seja abstrata. Caso contrário, um erro será emitido pelo compilador:

```
abstract class Food(open val price: Double) {

    abstract fun calculate(fee: Double): Double
}

class Hamburger(price: Double) : Food(price)

Class 'Hamburger' is not abstract and does not implement abstract base class member
public abstract fun calculate(fee: Double): Double defined in Food
```

Em Kotlin também é possível que uma classe abstrata herde de uma classe não abstrata. Quando isso for necessário, os métodos da superclasse não abstratos devem utilizar o modificador open.

Ainda que isso seja incomum, há casos em que uma classe herda de uma outra que compartilha um membro com alguma interface, implementada pela subclasse. Quando isso acontecer, podemos usar uma forma qualificada de super para acessar a implementação da superclasse. Vejamos um exemplo que deixará isso mais claro:

```
open class Food {
    open fun calculate() { /* Alguma implementação aqui */ }
}

interface Calculable {
    fun calculate() { /* Alguma implementação aqui */ }
}

class Hamburger : Food(), Calculable {
    override fun calculate() {
      super<Food>.calculate()
      super<Calculable>.calculate()
    }
}
```

Via de regra, sempre que uma classe herdar membros de mesmo nome de supertipos diferentes, ela mesma deve fornecer uma implementação para esse membro. No exemplo acima, a classe Hamburger fornece uma implementação para calculate, a qual invoca calculate() de Food e Calculable a partir de super qualificado.

### Conclusão

Para aqueles que vêm de outras linguagens de programação, como o [Java](https://www.devmedia.com.br/guia/programador-java/37809), a relação de herança implícita Type : Any do Kotlin pode ser imediatamente transcrita para Type extends Object, uma vez que todo objeto em Java deriva de Object. Contudo essa relação não deve ser vista dessa forma, dado que apenas um limitado conjunto dos métodos presentes em Object podem ser encontrados também em Any. Quando usamos Kotlin para a [JVM](https://www.devmedia.com.br/introducao-ao-java-virtual-machine-jvm/27624), toda vez que for necessário usar um Object, o compilador se encarrega de fazer a tradução para Any e usará métodos de extensão para completar o conjunto de métodos em Any.