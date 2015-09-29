---
layout: post
title:  "Criando Layouts com Susy Framework | Tableless"
date:   2015-08-03 11:23:32
categories: sass css susy
---
Conheça o Susy Framework e desenvolva layouts facilmente com ele.

Com a alta demanda e prazos cada vez menores somos obrigados a trabalhar cada vez mais rápido e isso serve de incentivo para que novos frameworks sejam feitos buscando melhorar a qualidade no desenvolvimento. Um desses frameworks é o Susy e nesse artigo vamos conhecer e dar os primeiros passos com ele.

<h2>O QUE É SUSY?</h2>

Susy é um framework que permite criar grids de acordo com as necessidades do seu site. Diferente de outros como Bootstrap e Foundation, você não vai precisar importar um arquivo cheio de classes em que vai usar apenas algumas delas. O Susy trabalha direto no estilo das classes que você definiu e personalizou.

Para começar a usá-lo você precisa ter o Sass instalado e o mínimo de conhecimento sobre ele. Não vou me aprofundar em Sass, pois não é o foco desse artigo, mas para quem tiver alguma dúvida a respeito pode ver uma série de artigos aqui mesmo no Tableless.

Agora que já sabemos do que se trata vamos começar a desenvolver nosso layout com Susy.

<h2>CRIANDO SEU PRIMEIRO LAYOUT COM SUSY</h2>

Assumindo que você já tem o Sass instalado, vamos instalar o Susy. Abra o prompt de comando e digite:

{% highlight ruby %}
$ gem install susy
{% endhighlight %}

Após concluir a instalação vamos criar uma pasta para o projeto e dentro dela um arquivo index.html, uma pasta css e uma pasta scss com um arquivo style.scss dentro dela.

<img src="http://tableless.com.br/wp-content/uploads/2015/07/estrutura-de-pastas.jpg" alt="Estrutura de pastas">

<h2>INICIANDO O DESENVOLVIMENTO</h2>

Vamos construir o seguinte layout:

<img src="http://tableless.com.br/wp-content/uploads/2015/07/layout.png" alt="Layout">

Primeiramente vamos iniciar o Sass para que nosso código possa ser compilado. Abra a pasta do projeto na linha de comando e digite:

{% highlight ruby %}
sass --watch scss:css -r susy
{% endhighlight %}

Feito isso um arquivo style.css foi criado dentro da pasta css. Vamos adiciona-lo no head no html:

{% highlight html %}
<link rel="stylesheet" href="css/style.css">
{% endhighlight %}

Vamos adicionar o normalize ao nosso projeto

Para usar o Susy no projeto temos apenas que adicionar a seguinte linha no style.scss:

{% highlight sass %}
@import "susy";
{% endhighlight %}

<h2>SUSY MAP</h2>

Susy Map é um conjunto de instruções que são declaradas no início do projeto.  O grid é gerado de acordo com as informações declaradas nele. Abaixo temos o Map do nosso projeto, vamos adiciona-lo no style.scss, mais informações podem ser adicionadas , mas para nosso projeto é o suficiente. As linhas estão comentadas informando o que cada uma faz.

{% highlight sass %}
$susy:(
    columns: 8, // número de colunas do grid
    container: 1140px, // largura do container
    debug: (image: show), // exibe as colunas do grid
);
{% endhighlight %}

Vou adicionar o código completo, mas somente as funções do Susy serão explicadas. Então vamos começar! No HTML, vamos adicionar o header da página:

{% highlight html %}
<header class="site-header">
    <div class="container">
        <div class="logo"><a href="#">Logo</a></div>
        <nav class="menu">
            <ul>
                <li><a href="#">Home</a></li>
                <li><a href="#">About</a></li>
                <li><a href="#">Photos</a></li>
                <li><a href="#">Contact</a></li>
            </ul>
        </nav>
    </div>
</header>
{% endhighlight %}

Vamos adicionar o estilo dele:

