---
layout: post
title: "AWS API Gateway Introdução"
summary: Nest post vamos mostrar um pouco do que é o serviço da AWS chamado API Gateway e o poder que ele pode ter ao dentro de uma arquitetura. 
author: luizleite
date: '2021-07-22 19:00:23 +0530'
category: ['aws']
thumbnail: /assets/img/posts/aws_logo.png
---

# AWS API Gateway Introdução

## Intro

***AWS API Gateway*** é uma feature que existe na *AMAZON* desenhado para criação, monitoramento, e proteção de API's. O serviço funciona como um portão da entrada para qualquer serviço que deseja configurar, ou seja, pode ser utiliza-lo para fazer um filtro para outras features, adicionando o ponto autenticação dentro do API Gateway para tirar essa responsabilidade do seu microsserviço.

![https://d1.awsstatic.com/serverless/New-API-GW-Diagram.c9fc9835d2a9aa00ef90d0ddc4c6402a2536de0d.png](https://d1.awsstatic.com/serverless/New-API-GW-Diagram.c9fc9835d2a9aa00ef90d0ddc4c6402a2536de0d.png)

Na imagem podemos ver a idéia de como utilizar, ele vai ser o primeiro contato do mundo externo com os serviços que estão dentro da AWS, como *AWS Lambda,* Amazon EC2, *Kinesis...*

O princípio é bem simples, mas isso cria uma porta de fácil configuração para uma arquitetura puramente *serverless* utilizando Lambda ou qualquer outro serviço.

## Precificação

API Gateway está disponível para *Free Tier* (Nível Gratuito) para uma quantidade de até 1 milhão de chamadas e 750.000 minutos de conexão, com isso, é possível fazer uma quantidade muito grande de iterações antes de ser cobrado.

Fora desse nível a cobrança é de um dolar a cada milhão de chamadas HTTP e $3,5 para chamadas Rest, além disso, é importante lembrar que caso o serviço seja ligado a um *Lambda* as cobranças serão separadas.

## Cadastro

Dentro deste [link](https://console.aws.amazon.com/apigateway/main/precreate) podemos ver que existem 4 tipos disponíveis para cadastros.

- HTTP API → Funciona com *lambda* ,  *backends ,* aconselhado para  aplicações de OAuth, OIDC.
- WebSocket API → Funciona com lambda, HTTP, AWS services bom para situações que esperam a resposta.
- REST API → Configuração mais cara em que é possível criar uma API totalmente costumizável.
- REST API(Private) → Mesma coisa, mas funciona apenas em uma VPC.

Dentro de cada tipo é possível fazer o cadastro via Wizard falando qual recurso a vai ser acessado como lambda ou API.

É possível configurar rotas diferentes para cada serviço, com isso, podemos fazer um lambda para um path e outro para um kinesis.

![https://res.cloudinary.com/luizleiteoliveira/image/upload/v1626109594/blog/api-gateway/Captura_de_Tela_2021-07-12_a%CC%80s_14.04.42_xlaedq.png](https://res.cloudinary.com/luizleiteoliveira/image/upload/v1626109594/blog/api-gateway/Captura_de_Tela_2021-07-12_a%CC%80s_14.04.42_xlaedq.png)

## Utilização

Ao finalizar o cadastro é disponibilizado uma URL para invocação do serviço descrito e já está disponível para uso. O serviço em é bem simples de utilizar/criar, o mais interessante dele é que a partir desse ponto pode ser feito como disparar uma arquitetura completa sem a necessidade de fazer um deploy de serviço, pode-se ativar lambda, filas, kinesis, e assim, poderia ficar todo a partir deste ponto as funcionalidades partiriam desta feature.

Para ajudar neste [link](https://github.com/luizleiteoliveira/tutorials/tree/main/aws_lambda_api_gateway) contém um exemplo de lambda que pode ser ligado no API-Gateway reescreve a resposta completa recebida. 

## _Quer me acompanhar?_
 
_Aqui estão algumas das minhas redes sociais._

    
 GitHub: [luizleite-hotmart](https://github.com/luizleite-hotmart)
    
 Twitter: luizleite_
    
 Twitch: [coffee_and_code](https://www.twitch.tv/coffee_and_code)
    
 Linkedin: [luizleiteoliveira](https://www.linkedin.com/in/luizleiteoliveira/)
 
