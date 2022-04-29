---
layout: post
title:  "Como usar Rest Client em Quarkus"
summary: Neste post você vai aprender como pode ser simples criar um client em Quarkus para utilizar em seus projetos.
author: luizleite
date: '2020-03-04 14:35:23 +0530'
category: [ 'quarkus']
thumbnail: /assets/img/quarkus_logo.png
---

## Como usar Rest Client em Quarkus

Tudo que está nesse projeto pode ser encontrado no repositório [GitHub](https://github.com/luizleite-hotmart/quarkus-graphql).

Para criar o projeto utilizamos o próprio site de bootstrap do quarkus para criar o seu pode acessar [aqui](https://code.quarkus.io/).

## Dependências adicionadas

As dependências que vamos usar podem ser encontradas diretamente pelo bootstrap. São as seguintes:

 - JSON-B  _Utilizada para fazer json bind automático_
 - REST Cliente _Utilizada para criar chamada rest_
 
 Para quem usa maven as dependencias vão ficar assim:
 
 ```xml
     <dependency>
       <groupId>io.quarkus</groupId>
       <artifactId>quarkus-resteasy-jsonb</artifactId>
     </dependency>
     <dependency>
       <groupId>io.quarkus</groupId>
       <artifactId>quarkus-rest-client</artifactId>
     </dependency>
 ```

e em gradle:

```
io.quarkus:quarkus-rest-client
io.quarkus:quarkus-resteasy-jsonb
```

## Background do projeto 
Para facilitar a utilização do projeto vamos usar uma api já pronta que retorna usuários aleatórios, ela pode ser encontrada no site [randomuser.me](https://randomuser.me/)

Para ele vamos utilizar uma chamada no endpoint https://randomuser.me/api/

Quando fazemos um get nesse endpoint recebemos um usuário aleatório no formato de json como o a seguir:

```json
{
  "results": [
    {
      "gender": "male",
      "name": {
        "title": "mr",
        "first": "brad",
        "last": "gibson"
      },
      "location": {
        "street": "9278 new road",
        "city": "kilcoole",
        "state": "waterford",
        "postcode": "93027",
        "coordinates": {
          "latitude": "20.9267",
          "longitude": "-7.9310"
        },
        "timezone": {
          "offset": "-3:30",
          "description": "Newfoundland"
        }
      },
      "email": "brad.gibson@example.com",
      "login": {
        "uuid": "155e77ee-ba6d-486f-95ce-0e0c0fb4b919",
        "username": "silverswan131",
        "password": "firewall",
        "salt": "TQA1Gz7x",
        "md5": "dc523cb313b63dfe5be2140b0c05b3bc",
        "sha1": "7a4aa07d1bedcc6bcf4b7f8856643492c191540d",
        "sha256": "74364e96174afa7d17ee52dd2c9c7a4651fe1254f471a78bda0190135dcd3480"
      },
      "dob": {
        "date": "1993-07-20T09:44:18.674Z",
        "age": 26
      },
      "registered": {
        "date": "2002-05-21T10:59:49.966Z",
        "age": 17
      },
      "phone": "011-962-7516",
      "cell": "081-454-0666",
      "id": {
        "name": "PPS",
        "value": "0390511T"
      },
      "picture": {
        "large": "https://randomuser.me/api/portraits/men/75.jpg",
        "medium": "https://randomuser.me/api/portraits/med/men/75.jpg",
        "thumbnail": "https://randomuser.me/api/portraits/thumb/men/75.jpg"
      },
      "nat": "IE"
    }
  ],
  "info": {
    "seed": "fea8be3e64777240",
    "results": 1,
    "page": 1,
    "version": "1.3"
  }
}
```

Caso chamemos o mesmo endpoint mas dessa vez com um Query Param `results=N` ele traz N resultados pra gente.

## Modelagem

Os dados vão ser modelados da seguinte forma:

 ### Names.java
 A classe name é utilizada para criar  a parte `name` do usuário,  que tem os campos String `title`, `first`, `last`
 
 ```java
 package org.luizleiteoliveira.entity;
 
 public class Names {
 
     private String title;
     private String first;
     private String last;
 
     public String getTitle() {
         return title;
     }
 
     public void setTitle(String title) {
         this.title = title;
     }
 
     public String getFirst() {
         return first;
     }
 
     public void setFirst(String first) {
         this.first = first;
     }
 
     public String getLast() {
         return last;
     }
 
     public void setLast(String last) {
         this.last = last;
     }
 }

```  

### User.java
Para modelar o usuário a api vem com diversos campos, vamos utilizar além do objeto `name` , `email`, `gender`, 
`phone`, `cell`, `nat`:

```java
package org.luizleiteoliveira.entity;

public class User {

    private String email;
    private String gender;
    private Names name;
    private String phone;
    private String cell;
    private String nat;

    public User() {}

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    public String getGender() {
        return gender;
    }

    public void setGender(String gender) {
        this.gender = gender;
    }

    public Names getName() {
        return name;
    }

    public void setName(Names name) {
        this.name = name;
    }

    public String getPhone() {
        return phone;
    }

    public void setPhone(String phone) {
        this.phone = phone;
    }

    public String getCell() {
        return cell;
    }

    public void setCell(String cell) {
        this.cell = cell;
    }

    public String getNat() {
        return nat;
    }

    public void setNat(String nat) {
        this.nat = nat;
    }
}

```   

### Results.java
Pra finalizar a classe que faz ficam todos os nossos resultados, pra nossa modelagem só vai conter uma lista de usuários.

```java
package org.luizleiteoliveira.entity;

import java.util.List;

public class Results {

    private List<User> results;
    public Results() {}

    public List<User> getResults() {
        return results;
    }

    public void setResults(List<User> results) {
        this.results = results;
    }
}
```


### Conclusão 

Eu fiquei bem feliz com o modo que foi implementado, achei extremamente simples e intuitivo. Usar as mesmas notações para registrar o controller você utilizar pra executar a chamada facilita muito.


### _Quer acompanhar um pouco mais? Me siga nas plataformas._

GitHub: [luizleite-hotmart](https://github.com/luizleite-hotmart)

Twitter: luizleite_

Twitch: [coffee_and_code](https://www.twitch.tv/coffee_and_code)

Linkedin: [luizleiteoliveira](https://www.linkedin.com/in/luizleiteoliveira/)

dev.to: [luizleite_](https://dev.to/luizleite_)