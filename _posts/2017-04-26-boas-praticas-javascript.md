---
layout: post
title: Boas práticas para JavaScript
bigimg: /img/first_post.jpg
tags: [development,javascript,boaspraticas]
comments: true
---

JavaScript é uma linguagem de programação interpretada, com tipagem fraca e dinâmica e totalmente executada no lado do cliente (navegador web). É atualmente a principal linguagem client-side disponível na Web e que, nos últimos anos, passou também a ser utilizada para desenvolvimento server-side (vide node-js). Também possui suporte a programação funcional e é baseada em ECMAScript, uma linguagem para programação baseada em Script e padronizada pela Ecma International.

Como em todo tipo de programação, especialmente em linguagens emergentes e/ou bastante dinâmicas (vide JavaScript com seus diversos frameworks e bibliotecas nascendo por segundo), para se desenvolver utilizando JavaScript também temos as boas e as más práticas. Este artigo dedica-se a introduzir as boas práticas para o desenvolvimento com esta linguagem, de maneira incremental, podendo (e devendo) ser acrescido de novas boas práticas sempre que disponível.

**1. Atente-se ao uso de === e ==**

Como dito anteriormente, o JavaScript é uma linguagem com tipagem fraca e dinâmica. Como tal, dispõe-se de dois tipos de operadores de igualdade:

a) === e !==;

b) == e !=;

Mas qual a diferença?

Os operadores **==** e **!=** tentam converter os valores das variáveis envolvidas na comparação para o mesmo tipo. Por exemplo, se eu tenho uma **var a = “5”** e uma **var b = 5**, caso eu utilize **==** será retornado true, pois ele fará a conversão de ambas as variáveis para um mesmo tipo e assim elas possuem o mesmo valor.

Já os operadores **===** e **!==** não tentarão converter os valores das variáveis envolvidas na comparação para o mesmo tipo. Assim, para que o retorno seja true em uma comparação utilizando **===**, ambas as variáveis precisam possuir o mesmo valor, o mesmo tamanho e o mesmo tipo.
Outro exemplo bastante comum da diferença entre os operadores em JavaScript, se dá em comparações utilizando undefined e null. Vale ressaltar que **undefined** se refere a um valor indefinido que não necessariamente é nulo e, portanto, não é de fato igual à **null**. Então, se temos uma **var a = null** caso seja feita uma comparação utilizando **== undefined**, a condição retornará true. Já se fizermos o mesmo utilizando **=== undefined** será retornado false, pois os valores não são do mesmo tipo.

**2. Sempre adicione scripts e arquivos JavaScript ao final da página**

