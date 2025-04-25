---
title:  "Utilizando MongoDB dentro do Quarkus com Panache"
summary: Neste post vai ser mostrado como é simples utilizar o MongoDB dentro do Quarkus com Panache.
author: luizleite
published: 2021-06-22
category: 'mongodb'
thumbnail: /assets/img/quarkus_logo.png
---

## Objetivo do projeto

No projeto vamos mostrar um exemplo básico de como configurar e fazer alguns endpoints para interagir com o 
MongoDB. O projeto consiste em ter uma entidade chamada **_Book_**, essa entidade possui 3 campos **_name_**, **_author_**  e 
**_yearPublication_**. Para adicionar alguma lógica a mais vamos ter que não deve ser possível ter dois livros com o mesmo nome.


## Pré requisitos

 - Java 11
 - Maven

## Configuração inicial

A configuração inicial foi feita dentro do próprio site do _Quarkus_ em [https://code.quarkus.io/](https://code.quarkus.io/).
Ao gerar o código já será possível ver uma classe de _resource_, e o **_pom.xml_** com _dependencyManagement_ para versão mais
atualizada do _Quarkus_. 

Além da configuração básica foram adicionadas as seguintes dependências:

 - _quarkus-mongodb-panache_ -> Realizar a comunicação com o MongoDB 

```xml
<dependency>
   <groupId>io.quarkus</groupId>
   <artifactId>quarkus-mongodb-panache</artifactId>
</dependency>
```

 - quarkus-resteasy-jsonb -> Formatação de respostas para _JSON_.

```xml
<dependency>
    <groupId>io.quarkus</groupId>
    <artifactId>quarkus-resteasy-jsonb</artifactId>
</dependency>
```


Como o projeto já usa _dependencyManagement_ não é necessário declarar as versões das dependências acima.

## O projeto

O projeto tem três classes para realizar as operações.

### Book

Esta classe vai ser a de domínio, com isso, possui os campos que vão ser salvos, além disso, também vão ser adicionadas classes e
métodos para fazer as operações no _MongoDB_.


```java
import io.quarkus.mongodb.panache.MongoEntity;
import io.quarkus.mongodb.panache.PanacheMongoEntity;

@MongoEntity(collection = "book")
public class Book extends PanacheMongoEntity {

    public String name;
    public String author;
    public Integer yearPublication;

    public static Book findByName(String nameToSearch) {
        return find("name", nameToSearch).firstResult();
    }
}
```

Podemos verificar que a classe possui os seguintes elementos:

 - **_MongoEntity_** -> Caso a collection do banco não possua o mesmo nome da entidade é necessário fazer essa declaração.
 - **_PanacheMongoEntity_** -> Permite a classe executar as operações de banco.
 - **_findByName_** -> Retorna elemento executando uma query em que o campo _name_ é igual ao parâmetro.


### BookService

Responsável por chamar a classe **_Book_** para executar as operações além de executar as regras de negócio.

```java
import javax.enterprise.context.ApplicationScoped;
import java.util.List;

@ApplicationScoped
public class BookService {


    public Book createOrUpdateBook(Book bookReceived) {
        Book book = Book.findByName(bookReceived.name);

        if ( book != null) {
            book.author = bookReceived.author;
            book.name = bookReceived.name;
            book.yearPublication = bookReceived.yearPublication;
            book.update();
        } else {
            book = new Book();
            book.author = bookReceived.author;
            book.name = bookReceived.name;
            book.yearPublication = bookReceived.yearPublication;
            book.persist();
        }
        return book;
    }

    public List<Book> findAllBooks() {
        List<Book> allBooks = Book.listAll();
        return allBooks;
    }

    public void deleteAllBooks() {
        Book.deleteAll();
    }

    public Book findBookByName(String name) {
        return Book.findByName(name);
    }
}

```

Na classe podemos verificar temos:
   
 - **createOrUpdateBook** Recebe um livro para ser salvo, note que há uma verificação ver se o livro já existe ou não.
   
 - **findAllBooks** Busca todos os livros.
   
 - **deleteAllBooks** Remove todos os livros que foram salvos.
   
 - **findBookByName** Busca um livro pelo nome.


### BookResource

Classe que possui todos os endpoints que são utilizados para executar operações definidas.

```java
import com.luizleiteoliveira.tutorials.domain.Book;
import com.luizleiteoliveira.tutorials.service.BookService;

import javax.inject.Inject;
import javax.ws.rs.*;
import javax.ws.rs.core.MediaType;
import java.util.List;

@Consumes(MediaType.APPLICATION_JSON)
@Produces(MediaType.APPLICATION_JSON)
@Path("/books")
public class BookResource {

    @Inject
    BookService bookService;

    @GET
    public Book findBookByName(String name) {
        return bookService.findBookByName(name);
    }

    @POST
    public Book createBookWithParameters(Book bookReceived) {
        return bookService.createOrUpdateBook(bookReceived);
    }

    @GET
    @Path("/findAll")
    public List<Book> listAllBooks() {
        return bookService.findAllBooks();
    }

    @DELETE
    @Path("/deleteAll")
    public void deleteAll() {
        bookService.deleteAllBooks();
    }
}
```

### application.properties

Arquivo de configuração, no caso atual possui apenas atributos para o mongo

```properties
quarkus.mongodb.connection-string = mongodb://localhost:27017
quarkus.mongodb.database = book
```

 - **_quarkus.mongodb.connection-string_** -> url de conexão com o mongo 
 - **_quarkus.mongodb.database_** -> database (lembre-se que pode não ser igual a collection essa)

## Conclusão

O projeto mostra como é simples e pode evoluir muito mais como o padrão de [repositório](https://quarkus.io/guides/mongodb-panache#solution-2-using-the-repository-pattern).
Todo projeto pode ser visto no [Github](https://github.com/luizleiteoliveira/tutorials/tree/main/quarkus_mongodb), se tiver sugestões pode mandar.

## _Want to follow me?_
 
_You can get in contact me on this social media._

    
 GitHub: [luizleite-hotmart](https://github.com/luizleite-hotmart)
    
 Twitter: luizleite_
    
 Twitch: [coffee_and_code](https://www.twitch.tv/coffee_and_code)
    
 Linkedin: [luizleiteoliveira](https://www.linkedin.com/in/luizleiteoliveira/)
 