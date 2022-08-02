# Kotlin: Controle de fluxo

## Aprenda a utilizar as estruturas de controle de fluxo if, when, for e while em Kotlin com este artigo.

Por que eu devo ler este artigo:As estruturas de controle de fluxo são utilizadas para manipular a ordem em que os comandos de um programa são executados. De forma natural, isso acontece linha a linha e de cima para baixo, o que não é suficiente para que um programa represente o maior número de situações do mundo real. A fim de compensar essa limitação utilizamos tais estruturas, que são abordadas neste artigo.

Tire sua dúvidaMarcar como concluídoAnotarCódigo fonte

[Artigos](https://www.devmedia.com.br/artigos/)[Kotlin](https://www.devmedia.com.br/artigos/kotlin)Kotlin: Controle de fluxo

As estruturas de controle de fluxo são utilizadas para manipular a ordem em que os comandos de um programa são executados. De forma natural, isso acontece linha a linha e de cima para baixo, o que não é suficiente para que um programa represente o maior número de situações do mundo real. A fim de compensar essa limitação utilizamos tais estruturas, que são abordadas neste artigo.

------

##### Guia do artigo:

- [if](https://www.devmedia.com.br/kotlin-controle-de-fluxo/40767#if)
- [when](https://www.devmedia.com.br/kotlin-controle-de-fluxo/40767#when)
- [for](https://www.devmedia.com.br/kotlin-controle-de-fluxo/40767#for)
- [while](https://www.devmedia.com.br/kotlin-controle-de-fluxo/40767#while)

------

Por exemplo, é possível que em um programa seja necessário determinar qual dentre dois números é o maior para se saber qual é o dia para pagamento mais distante. Escrever tal código só é possível utilizando uma estrutura condicional, como veremos na próxima seção.

Esse é apenas um dentre os muitos exemplos que justificam a utilização de **estruturas de controle de fluxo** em um programa. Ao longo deste artigo conheceremos outras situações semelhantes e como resolvê-las utilizando a estrutura mais adequada.

### if

A estrutura condicional if permite ao programa executar um dentre diferentes blocos de código dependendo do valor retornado por uma expressão lógica. A forma de se escrever essa estrutura pode ser vista em pseudocódigo no **Código 1**.

```
if expressão-lógica
    código
```

**Código 1**. Pseudocódigo if

Por exemplo, para saber qual é o maior valor entre duas variáveis podemos usar o **Código 2**.

```
val a = 9
val b = 10

var max = a

if(b > a) {
    max = b
}
```

**Código 2**. Maior variável

Dessa forma, caso a expressão lógica b > a retorne true, a expressão max = b será executada e o valor de max será 10. Caso contrário, o código dentro do if será ignorado, fazendo com que max termine com o valor 9.

A estrutura if pode ser estendida em um ou mais blocos else ou else...if.

Por exemplo, caso tenhamos que determinar qual dentre dois valores é o maior ou imprimir "valores iguais" podemos fazer conforme o **Código 3**.

```
val a = 2
val b = 4

if(b == a) {
    println("valores iguais")
}
```

**Código 3**. Impressão de valores iguais

Utilizando else...if podemos imprimir, por exemplo, "valores iguais" ou "valores diferentes". O resultado está no **Código 4**.

```
if (b > a) {
    println("valores diferentes")
} else if(a == b) {
    println("valores iguais")
}
```

**Código 4**. Código 3 utilizando else if

Podemos utilizar quantos if, if...else ou else...if forem necessários para implementar uma lógica.

### Usando if como expressão

Em Kotlin if é uma expressão e retorna um valor ao ser avaliado durante a execução do programa. Por isso aqui não temos o operador ternário ( : ?), assim como em outras linguagens. Isso porque apenas com o if conseguimos criar expressões semelhantes às obtidas com o operador ternário.

Na seção anterior aprendemos como utilizar o if de forma semelhante ao que aprendemos na matéria de algoritmos. Abaixo, no **Código 5** temos um exemplo de como utilizar o if como expressão.

```
val max = if(a > b) a else b
```

**Código 5**. if como expressão

Quanto a utilizar o if como expressão temos duas regras. Primeiro, o uso dos parênteses é obrigatório, de forma que escrever a expressão acima como val max = if a > b a else b estaria errado. Segundo, é obrigatório que exista um bloco else nesse caso.

Ao utilizar if como expressão o último comando no bloco if ou else retornará por padrão. Por exemplo, no **Código 6** o valor da variável max será a ou b.

```
val max = if(a > b) {
    println(a)
    a
} else {
    println(b)
    b
}
```

**Código 6**. if else como expressão

Perceba que para max obter o valor de a ou b não é necessário utilizar o comando return dentro do if/else, pois o último comando encontrado em um desses blocos será retornado por padrão.

### when

A estrutura de múltipla seleção when é utilizada para comparar um valor com conjunto de opções. Dessa forma, cada opção é verificada sequencialmente e uma vez que haja uma correspondência um valor pode ser retornado ou um comando executado.

Essa estrutura é muito utilizada quando uma variável pode assumir um dentre alguns valores conhecidos. No **Código 7**, por exemplo, estado pode ter os valores ativo ou inativo e qual mensagem será exibida para o usuário depende disso.

```
val estado = "ativo"

when(estado) {
    "ativo" -> print("Clique para desativar")
    "inativo" -> print("Clique para ativar")
}
```

**Código 7**. Uso do When

Opcionalmente podemos adicionar um bloco else às ramificações de when. Dessa forma, quando o valor informado não corresponder a nenhuma das opções esse bloco será executado, como mostra o **Código 8**.

```
val estado = "ativo"

when(estado) {
    "ativo" -> print("Clique para desativar")
    "inativo" -> print("Clique para ativar")
    else -> {
        print("valor de estado é desconhecido")
    }
}
```

**Código 8**. Uso do when else

Assim como o bloco else acima, qualquer ramificação dentro de when pode ser um bloco de código delimitado por chaves { }, ou um único comando ou expressão e nesse caso as chaves são opcionais.

É possível ainda agrupar diferentes valores dentro de uma única ramificação em when. Com isso o bloco de código pertencente a ela será executado caso haja uma correspondência por qualquer um dos seus valores, como mostra o **Código 9.**

```
val estado = ""

when(estado) {
    "ativo", "inativo" -> print("estado conhecido)
    else -> {
        print("estado desconhecido")
    }
}
```

**Código 9**. When com uma ramificação

No código acima, caso estado seja "ativo" ou "inativo" o trecho de código print("estado conhecido") será executado. Caso contrário, o código print("estado desconhecido") no else será executado.

### Usando when como expressão

É possível também utilizar when como expressão. Por exemplo, no **Código 10** atribuímos o valor true ou false a variável resultado dependendo do valor da variável estado.

```
val estado = "ativo"

val resultado = when(estado) {

  "ativo" -> true

  else -> false

}
```

**Código 10**. When como expressão

Ao utilizar when como expressão é obrigatória a presença de um bloco else. No caso acima, uma vez que o valor de estado é ativo a variável resultado receberá o valor true.

### 💡 when tem super-poderes

A condição em uma ramificação de when também pode ser uma expressão que retorna algum valor, desde que o valor retornado seja no mesmo tipo que o valor passado para when. Por exemplo, para comparar um número com uma série podemos escrever o seguinte código:

```
val poltrona = 2

when(poltrona) {

  in 1..10 -> println("Fileira A")
  in 11..20 -> println("Fileira B")
  in 21..30 -> println("Fileira C")
  in 31..40 -> println("Fileira D")
}
```

**Código 10**. When como expressão

Nesse exemplo, caso o valor da variável esteja ente os número 1 e 10, a mensagem "Fileira A" será impressa. Do contrário esse valor será comparado com as séries 11 a 20, 21 a 30 e, por fim, 31 a 40, executando cada respectivo código quando houver uma correspondência.

Nesse caso, o uso do operador in é obrigatório para que a comparação possa ser feita entre o valor cada número na série. Caso se queira saber se o número não está na série, por alguma razão, usamos !in, mas é pouco comum usarmos in dessa maneira.

### for

A estrutura de repetição for é muito utilizada para percorrer coleções. Em Kotlin a sintaxe dessa estrutura pode ser vista no **Código 11**.

```
for([item] in [coleção]) {
   [código]
}
```

**Código 11**. For em pseudocódigo

Coleção, neste contexto, é uma lista de valores que pode ser armazenada em uma variável. Ela não armazena os itens, mas a lista, de forma que ela continua contendo um único objeto, que nesse caso é uma estrutura de dados capaz de armazenar múltiplos valores.

Existem diversas formas de se gerar coleções em Kotlin que possamos utilizar para exemplificar como utilizar for. Uma delas é com uma expressão de série, como 1..4, para gerar os números de 1 a 4, 3..100 para os números de 3 a 100 e assim por diante.

Por exemplo, no **Código 12** a variável colecao contém os números de 1 a 9. Ao passarmos essa coleção para for serão impressos cada um dos seus itens, gerando a saída 123456789.

```
val colecao = 1..9

for (item in colecao) {
    print(item)
}
```

**Código 12**. Utilizando for para percorrer uma série

A variável item, no **Código 12**, é utilizada para armazenar os valores em colecao, um de cada vez. Dado que colecao contém os valores de 1 a 9, gerados com a expressão de série 1..9, for será repetido nove vezes. Da primeira vez o valor de item será 1, depois 2 e assim por diante.

### while

A segunda e última estrutura de repetição disponibilizada pela linguagem é while. Ela possui duas variações, sendo a primeira while e a segunda do..while. A sintaxe dessa estrutura podemos ver no **Código 13**, onde expressão-lógica deve ser avaliada como verdadeira para que a repetição continue.

```
while([expressão-lógica]) {
    [código]
}

do {
    [código]
} while([expressão-lógica])
```

**Código 13**. while e do..while em pseudocódigo

A principal diferença entre while ou do..while está na forma como eles verificam se a repetição deve acontecer. Enquanto while primeiro avalia a expressão lógica e depois executa a repetição, do..while faz o oposto, primeiro executando a repetição e depois avaliando a expressão lógica. Dessa forma, é certo que do..while repetirá pelo menos uma vez, enquanto while pode nunca se repetir.

Por exemplo, no **Código 14** temos a mesma expressão lógica sendo aplicada a while e do..while.

```
while (false) {
    println("Olá mundo!")
}

do {
    println("Olá mundo!")
} while (false)
```

**Código 14**. Expressão com while

Como resultado, no caso de while a mensagem Olá mundo! nunca será impressa, uma vez que a expressão lógica logo é avaliada como falsa. Em do..while a mensagem Olá mundo! será impressa uma vez, então a expressão lógica será avaliada como falsa e as repetições se encerrarão.

A principal diferença entre while e for é que no caso de while o controle de quando as repetições se encerrarão fica com o programador. Voltando ao **Código 12** perceba que as repetições ocorrerão apenas enquanto houver itens na série 1..9. Sendo assim, o momento de parada é aquele em que o último item na série for impresso.

Com while devemos nós mesmos nos certificar de que, em algum momento, as repetições se encerrarão, o que normalmente é feito utilizando variáveis de controle. Sendo assim, um código para percorrer uma série de número utilizando while poderia ser este do **Código 15**.

```
var nums = 0
val serie = 1..9

while (nums < serie.last) {
    println(serie.elementAt(nums))

    nums++
}
```

**Código 15**. Percorrendo uma série com while

Aqui utilizamos a variável de controle nums, que vai sendo incrementada a cada repetição. Ela é utilizada também na expressão lógica nums < serie.last para determinar que as repetições devem se encerrar assim que nums chegar ao valor 9.

Além disso, note que agora precisamos acessar o valor que corresponde a repetição atual na série a partir de um índice.

A função elementAt() é utilizada para acessar um número na série a partir da sua posição. Aqui e em muitas outras coleções de valores a primeira posição é 0. Sendo assim, na primeira repetição nums terá o valor 0 e o valor acessado na série estará no índice 0, que contém o número 1. Na próxima repetição, nums terá o valor 1 e acessará o valor no índice 1 da série, que é 2. Isso continuará acontecendo até que nums assuma o valor 8, índice que contém o valor 9 na série.

### Conclusão

**As estruturas de controle de fluxo são a base para se conseguir implementar algoritmos em uma linguagem de programação**. Ao avançar na linguagem até o ponto em que criaremos nossos próprios programas completos isso ficará ainda mais evidente. Kotlin possui as mesmas estruturas que a maioria das linguagens de programação, com exceção de que elas podem retornar valores, quando necessário. Essa pequena alteração na linguagem facilita o desenvolvimento e torna tais estruturas ainda mais essenciais no desenvolvimento de aplicações.