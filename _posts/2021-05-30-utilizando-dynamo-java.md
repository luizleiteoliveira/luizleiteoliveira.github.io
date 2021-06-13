---
layout: post
title:  "Como utilizar DynamoDB dentro com Java"
summary: Neste post vai ser mostrado como pode-se ser feito a chamada para inserir dados e medido o tempo para esse registro aparecer após inserção.
author: luizleite
date: '2021-05-30 19:00:23 +0530'
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
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>com.amazonaws</groupId>
                <artifactId>aws-java-sdk-bom</artifactId>
                <version>1.11.889</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>
```


Notem que pode ser adicionado o valor `1.11.889` como variável de propriedade para evitar a repetição.

## O projeto

Agora o projeto vai ser dividido em _MainClass_ que vai ter os códigos que não tem ligação com o Client do _DynamoDB_. 
A segunda classe será DynamoClientHelper que possui as chamadas para construir o cliente e executar as ações no Dynamo.

### MainClass

A _MainClass_ pode ser substituído por uma classe sua como um serviço que vai consultar o _DynamoDB_ para tomar decisões.
Como foi feito nesta classe as chamadas o cliente os parâmetros como _accessKey_ e _secretKey_ foram repassados via argumentos.

Caso queira rodar o projeto sem alterações os parameters devem ficar na seguinte ordem:

`"NOME_DA_TABELA" "ACCESS_KEY" "SECRET_KEY" "DAX_ENDPOINT"`


### DynamoClientHelper

O client irá realizar tem os métodos para interagir com o _DynamoDB_, como:

 - **getDaxClient** Recebe a url de endpoint do _Dax_ e devolve um _DynamoClient_ que é utilizado para operações.
   
 - **getDynamoClient** Recebe as credenciais da AWS para conectar ao _DynamoDB_ da conta e retorna um client.
   
 - **writeData** Recebe a nome da tabela a ser utilizada, um client que foi gerado, e o _JSON_ de data.
   
 - **retrieveItem** Recebe a nome da tabela a ser utilizada, um client que foi gerado, e o _ID_ do 'item' a ser retornado.
   
 - **queryIndexCount** Recebe a nome da tabela a ser utilizada, um client que foi gerado, o nome do index, e ucode para value na query, retorna quantidade de
   itens no index.

 - **deleteItem** Recebe a nome da tabela a ser utilizada, um client que foi gerado, e o _ID_ do item a ser deletado.

## Conclusão
Apesar de ser muito simples o client criado, através dele foi possível fazer alguns testes, e verificar o tempo que 
é gasto pelo _DAX_ quando um item é inserido até que ele apareça na busca via index. Além disto pode ser feita algumas melhorias
em que é colocado o _DAX_ como client principal e caso falhe chame o via outra opção. Coisa importante é que através desse client
já é possível fazer o update, utilizando o método writeData utilizando o mesmo _ID_.

Alterando poucas coisas no client já temos um _CRUD_ completo e atender vários requisitos.

## _Want to follow me?_
 
_You can get in contact me on this social media._

    
 GitHub: [luizleite-hotmart](https://github.com/luizleite-hotmart)
    
 Twitter: luizleite_
    
 Twitch: [coffee_and_code](https://www.twitch.tv/coffee_and_code)
    
 Linkedin: [luizleiteoliveira](https://www.linkedin.com/in/luizleiteoliveira/)
 