Essa é a dica de ouro no campo de performance de páginas Web: Jamais coloque um arquivo .js ou um trecho de script para ser carregado em outro lugar que não seja o final da página (antes do fechamento da tag <body> a menos que seja extremamente necessário.

Mas por quê? Bom, os navegadores Web carregam a página para visualização do usuário de maneira sequencialmente interpretada, ou seja, lendo e exibindo linha por linha. Então, de forma simples, se você adicionou um script no início ou no meio da página, o navegador não conseguirá avançar em seu carregamento enquanto o script não for completamente interpretado e executado. Assim, o usuário terá de ficar mais tempo esperando enquanto nada (ou quase nada) é exibido na página.

Se você adiciona seus scripts ao fim da página, eles só serão carregados após toda a página estar sendo exibida ao usuário, não influenciando de maneira direta em sua navegação e reduzindo seu tempo de espera. Lembre-se: Performance é UX.

**3. Reduza as variáveis globais, crie objetos**

__"Ao reduzir a quantidade de variáveis globais a um único nome, você reduz, significantemente, a chance de más interações com outras aplicações, widgets ou bibliotecas"__. - Douglas Crockford

Ao invés de:

{% highlight javascript %}
var name = 'Vinícius';
var lastName = 'Silva';
function doSomething() {...}

console.log(name); // Vinícius -- or use window.name
{% endhighlight %}

Utilize:

{% highlight javascript %}
var DudeNameSpace = {
 name : 'Vinícius',
 lastName : 'Silva',
 doSomething : function() {...}
}

console.log(DudeNameSpace.name); // Vinícius
{% endhighlight %}

**4. Instanciar múltiplas variáveis sem a palavra-chave var**

Sim, assim como em Java, você pode instanciar múltiplas variáveis de uma só vez separando-as com uma vírgula. Isso funciona da seguinte forma:

{% highlight javascript %}
var someItem = 'some string',
  anotherItem = 'another string',
  oneMoreItem = 'one more string';
{% endhighlight %}

**5. Cuidado com o escopo this**

Em JavaScript, diferentemente de Java, utiliza-se bastante a palavra chave this para indicar o escopo/contexto de uma função/método e não exatamente de uma classe (visto que na maioria dos casos ainda não utilizamos classes e objetos como em O.O. no JavaScript). Assim, devemos nos atentar para o que estamos referenciando com o this.

Para demonstrar, tomarei dois exemplos:

{% highlight javascript %}
$(document).ready(function(){
  $(this).find("#teste").css("background-color", "red");
)};
{% endhighlight %}

e:

{% highlight javascript %}
$(document).ready(function(){
  $(“.btn”).on(“click”, function(){
     var id = $(this).attr(“id”);
  });
)};
{% endhighlight %}

Qual a diferença entre o uso do **this** nos dois casos?

Bom, o primeiro faz referência ao **document** que é o contexto no qual ele está inserido e, no exemplo, busca em todo o documento por um elemento com o ID teste e altera seu fundo para vermelho. Já no segundo exemplo, o **this** faz referência ao contexto do botão que foi clicado (atributo listener do tipo click ao qual colocamos através de bind no botão) e retorna o id deste botão. Caso o segundo exemplo de **this** tivesse a mesma funcionalidade do primeiro, ele provavelmente não faria nada pois não encontraria o elemento com ID teste no documento estando fora do contexto do documento (a menos que houvesse um elemento com ID teste dentro do botão clicado, claro).

**6. Nunca declare objetos String, Number ou Boolean e não use new Object()**

Sempre trate String, Number ou Boolean como primitivos e não como objetos. Declarar essas variáveis como objetos reduz a velocidade de execução e pode causar efeitos colaterais indesejados. Exemplos:

{% highlight javascript %}
var x = "John";
var y = new String("John");
(x === y) // is false because x is a string and y is an object.
var x = new String("John");
var y = new String("John");
(x == y) // is false because you cannot compare objects.
{% endhighlight %}

Além disso, não use a palavra chave new e seu construtor para instanciar variáveis. Ao invés disso:

* Use {} ao invés de new Object()
* Use "" ao invés de new String()
* Use 0 ao invés de new Number()
* Use false ao invés de new Boolean()
* Use [] ao invés de new Array()
* Use /()/ ao invés de new RegExp()
* Use function (){} ao invés de new Function()

Exemplos:

{% highlight javascript %}
var x1 = {};           // novo objeto
var x2 = "";           // nova string primitiva
var x3 = 0;            // novo número primitivo
var x4 = false;        // novo booleano primitivo
var x5 = [];           // novo objeto de vetor
var x6 = /()/;         // novo objeto de regexp
var x7 = function(){}; // novo objeto de função
{% endhighlight %}

**7. Cuidado com adição e concatenação**

A adição é definida pela soma de dois valores. Já a concatenação é definida pela junção de duas strings em uma. Em JavaScript, assim como no Java, ambas as operações são realizadas através do operador +.

Por exemplo:

{% highlight javascript %}
var x = 10 + 5;          // o resultado de x será 15
var x = 10 + "5";        // o resultado de x será "105"
{% endhighlight %}

Isso ocorre pois a linguagem é fracamente tipada, então ela faz a conversão implícita das variáveis para um mesmo tipo e realiza a operação. Neste segundo caso, ela converte ambas para String e concatena.

**8. Undefined não é null**

Em JavaScript, null é usado para objetos e undefined é usado para variáveis, propriedades e métodos. Para ser null, um objeto deve ter sido definido antes, senão ele será undefined. É muito comum ficarmos em dúvida de como comparar se uma variável é undefined ou null. A maneira correta é:

{% highlight javascript %}
if (typeof myObj !== "undefined" && myObj !== null)
{% endhighlight %}

Como se vê no exemplo, antes de compararmos se o objeto é nulo, devemos comparar se o objeto foi definido. Se ele não tiver sido definido, não tem como ele ser nulo. Outro ponto de atenção é o citado no item 1 deste artigo: atente-se ao operador de comparação. Perceba que para este caso nós queremos comparar a variável como um todo, incluindo seu tipo, então deve-se utilizar o operador **!==** para se ter uma comparação exata.

**9. Reduza o acesso ao DOM**

Um erro bastante comum pode ser visto no exemplo à seguir:

{% highlight javascript %}
document.getElementById("demo").innerHTML("Hello");
document.getElementById("demo").innerHTML("World!");
{% endhighlight %}

Ou para os mais acostumados com jQuery:

{% highlight javascript %}
$("#demo").html("Hello");
$("#demo").html("World!");
{% endhighlight %}

Mas qual é o erro nisso? Afinal, o código funciona!

Sim, o código funciona. Porém, perceba que em cada linha ele foi realizada uma chamada para o método **getElementById** sobre o documento no primeiro exemplo e, no segundo exemplo, para o método **html()** sobre o seletor **$("#demo")**. Essas chamadas são realizadas em cima de uma API chamada **DOM**. O **DOM (Document Object Model)** é uma plataforma existente nos navegadores, que indexa e representa as marcações (HTML, XML, etc) em uma árvore da maneira como elas são lidas pelo navegador durante a renderização de uma página. Feito isso, o DOM permite a manipulação dessas marcações através de diversos métodos providos por sua API.

Enfim, o problema dos códigos mostrados acima é que, em cada chamada de métodos que fazemos, o DOM terá de varrer toda a árvore em busca do elemento que estamos buscando e alterá-lo, o que reduz em muito o desempenho da nossa aplicação. Além disso, criamos um problema acerca da reutilização pois se um dia decidirmos mudar o ID ou a Classe de um elemento que utilizamos em vários lugares, teremos que alterar em cada parte do código em que este está sendo replicado. Pensando nisso, as soluções mais viáveis seriam salvar o resultado do seletor uma única vez em uma variável e reutilizá-la em todos os locais necessários, fazendo assim somente uma chamada ao DOM e que, caso alteremos o nome do elemento, só precisaremos alterar em um trecho de código no script. Seguem os exemplos refatorados:

{% highlight javascript %}
var obj = document.getElementById("demo");
obj.innerHTML("Hello");
obj.innerHTML("World!");
{% endhighlight %}

Em jQuery:

{% highlight javascript %}
var obj = $("#demo");
$(demo).html("Hello");
$(demo).html("World!");
{% endhighlight %}

**Quer mais?**

Então tenho a dica de ouro para você: Acesse o <a href="http://jstherightway.org/" target="_blank">JS: The Right Way!</a> Esse é simplesmente aquele guia espetacular que abrange conceitos do começo ao fim, sendo mantido e revisado por diversos desenvolvedores através do GitHub.

**Referências**
* <a href="https://code.tutsplus.com/pt/tutorials/24-javascript-best-practices-for-beginners--net-5399" target="_blank">24 boas práticas no JavaScript para Iniciantes - Tuts+</a>
* <a href="http://www.devmedia.com.br/boas-praticas-de-programacao-em-javascript/34215" target="_blank">Boas práticas de programação em JavaScript - DevMedia</a>
* <a href="https://www.w3schools.com/js/js_best_practices.asp" target="_blank">JavaScript Best Practices - W3Schools</a>
* <a href="https://www.w3schools.com/js/js_mistakes.asp" target="_blank">JavaScript Common Mistakes - W3Schools</a>
* <a href="https://www.w3schools.com/js/js_performance.asp" target="_blank">JavaScript Performance - W3Schools</a>
* <a href="https://tableless.com.br/tenha-o-dom/" target="_blank">O que é DOM - Document Object Model - Tableless</a>

