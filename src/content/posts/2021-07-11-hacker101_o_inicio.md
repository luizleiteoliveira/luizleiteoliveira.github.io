---
title: "Hacker101 o início do CTF"
summary: Nest post vamos começar os primeiros passos recomendados pelo _Hacker One_ para se tornar um hacker e identificar falhas do sistema. 
author: luizleite
published: 2021-07-11
category: 'hacker'
thumbnail: /assets/img/posts/OWASP.png
---

## Hacker101 o início do CTF

## Intro

A um tempo comecei a ter mais preocupação com questões de segurança e vulnerabilidadades, a partir daí comecei a pesquisar sobre locais que me mostrassem como detectar falhas e evoluir cada vez mais nesse sentido. Um dos sites mais famosos que conta até com um programa de bug bounty é o [Hacker One](https://hackerone.com/), então dando uma navegada caí em um site que eles mantém para ensinar algumas técnicas e ferramentas para quem está começando esta jornada.

O site é o [Hacker101](https://www.hacker101.com/) que contém um conteúdo de primeiros passos, conteúdos, ferramentas, e CTF's para iniciar a se familizarizar com este universo. A parte que decidi comentar neste post são os primeiros CTF que estão disponíveis.

## O que é um CTF

CTF significa `Capture the flag` um jogo em que você tem que encontrar a bandeira que está em uma base, dentro da *Hacker101* é um ambiente seguro para testar/aprender como utilizar hacks.

A idéia é fazer alterações, tentar invadir a base, verificar no código fonte e assim localizar uma chave que no site tem o formato `^FLAG^37ae568362f974017fa575f08cd215044cd6bb395c3f5e5e293ee5324ba6769c$FLAG$` o que deixa bem claro quando encontramos.

## Como começar

Antes de começar deve-se criar uma conta na *Hacker One* e vincular a conta [aqui](https://ctf.hacker101.com/auth). Após vincular a conta já aparece uma página com os desafios e uma parte de *invitations*, lendo dentro da [documentação](https://docs.hackerone.com/hackers/hacker101.html#hacker101-ctf) eles mostram que toda vez que você completa 26 seu perfil será colocado como prioridade para entrar em programas privados do site.

![https://docs.hackerone.com/static/7fb10ab59a3437e34f2595c5d23642d9/0b533/hacker101-5.png](https://docs.hackerone.com/static/7fb10ab59a3437e34f2595c5d23642d9/0b533/hacker101-5.png)

Note também que existe a dificuldade de cada problema, percentual de completude, conhecimento necessário, e algumas dicas para concluir a tarefa.

## O primeiro problema (A little something to get started)

Como está no início vamos começar do problema com dificuldade trivial, e depois, vamos evoluindo semana após semana.

Abrindo o GO que a página disponibiliza é aberta uma página apenas com a seguinte mensagem.

`Welcome to level 0. Enjoy your stay.`

Com isso a primeira coisa que penso em fazer é inspecionar o código dessa tela(botão direito inspecionar).

Verificando dentro da parte de source o código do index está da seguinte forma:

```html
<!doctype html>
<html>
	<head>
		<style>
			body {
				background-image: url("background.png");
			}
		</style>
	</head>
	<body>
		<p>Welcome to level 0.  Enjoy your stay.</p>
	</body>
</html>
```

Ou seja, existe uma imagem de background que está sendo adicionada na página. Verificando a aba de *Network* do console é possível  ver a url que está a imagem.

![https://res.cloudinary.com/luizleiteoliveira/image/upload/v1626026141/blog/Captura_de_Tela_2021-07-11_a%CC%80s_14.55.33_vzbjy0.png](https://res.cloudinary.com/luizleiteoliveira/image/upload/v1626026141/blog/Captura_de_Tela_2021-07-11_a%CC%80s_14.55.33_vzbjy0.png)

Acessando a url já aparece a chave, e realmente não há dúvida que encontramos a primeira flag.

![https://res.cloudinary.com/luizleiteoliveira/image/upload/v1626026282/blog/Captura_de_Tela_2021-07-11_a%CC%80s_14.57.45_lry8re.png](https://res.cloudinary.com/luizleiteoliveira/image/upload/v1626026282/blog/Captura_de_Tela_2021-07-11_a%CC%80s_14.57.45_lry8re.png)

Depois é só fazer o submit da flag encontrada [aqui](https://ctf.hacker101.com/ctf/flagcheck) e concluimos nosso primeiro desafio. \o/

## _Quer me acompanhar?_
 
_Aqui estão algumas das minhas redes sociais._

    
 GitHub: [luizleite-hotmart](https://github.com/luizleite-hotmart)
    
 Twitter: luizleite_
    
 Twitch: [coffee_and_code](https://www.twitch.tv/coffee_and_code)
    
 Linkedin: [luizleiteoliveira](https://www.linkedin.com/in/luizleiteoliveira/)
 
