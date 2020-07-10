---
layout: post
title:  "Grapqhl on Quarkus"
summary: How to Use GraphQl on Quarkus
author: Luiz Leite
date: '2020-07-02 14:35:23 +0530'
category: Java
thumbnail: /assets/img/posts/code.jpg
---

Tudo que está nesse projeto pode ser encontrado no repositório [GitHub](https://github.com/luizleite-hotmart/quarkus-graphql).

Para a nossa poc o que fizemos foi primeiro pegar.

## Intro ao projeto 

Esse projeto já foi contextualizado no [dev.to](https://dev.to/luizleite_/como-usar-graphql-com-quarkus-ndn). Basicamente o que ele faz
é pegar usar o get da api encontrada em `https://randomuser.me/api` e aplica um GraphQL na resposta.

## GRAPHQL On Quarkus

### Dependências

Para utilizar o GraphQL neste projeto utilizamos uma dependência da [Vert.x](https://vertx.io/), documentação completa você pode encontrar
[aqui](https://vertx.io/docs/vertx-web-graphql/java/) para importar basta adicionar em seu pom.xml

```xml
    <dependency>
      <groupId>io.vertx</groupId>
      <artifactId>vertx-web-graphql</artifactId>
    </dependency>
```

### Vamos para o código

Antes de usar o GraphQL precisamos modelar alguns DTO's para nossa modelagem:

```java
public class Names {

    private String title;
    private String first;
    private String last;
    
//    Getters and Setters

}
```

Para esse modelo precisamos utilizaremos um Fetcher, que deve ficar da seguinte forma:

```java
public class UserFetcher implements DataFetcher<List<User>> {

    private final UserClient userClient;

    public UserFetcher(UserClient userClient) {
        this.userClient = userClient;
    }


    private Results getResult() {
        return userClient.getUsers();
    }

    @Override
    public List<User> get(DataFetchingEnvironment environment) throws Exception {
        return getResult().getResults();
    }
}
``` 
    
### Producer

Para pegar os resultados, vamos precisar também de uma classe producer como a seguir, é um pouco de boilerplate, vou tentar fazer uma
extensão de quarkus para reduzir essa e outras partes que são repetitivas, enquanto isso não acontece, a classe fica da seguinte forma:

 ```java
 public class GraphQLProducer {
 
     private Logger LOGGER = LoggerFactory.getLogger(GraphQLProducer.class);
 
     @RestClient
     @Inject
     private UserClient userClient;
 
     @Produces
     public GraphQL setup() {
 
         LOGGER.info("Setting up GraphQL..");
 
         SchemaParser schemaParser = new SchemaParser();
         TypeDefinitionRegistry registry = schemaParser.parse(
             new InputStreamReader(
                 Objects.requireNonNull(Thread.currentThread().getContextClassLoader()
                     .getResourceAsStream("META-INF/resources/graphql.schema"))));
 
         SchemaGenerator schemaGenerator = new SchemaGenerator();
         GraphQLSchema graphQLSchema = schemaGenerator.makeExecutableSchema(registry, wirings());
         return GraphQL.newGraphQL(graphQLSchema).build();
     }
 
     private RuntimeWiring wirings() {
 
         LOGGER.info("Wiring queries..");
 
         return RuntimeWiring.newRuntimeWiring()
             .type("Query",
                 builder -> builder.dataFetcher("allUsers", new UserFetcher(userClient)))
             .build();
     }
 
 
 }
 ```
Na parte de `getResourceAsStream()` você coloca o schema de graphql que você desejar, no nosso caso ficou o `META-INF/resources/graphql.schema`

```
type Names {
  first: String
  last: String
}


type User {
  email: String
  name: Names
  gender: String
  phone: String
  cell: String
  nat: String
}

type Query {
  allUsers : [User]
}
```

Além disto, na parte de `wirings()` colocamos quem será o Fetcher que vai ser utilizado para `allUsers` que foi declarado dentro do `graphql.schema`

Finalizando temos que declarar a parte de Rotas (outra parte que é um pouco boilerplate que também pode entrar na extensão). 

```java
@ApplicationScoped
public class Routes {

    @Inject
    GraphQL graphQL;

    public void init(@Observes Router router) {

        boolean enableGraphiQL = true;

        router.route("/graphql")
            .handler(new GraphQLHandlerImpl(graphQL, new GraphQLHandlerOptions()));
        router.route("/graphiql/*").handler(
            GraphiQLHandler.create(new GraphiQLHandlerOptions().setEnabled(enableGraphiQL)));
    }

}
```

Foram declaradas duas rotas:
 
 1. `/graphql` &#8594; onde ficam os "endpoints" para acesso dos usuários
 2. `/graphiql/*` &#8594; onde fica a documentação para o que foi registrado.
 
 ## Concluindo
 Resumindo tudo que fizemos não é muito complicado depois da primeira vez, mas falta muita documentação e várias vezes vi 
 lugares mostrando como se fosse meio que mágica, como se fizesse um import e já funciona. Depois de muitos testes funciona
 bem, mas como já disse existem muitas partes que podem ser eliminadas por uma extensão mais completa.
 
 Outra coisa que ficou muito obscuro e difícil são as headers como authorização não é muito simples de manipular e fazer alguma
validação com elas.

Com isso tudo não acho uma boa opção comparando com o que já tem em Spring Boot, para esse caso ainda não está maduro suficiente,
mas é claro que não está muito longe e, é fácil de ver como chegariam nesse ponto.

 ## _Quer acompanhar um pouco mais?_ 
 Me siga nas plataformas._

GitHub: [luizleite-hotmart](https://github.com/luizleite-hotmart)

Twitter: luizleite_

Twitch: [coffee_and_code](https://www.twitch.tv/coffee_and_code)

Linkedin: [luizleiteoliveira](https://www.linkedin.com/in/luizleiteoliveira/)

dev.to: [luizleite_](https://dev.to/luizleite_)