---
layout: post
title:  "Como utilizar DynamoDB dentro com Java"
summary: Neste post vai ser mostrado como pode-se ser feito a chamada para inserir dados e medido o tempo para esse registro aparecer após inserção.
author: luizleite
date: '2020-04-29 19:00:23 +0530'
category: ['dynamodb', 'java']
thumbnail: /assets/img/posts/aws_logo.png
---

## Objetivo do projeto
O projeto criado mostra como é simples fazer uma chamada dentro do Java, mesmo sem um framework como Spring Boot ou Quarkus.
Através dele foi possível verificar com a própria AWS sobre tempo de salvar e recuperar um dado através do [DAX](https://aws.amazon.com/pt/dynamodb/dax/).

## Pré requisitos

 - Java 11
 - Maven

## Configuração inicial

Para fazer a configuração é importante adicionar no projeto do Maven as seguintes dependências:

 - amazon-dax-client

```xml
    <dependency>
        <groupId>com.amazonaws</groupId>
        <artifactId>amazon-dax-client</artifactId>
        <version>1.0.221844.0</version>
    </dependency>
```

 - aws-java-sdk-bom
```xml
    <dependencies>
        <dependency>
            <groupId>com.amazonaws</groupId>
            <artifactId>aws-java-sdk-bom</artifactId>
            <version>1.11.889</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
```



## Conclusão
Além dessas operações existem muitas outras que podem ser executadas através do aws cli.
O banco que criamos está totalmente operacional e podemos criar uma boa idéia de quanto será usado recurso gasto
como unidades de leitura para o projeto, com esse dado mais o volume esperado pode-se chegar bem rápido a quando fica
o custo além de demonstrar que a modelagem pode funcionar ou não.

## _Want to follow me?_
 
_You can get in contact me on this social media._

    
 GitHub: [luizleite-hotmart](https://github.com/luizleite-hotmart)
    
 Twitter: luizleite_
    
 Twitch: [coffee_and_code](https://www.twitch.tv/coffee_and_code)
    
 Linkedin: [luizleiteoliveira](https://www.linkedin.com/in/luizleiteoliveira/)
 