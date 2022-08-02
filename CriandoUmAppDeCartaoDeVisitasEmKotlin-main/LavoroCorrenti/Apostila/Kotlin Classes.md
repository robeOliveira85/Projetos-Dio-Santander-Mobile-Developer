# Kotlin: Classes

## Ao concluir esse artigo você saberá como utilizar classes em Kotlin, evitando assim as armadilhas sintáticas dessa linguagem ao usar construtores e blocos de inicialização.

Por que eu devo ler este artigo:Essa é uma introdução às classes em Kotlin. Através de exemplos, apresentaremos neste artigo como escapar de algumas armadilhas ao trabalhar com classes e veremos como interpretar e corrigir erros comuns emitidos pelo compilador referentes a construtores e blocos de inicialização.

Tire sua dúvidaMarcar como concluídoAnotar

[Artigos](https://www.devmedia.com.br/artigos/)[Kotlin](https://www.devmedia.com.br/artigos/kotlin)Kotlin: Classes

#### Kotlin: Classes

- [Declaração de classes](https://www.devmedia.com.br/kotlin-classes/40345#1)
- [Construtores](https://www.devmedia.com.br/kotlin-classes/40345#2)
- [Construtores e propriedades](https://www.devmedia.com.br/kotlin-classes/40345#3)
- [Imutabilidade dos parâmetros dos construtores secundários](https://www.devmedia.com.br/kotlin-classes/40345#4)
- [Instanciando uma classe](https://www.devmedia.com.br/kotlin-classes/40345#5)
- [Funções membro](https://www.devmedia.com.br/kotlin-classes/40345#6)
- [Construtores e blocos de inicialização](https://www.devmedia.com.br/kotlin-classes/40345#7)
- [Níveis de acesso](https://www.devmedia.com.br/kotlin-classes/40345#8)

#### Artigos da Série Kotlin

- [Kotlin: Herança](https://www.devmedia.com.br/kotlin-heranca/40346)

Kotlin não é uma linguagem em que todo o código precisa estar contido em uma classe. Ela incorpora a programação procedural e funcional e, caso seja necessário, se pode utilizar os objetos nativos, implementando as regras de negócio do projeto com funções. Porém, quando trabalhamos de forma orientada a objetos nessa linguagem devemos nos atentar para os diversos recursos que ela dispõe para a escrita de classes, assunto tratado aqui.

**Uma classe é um bloco de construção de software fundamental, encontrado na grande maioria das linguagens orientadas a objetos e, dessa forma, também presente em Kotlin**. Elas são usadas para criar novos tipos de dados, utilizando uma estrutura composta por métodos e variáveis que podem possuir os seus próprios tipos.

Abaixo, na **Figura 1** vemos uma visão geral da estrutura de uma classe em Kotlin.

![Estrutura de uma classe](https://www.devmedia.com.br/arquivos/Salas/Linguagem/kotlin/113/classes_artigo.png)**Figure 1**. Estrutura de uma classe

Ao longo desse artigo explicaremos esses elementos e alguns outros.

### Declaração de classes

Em Kotlin, um arquivo pode conter diferentes **declarações de classes**, as quais são feitas utilizando a palavra reservada class. Abaixo apresentamos uma **declaração de classe feita da forma mais simples possível em Kotlin**.

```
class Medicamento
```

No exemplo acima, uma vez que a classe Medicamento não possui um corpo, podemos omitir as chaves que estariam na frente do seu nome.

### Construtores

Um construtor é uma função especial da classe, utilizada na criação e inicialização dos objetos derivados dela.

Kotlin distingue os construtores de uma classe entre primários e secundários. O construtor primário de uma classe faz parte do seu cabeçalho e pode conter apenas uma lista de parâmetros, assim como apresentado abaixo.

```
class Medicamento(val formula: String, val posologia: String)
```

Dado que o construtor primário não pode conter nenhum código, uma classe pode conter um ou mais blocos de inicialização, que são executados na ordem como são declarados no corpo da mesma. Nesses blocos de inicialização, conforme visto no **Código 1**, podemos acessar quaisquer parâmetros que estejam presentes no construtor primário. A relação entre construtor primário e propriedades será detalhada ainda neste artigo.

```
class Medicamento(val formula: String, val posologia: String) {
     init {
         require(formula.trim().length > 0) {
            "Informe uma fórmula"
         }

         require(posologia.trim().length > 0) {
            "Informe uma posologia"
         }
     }
}
```

**Código 1**. Blocos de inicialização

Além do construtor primário, uma classe também pode declarar um ou mais construtores secundários. Esses, por sua vez, sempre devem utilizar a palavra reservada constructor em suas declarações, mesmo quando um modificador de acesso for omitido ou nenhuma anotação for utilizada, como mostra o **Código 2**.

```
class Medicamento {
    constructor(formula: String, posologia: String)
}
```

**Código 2**. Construtor secundário

**No caso de uma classe possuir mais de um construtor**, sendo um deles primário, cada construtor secundário deve delegar ao construtor primário. Quando ambos os construtores estiverem na mesma classe, a delegação de um construtor para o outro é feita com a palavra reservada this.

Podemos utilizar valores padrão do construtor delegado de uma classe. Para demonstrar isso vamos adicionar um terceiro parâmetro no construtor para a contraindicação do medicamento. Considerando que a classe Medicamento possa ser inicializada sem um valor para essa propriedade, caso no qual o mesmo passará a ser uma mensagem padrão, poderíamos modelá-la da seguinte forma apresentada no **Código 3**.

```
class Medicamento(val formula: String, posologia: String, val contraindicacao: String) {

     constructor(formula: String, posologia: String): this(formula, posologia,
          "Este medicamento não é indicado para pessoas alérgicas a $formula")
}
```

**Código 3**. Valores padrão do construtor delegado

Dessa forma, na delegação do construtor this(formula, posologia, "Este medicamento não é indicado para pessoas alérgicas a $formula") informamos um valor padrão para a propriedade contraindicacao e assim a classe poderá ser instanciada com ou sem esse terceiro parâmetro, como mostra o **Código 4**.

```
val medicamento1 = Medicamento("C8H9NO2", "...")
val medicamento2 = Medicamento("C8H9NO2", "...", "Minha contraindicação")
```

**Código 4**. Instanciando uma classe com ou sem parâmetros

No exemplo acima, o objeto medicamento1 será iniciado com o valor padrão para a propriedade contraindicacao, que é "Este medicamento não é indicado para pessoas alérgicas a $formula". Para o objeto medicamento2, o valor da propriedade contraindicacao será aquele informado no construtor, que é "Minha contraindicação".

### Construtores e propriedades

O construtor primário age de forma diferente do construtor secundário quanto a geração de propriedades para a classe na qual eles são declarados. Essa abordagem impede que as propriedades de uma classe variem de acordo com os parâmetros declarados nos construtores, uma vez que a geração de propriedades é limitada ao construtor primário de uma classe.

Para demonstrar isso observe a declaração da classe Medicamento no **Código 5**.

```
class Medicamento {
    constructor(formula: String, posologia: String)
}
```

**Código 5**.Impedindo as propriedades de variarem

Diferentemente do construtor primário, os parâmetros do construtor secundário não gerarão propriedades, visto que ao tentar executar um código como o **Código 6**, será gerado um erro de compilação.

```
val medicamento: Medicamento = Medicamento("", "")
medicamento.formula

Error:(10, 17) Kotlin: Unresolved reference: formula
```

**Código 6**. Erro de compilação

Sendo assim, nesse caso é obrigatório declarar uma propriedade na classe Medicamento, a qual poderá receber o valor do parâmetro do construtor e ser acessada a partir de uma instância, como visto acima.

Podemos eliminar o erro ao acessar a propriedade formula, conforme o **Código 7**.

```
class Medicamento {
     val formula: String

     constructor(formula: String, posologia: String) {
        this.formula = formula
     }
}
```

**Código 7**. Eliminando o erro de compilação

Assim, uma classe em Kotlin pode possuir propriedades declaradas em seu corpo explicitamente. Essas propriedades podem ser mutáveis, quando declaradas com a palavra reservada var, ou somente leitura, quando declaradas com a palavra reservada val. Caso elas sejam declaradas imutáveis, será necessário inicializá-las, o que pode ser feito no construtor secundário da classe, como no exemplo acima.

### Imutabilidade dos parâmetros dos construtores secundários

Conforme demonstrado anteriormente, os parâmetros do construtor secundário são declarados sem utilizar as palavras reservadas val ou var, como apresentado no **Código 8**.

```
class Medicamento {
    constructor(formula: String, posologia: String)
}
```

**Código 8**. Imutabilidade dos parâmetros

Caso uma tentativa de fazer o contrário seja feita, digamos declarando a fórmula como val formula: String, um erro de compilação será emitido com a seguinte mensagem apresentada no **Código 9**.

```
Kotlin: 'val' on secondary constructor parameter is not allowed
```

**Código 9**. Erro de Compilação

**Em Kotlin**, tanto os parâmetros de funções quanto os de construtores são imutáveis por definição, o que elimina a necessidade de se utilizar val ou var nesse caso.

Em adendo, a utilização de val ou var em construtores é restrita ao construtor primário de uma classe para definir a mutabilidade das suas propriedades, será imutável ou não, dado que apenas ele pode gerar propriedades para uma classe.

Por exemplo, digamos que a fórmula de um medicamento nunca possa alterada, uma vez que seja definida no momento da criação de uma instância, mas que a sua posologia depende de uma regra externa a essa classe. Dessa forma, podemos modelar a classe Medicamento conforme apresentado a seguir:

```
class Medicamento(val formula: String, var posologia: String)
```

Tendo sido o parâmetro formula declarado imutável, a propriedade homônima da classe Medicamento também será somente leitura. Ao tentar atribuir valor a ela a partir de uma instância o seguinte erro será apresentado:

```
Error:(8, 5) Kotlin: Val cannot be reassigned
```

O mesmo erro não ocorrerá ao atribuir valor a propriedade posologia a partir de uma instância da classe Medicamento, uma vez que o parâmetro posologia, presente no construtor primário, foi declarado mutável.

### Instanciando uma classe

Para criar uma instância de uma classe usamos seu nome e construtor. Em Koltin não utilizamos a palavra-chave new, como mostra o **Código 10**.

```
class Medicamento(val formula: String, val posologia: String) {
}

val medicamento = Medicamento("C8H9NO2", "...")
```

**Código 10**. Sem o uso do new

Aqui devemos observar que se uma classe possui um construtor, sendo ele primário ou secundário, o mesmo deve ser invocado. Por exemplo, no **Código 11** a classe Medicamento possui um construtor secundário, o que torna obrigatório instanciá-la da forma Medicamento("C8H9NO2", "...").

```
class Medicamento {
    constructor(formula: String, posologia: String)
}
```

**Código 11**. Invocando construtor

Caso isso não seja feito e a classe seja instanciada como Medicamento(), sem que os argumentos sejam fornecidos para o construtor, teremos um erro de compilação com a mensagem Error:(10, 35) Kotlin: No value passed for parameter 'formula' Error:(10, 35) Kotlin: No value passed for parameter 'posologia', onde formula e posologia são propriedades da classe.

### Funções membro

Funções membro são funções declaradas dentro de classes. As regras que aprendemos anteriormente para a escrita de funções também se aplicam aqui com uma exceção, funções membro podem utilizar a palavra-chave this para referenciar a instância atual.

Funções membro sempre devem ser invocadas a partir de instâncias da classe. No **Código 12** vemos um exemplo onde invocamos uma função a partir de uma instância de uma classe Medicamento.

```
class Medicamento(val formula: String, val posologia: String) {

    fun contem(formula: String) = this.formula.contains(formula, ignoreCase = true)
}

fun main() {
    val medicamento = Medicamento("C8H9NO2", "...")

    if (medicamento.contem("C8H9NO2")) {
        println("Este medicamento contém paracetamol")
    }
}
```

**Código 12**. Funções membro

Observe que na **Linha 3** utilizamos this para diferenciar entre a propriedade e o parâmetro formula.

Tenha cuidado ao usar this em classes que possuam apenas um construtores secundários, uma vez que eles não geram propriedades e, portanto, devem iniciar a classe ou delegar isso para um outro construtor. Entendido isso, caso uma classe possua apenas um construtor secundário ela deve conter propriedades que precisam ser iniciadas dentro dele.

Por exemplo, o **Código 13** vai falhar ao usarmos a palavra-chave this, pois a classe não possui uma propriedade chamada formula.

```
class Medicamento {
    constructor(formula: String, posologia: String)

    fun contem(formula: String) = this.formula.contains(formula, ignoreCase = true)
}
```

**Código 13**. Falha ao usar a palavra-chave this

No **Código 13** o erro emitido pelo compilador será Error:(6, 40) Kotlin: Unresolved reference: formula. Para corrigir esse erro a classe precisa iniciar uma propriedade chamada formula no construtor secundário, como mostra o **Código 14**.

```
class Medicamento {
    val formula: String
    val posologia: String

    constructor(formula: String, posologia: String) {
        this.formula = formula
        this.posologia = posologia
    }

    fun contem(formula: String) = this.formula.contains(formula, ignoreCase = true)
}
```

**Código 14**. Iniciando propriedade

Contudo, o código acima é desnecessariamente longo e nesses casos podemos declarar a classe com um construtor primário, como vemos no **Código 15**.

```
class Medicamento(val formula: String, val posologia: String) {

    fun contem(formula: String) = this.formula.contains(formula, ignoreCase = true)
}
```

**Código 15**. Declara a classe com um construtor primário

Sendo assim, o código acima é equivalente ao anterior e também resolve o problema do this, porém de um jeito mais sucinto.

### Construtores e blocos de inicialização

A delegação de um construtor secundário para o construtor primário ocorrerá como sendo a primeira instrução no construtor secundário. Isso quer dizer que, uma vez que eles passam a fazer parte do construtor primário, cada bloco de inicialização será executado antes de qualquer construtor secundário.

Fundamentalmente, caso uma classe não possua nenhum construtor e não seja abstrata, um construtor público vazio lhe será atribuído. Supondo que nessa mesma classe não exista um construtor primário declarado, a delegação ocorrerá de forma implícita, como demonstrado no **Código 16**.

```
class Medicamento {
     val formula: String

     constructor(formula: String, posologia: String) {
         this.formula = formula

         println("Construtor secundário")
     }

     init {
         println("Bloco de inicialização")
     }
}
```

**Código 16**. Delegação implícita

No exemplo acima, não importando a ordem da declaração do construtor secundário e do bloco de inicialização, as mensagens exibidas serão “Bloco de inicialização” e “Construtor secundário”, nessa ordem.

### Níveis de acesso

Em Kotlin, não informar um nível de acesso para um tipo faz com que o modificador public seja automaticamente aplicado. Além desse, geralmente não utilizado devido a redundância que causa, Kotlin possui três níveis de acesso. Dentre eles, Protected pode ser utilizado apenas por classes aninhadas. Quando utilizado em nível de arquivo, um erro será emitido pelo compilador, como mostra o **Código 17**.

```
protected class Medicamento constructor(val formula: String, var posologia: String)

Error:(3, 1) Kotlin: Modifier 'protected' is not applicable inside 'file'
```

**Código 17**. Erro do compilador

No **Código 18** utilizamos o modificador protected, que é sintaticamente permitido porque Tributacao não está em nível de arquivo e é uma classe aninhada em Medicamento.

```
class Medicamento {

    protected class Tributacao
}
```

**Código 18**. Modificador protected

Private é um nível de acesso que restringe o escopo de utilização da classe apenas ao arquivo no qual ela foi declarada.

Ao tentar utilizar uma classe private fora do arquivo no qual ela foi declarada, um erro será emitido pelo compilador prematuramente, no momento da sua importação, como mostra o **Código 19**.

```
private class Medicamento constructor(val formula: String, var posologia: String)

import br.com.devmedia.kotlin.a.Medicamento

fun main(args: Array) {
     println(Medicamento("C8H9NO2", "...12 anos ou mais variam de 500 a 1000 mg/
     dose com intervalos de 4 a 6 horas..."))
}

Error:(3, 33) Kotlin: Cannot access 'Medicamento': it is private in file
```

**Código 19**. Modificador private

Em Kotlin, a declaração de um pacote é feita com a palavra reservada package, seguida do nome do pacote. A importação de uma classe de outro pacote é feita com a palavra import, seguida do nome completo da classe.

O terceiro modificador de acesso é internal, que permite criar uma instância da classe em qualquer lugar no módulo no qual ela foi declarado. Para o Kotlin um módulo é um conjunto de fontes compilados juntos. Isso torna esse comportamento difícil de ser verificado em um mesmo módulo do Intellij IDEA, projeto do Maven, Ant task, etc.

### Conclusão

Kotlin é uma linguagem orientada a objetos e funcional, que ganhou maior popularidade ao ser anunciada como uma alternativa ao Java para desenvolvimento para Android, na I/O Conference de 2017. Desde então, diversos projetos como Kotlin Native, Coroutines e Kotlin to [JavaScript](https://www.devmedia.com.br/guia/javascript/34372) têm sido alvo do interesse de diferentes comunidades de programadores. Independe de qual seja o propósito para o qual você pretende utilizar o Kotlin, será necessário conhecer a sintaxe dessa linguagem, que é concisa e, ao mesmo tempo, repleta de recursos sintáticos.