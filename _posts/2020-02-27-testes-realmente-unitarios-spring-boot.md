---
layout: post
title:  "Testes realmente unitários no Spring Boot"
summary: Testes realmente unitários no Spring Boot
author: luizleite
date: '2020-02-13 14:35:23 +0530'
category: spring-boot
thumbnail: /assets/img/posts/spring-logo.svg
---

### Introdução
A um tempo fui rodar um teste que tinha a anotação `@SpringBootTest` e quando começou a rodar, já apareceu o Banner do Spring Boot
e uma mensagem:
` contextLoader = 'org.springframework.boot.test.context.SpringBootContextLoader'`

Quando eu vi isso, eu já vi que eu nem sabia o por quê de utilizar essa anotação e o qual deveria usar. Então o objetivo
deste post é de mostrar quais devem ser utilizadas pra qual cenário especifico.

###  Setup Inicial
Para o nosso setup inicial, vamos utilizar o mínimo e o pom vai precisar das seguintes depêndencias:

```xml
        <dependency>
            <groupId>com.h2database</groupId>
            <artifactId>h2</artifactId>
            <scope>runtime</scope>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
            <exclusions>
                <exclusion>
                    <groupId>org.junit.vintage</groupId>
                    <artifactId>junit-vintage-engine</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
        </dependency>
```  
a exclusão do `junit-vintage-engine` é necessária para utilização do JUnit5 que utiliza o `junit-jupiter-engine` .
O uso do H2 vai ser para testar a camada de dados.

### Camada de serviços e DTOs

Vamos criar uma classe bem simples pra fazer alguns testes, para isso vamos criar um usuário:

```java
public class User {

    private String name;
    private int age;
    private String document;

    public User(String name, int age, String document) {
        this.name = name;
        this.age = age;
        this.document = document;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public String getDocument() {
        return document;
    }

    public void setDocument(String document) {
        this.document = document;
    }

    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                ", age=" + age +
                ", document='" + document + '\'' +
                '}';
    }
}
```

e o teste que vai executar sobre essa classe vai ser:

```java
import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.junit.jupiter.api.Assertions.assertTrue;


public class EntityTest {

    @Test
    public void UserTest() {
        User user = new User("name", 12, "DOC12543");
        assertEquals("name", user.name);
        assertTrue(user.toString().contains("User{"));
    }
}
```
E deste jeito já tá rodando os nossos testes em 135ms enquanto se utilizarmos a notação `@SpringBootTest` roda em 459ms,
parece pouco mas vamos lembrar que esse teste é o mais simples e nossa aplicação não tem nada configurado. Mesmo assim
a diferenção entre os dois testes já foi mais que o triplo, isso só pra validar um DTO.

### Camada de dados

Para testar nossa camada de dados vamos dar colocar em nossa classe `User` um atributo `@Entity` e o _ID_ que fica obrigatório:
```java
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;

@Entity
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private Long id;
    private String name;
    private int age;
    private String document;

    public User(String name, int age, String document) {
        this.name = name;
        this.age = age;
        this.document = document;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public String getDocument() {
        return document;
    }

    public void setDocument(String document) {
        this.document = document;
    }

    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                ", age=" + age +
                ", document='" + document + '\'' +
                '}';
    }

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }
}

```
E o acesso vamos fazer via `JpaRepository` com apenas um método:

```java
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface UserRepository extends JpaRepository<User, Long> {

    public User findUserByNameAndDocument(String name, String doc);
}
``` 

Depois de ter criado essas classes já podemos criar alguns testes para essa parte. Alguns exemplos podem ser vistos a seguir:

```java
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.orm.jpa.DataJpaTest;
import org.springframework.test.context.junit.jupiter.SpringExtension;

import static org.junit.jupiter.api.Assertions.*;

@ExtendWith(SpringExtension.class)
@DataJpaTest
public class UserRepositoryTests {

    @Autowired
    private UserRepository userRepository;

    @Test
    public void insertUser() {
        User user = new User("name", 11, "DOC12355");
        userRepository.save(user);
        Integer countUser = userRepository.findAll().size();
        assertEquals(1, countUser);
    }

    @Test
    public void checkUserSavedWithDocument() {
        User user = new User("name", 11, "DOC12355");
        userRepository.save(user);
        Integer countUser = userRepository.findAll().size();
        assertEquals(1, countUser);
        User user1 = userRepository.findUserByNameAndDocument("name", "DOC12355");

        assertNotNull(user1);
        assertEquals(user, user1);
    }

    @Test
    public void checkUserSavedWithDocumentPassingOtherDocumentShouldReturnNull() {
        User user = new User("name", 11, "DOC12355");
        userRepository.save(user);
        Integer countUser = userRepository.findAll().size();
        assertEquals(1, countUser);
        User user1 = userRepository.findUserByNameAndDocument("name", "99999");

        assertNull(user1);
    }
}

```
Utilizando `@DataJpaTest` ajuda a configurar algumas coisas automagicamente:

- Configuração de H2 in-memory
- Spring Data, Datasource
- Modo de logar o SQL ON

o modo de logar o sql para o nosso caso fica assim:

```mysql
insert into user (age, document, name, id) values (?, ?, ?, ?)
select user0_.id as id1_0_, user0_.age as age2_0_, user0_.document as document3_0_, user0_.name as name4_0_ from user user0_
select user0_.id as id1_0_, user0_.age as age2_0_, user0_.document as document3_0_, user0_.name as name4_0_ from user user0_ where user0_.name=? and user0_.document=?
```
Esse teste com `@SpringBootTest` não roda.


### Camada Web


Para camada web a gente já fez um post [aqui](https://dev.to/luizleite_/como-fazer-testes-unitarios-em-controllers-de-um-app-spring-boot-1bbm)
mas para o nosso caso de usuário vamos criar um controller para usuário, o mais simples seria o que retorna todos os usuários. Ficaria mais ou menos assim:

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.ResponseBody;

import java.util.List;

@Controller
public class UserController {

    @Autowired
    private UserRepository userRepository;

    @GetMapping(value = "/users")
    @ResponseBody
    public List<User> findAllUsers() {
        return userRepository.findAll();
    }
}
``` 

E um teste simples para resposta desse controller seria:
```java

import org.hamcrest.Matchers;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.WebMvcTest;
import org.springframework.boot.test.mock.mockito.MockBean;
import org.springframework.test.context.junit.jupiter.SpringExtension;
import org.springframework.test.web.servlet.MockMvc;

import java.util.List;

import static org.hamcrest.Matchers.containsString;
import static org.mockito.Mockito.when;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.content;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;

@ExtendWith(SpringExtension.class)
@WebMvcTest(controllers = UserController.class)
class UserControllerTest {

    @Autowired
    private MockMvc mockMvc;

    @MockBean
    private UserRepository userRepository;

    @Test
    public void findAllUsers() throws Exception {
        User user = new User("teste", 25, "DOC12346");
        List<User> userList = List.of(user);
        when(userRepository.findAll()).thenReturn(userList);
        this.mockMvc.perform(get("/users"))
                .andExpect(status().isOk())
                .andExpect(content().string(containsString("DOC12346")));
    }

}
```

Dentro desse teste utilizamos o MockMvc para fazer a chamada do controller, lembrando que isso é uma chamada "fake",
e o `@MockBean` para executar um mock do nosso repository. Outra coisa que fizemos foi adicionar.

## Conclusão

Além destes existem mais algumas configurações automaticas que você pode checar [aqui](https://docs.spring.io/spring-boot/docs/current/reference/html/appendix-test-auto-configuration.html),
mas sempre surgem novas ferramentas. Mas utilizar a abordagem correta para parte que você deseja testar, vai facilitar o uso
além de te dar velocidade, tanto na hora de rodar, quanto na hora do desenvolvimento

## _Gostou?_
 
_Se gostou aqui estão minhas redes sociais para acompanhar os novos posts. _

    
 GitHub: [luizleite-hotmart](https://github.com/luizleite-hotmart)
    
 Twitter: luizleite_
    
 Twitch: [coffee_and_code](https://www.twitch.tv/coffee_and_code)
    
 Linkedin: [luizleiteoliveira](https://www.linkedin.com/in/luizleiteoliveira/)
    
 