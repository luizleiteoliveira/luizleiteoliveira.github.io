---
title: "Hacker101 segundo desafio"
summary: Nest post vamos mostrar o passo a passo para resolver o segundo CTF da _Hacker One_ como parte do caminho para entender melhor de possíveis falhas de segurança e vulnerabilidades. 
author: luizleite
published: 2021-07-30
category: 'hacker'
thumbnail: /assets/img/posts/OWASP.png
---

## Hacker101 CTF 2

Para quem não acompanhou tudo a pouco tempo atrás eu comecei a fazer os CTF's da *Hacker One ,* se **não **viu a primeira parte pode clicar [aqui](https://luizleiteoliveira.github.io/hacker/owasp/seguran%C3%A7a/2021/07/11/hacker101_o_inicio/#/). Agora que o cenário está alinhado vamos para o desafio chamado ***Micro-CMS v1** que está marcado como uma dificuldade fácil.

## Flag 1

Este problema diferente do nosso primeiro existem um total de 4 flags, eu não sei se tem alguma ordem correta ou mais fácil para encontrar, mas vou descobrir isso ao longo das resoluções.

Iniciando o problema clicamos no link e aparece a seguinte tela

![https://res.cloudinary.com/luizleiteoliveira/image/upload/v1627238265/blog/ctf2/Captura_de_Tela_2021-07-25_a%CC%80s_15.34.56_jeukgm.png](https://res.cloudinary.com/luizleiteoliveira/image/upload/v1627238265/blog/ctf2/Captura_de_Tela_2021-07-25_a%CC%80s_15.34.56_jeukgm.png)

Verificando o código notamos o seguinte:

```html
<body data-new-gr-c-s-check-loaded="14.1022.0" data-gr-ext-installed="">
		<ul>
<li><a href="page/1">Testing</a></li>
<li><a href="page/2">Markdown Test</a></li>
		</ul>
		<a href="page/create">Create a new page</a>
	

</body>
```

Ao ver esse page/1 e page/2 a primeira coisa que eu penso é testar se existe algum problema de *IDOR* (já existe um post sobre [IDOR aqui no blog](https://luizleiteoliveira.github.io/seguran%C3%A7a/owasp/2021/06/30/idor_GUIA/#/)).

Após acessar os links vieram que existe um código para edição que é bem-parecido com o link da página com o `path` como um dos parâmetros de acesso. Além disso, ao criar uma página o código inicial alterou para:

```html
<body data-new-gr-c-s-check-loaded="14.1022.0" data-gr-ext-installed="">
		<ul>
<li><a href="page/1">Testing</a></li>
<li><a href="page/2">Markdown Test</a></li>
<li><a href="page/10">teste</a></li>
<li><a href="page/11">create first page</a></li>
		</ul>
		<a href="page/create">Create a new page</a>
	

</body>
```

Interessante prestar atenção queo código pulou diretamente para o 10 e depois foi para 11, ou seja, a criação é sequencial e vale a pena testar os identificadores de 2 a 9 para ver o que acontece. O resultado de 3 a 6 foi `Not Found` agora quando cheguei no 7 o resultado foi um `403`que avisando que eu não tenho permissão de acessar o recurso.

Após fazer isso a primeira coisa que tento é alterar o link de `/page/7` para `/page/edit/7` que é o endereço de edição das páginas. Ao fazer isso a primeira flag já fica exposta no conteúdo e já podemos submeter.

## Flag 2

Ainda quando olhava a outra flag vi que os cadastros permitem Markdown, isso é bem legal, mas por baixo que podem permitir XSS, existem 3 lugares que pode ser testados, URL , titulo e body da página(que possui um botão em uma das páginas).

Vou começar com o título ao adicionar o seguinte código `<script>alert('xss')</script>` que caso seja executado vai mostrar um alerta na tela.

Após salvar e navegar é exibido no alerta a segunda flag.

## Flag 3

Agora que já aceitou o XSS do título vamos testar no body para ver se tem algum efeito com o mesmo código.

```html
<script>alert('xss')</script>
<button>Some button</button>
```

Diretamente no corpo não aconteceu nada, mas como o botão renderizou vou testar uma ação dentro dele para ver se acontece algo alterando o código para:

```html
<button onclick="<script>alert('xss')</script>">Some button</button>
```

Após fazer isso não apareceu nada, mas ao inspecionar o código da página, aparece o seguinte trecho:

```html
<p><button flag="^FLAG^A_FLAG_encontrada$FLAG$" onclik="<scrubbed>alert('xss')</scrubbed>" &gt;some="" button<="" button=""><p></
</button></p>
```

## Flag 4

Para finalizar vamos testar a URL, comecei a tentar passar um script via URL e nada aconteceu. Depois de um tempo comecei a pensar se a URL permitia SQL injection e a maneira mais fácil de testar isso é ao colocar o caracter `'` ao final.

Primeiro testei na página de visualização de cada item e não funcionou, depois ao testar no path da seguinte forma `page/edit/id'` só adicionando a apostrofe no final da URL o site já redireciona para uma página com a última flag.

## Fim do desafio
Ao finalizar conseguimos mais 8 pontos, com mais 1 do último já temos 9 dos 26 necessários para participar de um programa fechado da _Hacker One_

## _Quer me acompanhar?_
 
_Aqui estão algumas das minhas redes sociais._

    
 GitHub: [luizleite-hotmart](https://github.com/luizleite-hotmart)
    
 Twitter: luizleite_
    
 Twitch: [coffee_and_code](https://www.twitch.tv/coffee_and_code)
    
 Linkedin: [luizleiteoliveira](https://www.linkedin.com/in/luizleiteoliveira/)
 
