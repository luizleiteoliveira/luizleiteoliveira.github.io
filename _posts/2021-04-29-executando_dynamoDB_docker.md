---
layout: post
title:  "Como Rodar DynamoDB em docker local"
summary: Neste post vou mostrar como em poucos comandos colocamos para rodar o DynamoDB em um container docker.
author: luizleite
date: '2020-04-29 19:00:23 +0530'
category: ['docker']
thumbnail: /assets/img/posts/Moby-logo.png
---

## Mas pra que?
Antes de começar um projeto e precisar de uma conexão com chaves da AWS esse recurso vai encurtar muito o tempo da sua primeira POC.
No futuro esse simples passo pode te ajudar a construir testes robustos para uma aplicação até com cenários mais controlados.

## Pré requisitos

 - Docker 
 - AWS CLI

## Como fazer?

Primeiro o que é necessário fazer o pull diretamente do [dockerHub](https://hub.docker.com/r/amazon/dynamodb-local/)

 `docker pull amazon/dynamodb-local`

Depois podemos executar, nesse caso testamos a porta 8000 para acesso

`docker run -p 8000:8000 amazon/dynamodb-local`

após executar deverá aparecer uma mensagem como a seguinte

```shell
Initializing DynamoDB Local with the following configuration:
Port:   8000
InMemory:       true
DbPath: null
SharedDb:       false
shouldDelayTransientStatuses:   false
CorsParams:     *
```


## Testando 
Agora utilizando o AWS CLI vamos executar alguns comandos para testar o docker. Após cada resposta é necessário escrever `:q` para sair.
Todos os comandos que podem ser executados podem ser vistos [aqui](https://docs.aws.amazon.com/cli/latest/reference/dynamodb/index.html).

### Criar tabelas:

Comando para criar uma tabela de chamada `Usuario`. A tabela possui `Nome` como chave(key) e `Email` como sort key.
```shell
aws dynamodb create-table \
    --table-name Usuario \
    --attribute-definitions \
        AttributeName=Nome,AttributeType=S \
        AttributeName=Email,AttributeType=S \
    --key-schema AttributeName=Nome,KeyType=HASH AttributeName=Email,KeyType=RANGE \
    --provisioned-throughput ReadCapacityUnits=1,WriteCapacityUnits=1 \
    --endpoint-url http://localhost:8000
```

Comando para criar uma tabela de chamada `Livro`. A tabela possui `ISBN` como chave(key) e `Nome` como sort key.

```shell
aws dynamodb create-table \
    --table-name Livro \
    --attribute-definitions \
        AttributeName=ISBN,AttributeType=S \
        AttributeName=Nome,AttributeType=S \
    --key-schema AttributeName=ISBN,KeyType=HASH AttributeName=Nome,KeyType=RANGE \
    --provisioned-throughput ReadCapacityUnits=1,WriteCapacityUnits=1 \
    --endpoint-url http://localhost:8000
```

Após cada criação aparece um comando como o seguinte:
```shell
{
    "TableDescription": {
        "AttributeDefinitions": [
            {
                "AttributeName": "ISBN",
                "AttributeType": "S"
            },
            {
                "AttributeName": "Nome",
                "AttributeType": "S"
            }
        ],
        "TableName": "Livro",
        "KeySchema": [
            {
                "AttributeName": "ISBN",
                "KeyType": "HASH"
            },
            {
                "AttributeName": "Nome",
                "KeyType": "RANGE"
            }
        ],
        "TableStatus": "ACTIVE",
        "CreationDateTime": "2021-04-29T18:41:27.467000-03:00",
        "ProvisionedThroughput": {
            "LastIncreaseDateTime": "1969-12-31T21:00:00-03:00",
            "LastDecreaseDateTime": "1969-12-31T21:00:00-03:00",
            "NumberOfDecreasesToday": 0,
            "ReadCapacityUnits": 1,
            "WriteCapacityUnits": 1
        },
        "TableSizeBytes": 0,
        "ItemCount": 0,
        "TableArn": "arn:aws:dynamodb:ddblocal:000000000000:table/Livro"
    }
}
```

### Listar tabelas:
`aws dynamodb list-tables --endpoint-url http://localhost:8000`

### Put de item
Inserindo item na tabela.

```shell
aws dynamodb put-item \
    --table-name Livro \
    --item \
        '{"ISBN": {"S": "1298DSAHI"}, "Nome": {"S": "Livro do Luiz"}}' \
    --return-consumed-capacity TOTAL  \
    --endpoint-url http://localhost:8000
```

Inserindo item na tabela Livro com autor.

```shell
aws dynamodb put-item \
    --table-name Livro \
    --item \
        '{"ISBN": {"S": "12343AOIDJOFA"}, "Nome": {"S": "Livro do Luiz2"}, "Autor": {"S": "Luiz Leite Oliveira"}}' \
    --return-consumed-capacity TOTAL  \
    --endpoint-url http://localhost:8000
```

Como resultado recebemos:

```shell
{
    "ConsumedCapacity": {
        "TableName": "Livro",
        "CapacityUnits": 1.0
    }
}
```


### Consultando registros:

Para consulta basta executar o comando query:

Query que não tem retorno pois não existe o `ISBN`

```shell
aws dynamodb query --table-name Livro --key-conditions \
  '{"ISBN" : {"AttributeValueList": [{"S": "TESTE_NAO_EXISTE"}],"ComparisonOperator": "EQ"}}' \
 --endpoint-url http://localhost:8000
```

Query que retorna livro que possuí autor:

```shell
aws dynamodb query --table-name Livro --key-conditions \
  '{"ISBN" : {"AttributeValueList": [{"S": "12343AOIDJOFA"}],"ComparisonOperator": "EQ"}}' \
 --endpoint-url http://localhost:8000
```

Resultado:
```shell
{
    "Items": [
        {
            "ISBN": {
                "S": "12343AOIDJOFA"
            },
            "Nome": {
                "S": "Livro do Luiz2"
            },
            "Autor": {
                "S": "Luiz Leite Oliveira"
            }
        }
    ],
    "Count": 1,
    "ScannedCount": 1,
    "ConsumedCapacity": null
}
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
 