{% highlight sass %}
.container {
    @include container(); // inclui na classe o container definido no Susy Map
}
.logo {
    float: left;
    padding: 0.9375rem;
    line-height: 2rem;
    font-size: 1.5rem;
}
nav {
    float: right;
    li {
        list-style: none;
        float: left;
        margin-left: 2rem;
        line-height: 2rem;
    }
}
{% endhighlight %}

Depois do header vamos inserir o banner no html:

{% highlight html %}
<div class="container">
    <img src="img/banner.jpg" class="banner">
</div>
{% endhighlight %}

Agora o estilo do banner:

{% highlight sass %}
img {
    width:100%;
}
.banner {
    @include span(8 of 8);
    margin: gutter() 0;
    height: 100%;
}
{% endhighlight %}

O “@include span (8 of 8)” é uma função do Susy que diz que o banner irar ocupar 8 colunas das 8 declaradas no Susy Map, mas não é só isso, reparem que no margin adicionamos um valor “gutter()”, isso é outra função do Susy que adiciona o valor que existe entre os grids, pode ver como o espaço que está entre o header e o banner é o mesmo que está entre as 8 colunas que definimos no grids.

Agora vamos adicionar o conteúdo do layout:

{% highlight html %}
<div class="container">
    <div class="sidebar">
        <ul>
            <li><a href="#">Fortaleza</a></li>
            <li><a href="#">Natal</a></li>
            <li><a href="#">Recife</a></li>
            <li><a href="#">Salvador</a></li>
        </ul>
    </div>
    <div class="content">
        <div class="content-item"><img src="img/001.jpg"></div>
        <div class="content-item"><img src="img/002.jpg"></div>
        <div class="content-item"><img src="img/003.jpg"></div>
        <div class="content-item"><img src="img/004.jpg"></div>
        <div class="content-item"><img src="img/005.jpg"></div>
        <div class="content-item"><img src="img/006.jpg"></div>
    </div>
</div>
{% endhighlight %}

E o estilo delas:

{% highlight sass %}
.sidebar {
    @include span (2 of 8);
    ul {
        margin: 0;
        padding: 1.2rem;
    }
    li {
        list-style: none;
        font-size: 1.1rem;
        border-bottom: 2px dotted #c6c6c6;
        &:last-child {
            border:none;
        }
    }	
    a {
        display: block;
        padding: 1rem .5rem;
        color: #333;
        line-height: 2;
        text-decoration: none;
    }
}
.content {
    @include span (6 of 8 last);
}
.content-item {
    @include gallery(2 of 6);
    margin-bottom: gutter();
}
{% endhighlight %}

Repare nos includes que acabamos de usar. Na mesma linha usamos 2 colunas para o sidebar e as 6 para o content, veja que adicionamos um last no final do include na classe content, esse last serve para dizer que são as ultimas colunas da linha. O espaço entre os grids é feito com um margin-right e o last serve parar remover esse margin-right do ultimo item do grid. Olha lá o aquivo style.css para entender melhor o que está sendo feito.

O footer não tem muito o que falar, então vamos apenas adicionar o código:

{% highlight html %}
<footer class="site-footer">
    <div class="container">
        <p>Copyright © 2015 - Desenvolvido por Felipe César para o Tableless</p>
    </div>
</footer>
{% endhighlight %}

E o estilo:

{% highlight sass %}
.site-footer {
    margin-top: 2rem;
    padding: 0.6rem 0;
    color: #fff;
}
{% endhighlight %}

Por fim vamos apenas remover do Susy Map a linha que exibe os grids:

{% highlight sass %}
debug: (image: show),
{% endhighlight %}

Pronto, finalizamos o nosso layout, fácil não é?! Esse projeto foi bem simples apenas para apresentar o framework e mostrar como funciona, mas não pare por aí, faça o download do código, brinque com ele, acesse a documentação do Susy e conheça outros recursos que podem te ajudar bastante no desenvolvimento.

Espero que tenham gostado do artigo, bons estudos!

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
