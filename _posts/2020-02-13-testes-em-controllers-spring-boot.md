---
layout: post
title:  "Como fazer testes unitários em controllers de um app Spring Boot"
summary: Como fazer testes unitários em controllers de um app Spring Boot
author: luizleite
date: '2020-02-13 14:35:23 +0530'
category: ['spring-boot']
thumbnail: /assets/img/posts/spring-logo.svg
---

## Mas pra que?
Antes de começar a falar sobre o como fazer esses teste, temos que primeiro entender o motivo deles serem importantes e, considerados necessários. Os teste aqui vão fazer um mock de banco, conexões, serviços, com isso, a única parte que vai ser testada será o controller.

A maioria dos desenvolvedores, eu me incluo nessa parte, compartilha que nessa camada não deve existir regra de negócio ou chamada para os dados, existem outras camadas para isso. Então a única coisa que vai ser testada será uma resposta simulada (Mockada no bom português), muitos vêem que isso não é suficiente para criar testes unitários e testes end-to-end seriam mais adequados, mas vamos para um exemplo.

Imagine que você possui o seguinte código VO:

```Java
public class Stock {
    private Date date;
    private String code;
    private BigDecimal value;
}
```

e em um belo dia alguém decide subir o código novo removendo um dos atributos, que já foram combinados em contrato com as outras aplicações e ele fica assim:

```java
public class Stock {
    private Date date;
    private BigDecimal value;
}
```

Claro que um teste de integração caso estejam bem executados pegariam antes de ir para produção, mas testes de integração custam caro e são lentos para executar. No conceito de Fail-Fast, em que quanto antes falhar menos danoso será para o seu projeto testes de contrato facilitariam muito para uma falha e correção antecipada.

## Como fazer?
Já avisei aqui que este será focado em Spring Boot caso esteja em outro framework pode ser diferente.

Antes de começar vamos criar um projeto para isso você pode apenas ir em [Start Spring](https://start.spring.io/) e criar seu projeto apenas com a dependência Web, ou seguir o tutorial que fiz no [GitHub](https://github.com/luizleite-hotmart/restControllerTest)

Se estiver tudo certo as dependências do seu Pom devem parecer com o código a seguir.

```xml
	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
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
    </dependencies>
```

Agora vamos criar um VO de stocks que tem uma data, o código da ação e, o valor que ela tem. O meu código da classe ficou da seguinte forma.

```java
import java.math.BigDecimal;
import java.util.Date;

public class Stock {
    private Date date;
    private String code;
    private BigDecimal value;

    public Stock(Date date, String code, BigDecimal value) {
        this.date = date;
        this.code = code;
        this.value = value;
    }

    public BigDecimal getValue() {
        return value;
    }

    public void setValue(BigDecimal value) {
        this.value = value;
    }

    public String getCode() {
        return code;
    }

    public void setCode(String code) {
        this.code = code;
    }

    public Date getDate() {
        return date;
    }

    public void setDate(Date date) {
        this.date = date;
    }
}
```

Agora para ser um pouco mais difícil vamos criar um service que retorna um algumas ações, o meu ficou da seguinte forma:

```java
import java.math.BigDecimal;
import java.util.Date;
import java.util.List;
import java.util.stream.Collectors;
import java.util.stream.IntStream;

@Service
public class StockServices {
    
    public List<Stock> createMockStocks(int howMany) {
        return IntStream.range(0, howMany).mapToObj(i -> new Stock(new Date(), "STK" + i, BigDecimal.valueOf(i))).collect(Collectors.toList());
    }
}
```

e agora pra finalizar, nossa camada Web que deve ter ficado algum código assim:

```java
import com.luizleiteoliveira.restControllerTest.service.StockServices;
import com.luizleiteoliveira.restControllerTest.vo.Stock;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.ResponseBody;

import java.util.List;

@Controller
public class StockController {

    private final StockServices stockServices;

    public StockController(StockServices stockServices) {
        this.stockServices = stockServices;
    }

    @GetMapping(value = "/stocks")
    public @ResponseBody
    List<Stock> getStocks(@RequestParam("howMany") int howMany) {
        return stockServices.createMockStocks(howMany);
    }
}
```

Agora se rodar nosso código e acessar [localhost com 3 de parâmetro](http://localhost:8080/stocks?howMany=3) `http://localhost:8080/stocks?howMany=3`
Vamos ter o resultado a seguir:

```json
[ 
   { 
      "date":"2020-01-27T08:34:42.925+0000",
      "code":"STK0",
      "value":0
   },
   { 
      "date":"2020-01-27T08:34:42.926+0000",
      "code":"STK1",
      "value":1
   },
   { 
      "date":"2020-01-27T08:34:42.926+0000",
      "code":"STK2",
      "value":2
   }
]
```

Agora basta testar, existem algumas formas de fazer isso, a que usuaremos vai ser com `@WebMvcTest`, esta anotação permite que você teste todos os controllers da sua aplicação. Melhor que isso você pode anotar qual controller que você quer testar, no nosso caso `StockController`, ficaria `@WebMvcTest(StockController.class)`.

Agora para você testar a chamada o Spring Framework possui uma classe chamada `MockMvc` que permita que você execute essas chamadas.

### Exemplo

#### 1) Teste de falha
Já sabemos que o nosso controller necessita um parâmetro chamado `howMany`, com isso, um teste de falha pode ser caso não seja passado esse parâmetro tem que retornar um erro.

O teste ficaria mais ou menos assim:

```java
    @Test
    public void callingWithoutParameterShouldReturnBadRequest() throws Exception {
        List<Stock> result = new ArrayList<>();
        result.add(new Stock(new Date(),"NEWSTOCK", BigDecimal.TEN));
        Mockito.when(stockServices.createMockStocks(5)).thenReturn(result);
        this.mockMvc.perform(get("/stocks")).andExpect(MockMvcResultMatchers.status().isBadRequest());
    }
```

#### 2)Teste de sucesso
Agora que já passamos por um erro o outro teste em que, passamos os parâmetros corretamente e ele retorna as stocks esperadas.

Esse teste ficaria da seguinte maneira:

```java
    @Test
    public void shouldReturnJustOneFromResult() throws Exception {
        List<Stock> result = new ArrayList<>();
        result.add(new Stock(new Date(),"NEWSTOCK", BigDecimal.TEN));
        Mockito.when(stockServices.createMockStocks(1)).thenReturn(result);
        this.mockMvc.perform(get("/stocks").queryParam("howMany", "1"))
                .andExpect(MockMvcResultMatchers.status().isOk()).andExpect(content().string(containsString("NEWSTOCK")));
    }
```

Caso falte alguma coisa a classe inteira está a seguir:

```java
package com.luizleiteoliveira.restControllerTest.controller;

import com.luizleiteoliveira.restControllerTest.service.StockServices;
import com.luizleiteoliveira.restControllerTest.vo.Stock;
import org.hamcrest.Matchers;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.mockito.Mockito;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.WebMvcTest;
import org.springframework.boot.test.mock.mockito.MockBean;
import org.springframework.test.context.junit4.SpringRunner;
import org.springframework.test.web.servlet.MockMvc;
import org.springframework.test.web.servlet.result.MockMvcResultMatchers;

import java.math.BigDecimal;
import java.util.ArrayList;
import java.util.Date;
import java.util.List;

import static org.hamcrest.Matchers.containsString;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.content;

@RunWith(SpringRunner.class)
@WebMvcTest(StockController.class)
public class StockControllerTest {

    @Autowired
    private MockMvc mockMvc;

    @MockBean
    private StockServices stockServices;

    @Test
    public void callingWithoutParameterShouldReturnBadRequest() throws Exception {
        List<Stock> result = new ArrayList<>();
        result.add(new Stock(new Date(),"NEWSTOCK", BigDecimal.TEN));
        Mockito.when(stockServices.createMockStocks(5)).thenReturn(result);
        this.mockMvc.perform(get("/stocks")).andExpect(MockMvcResultMatchers.status().isBadRequest());
    }

    @Test
    public void shouldReturnJustOneFromResult() throws Exception {
        List<Stock> result = new ArrayList<>();
        result.add(new Stock(new Date(),"NEWSTOCK", BigDecimal.TEN));
        Mockito.when(stockServices.createMockStocks(1)).thenReturn(result);
        this.mockMvc.perform(get("/stocks").queryParam("howMany", "1"))
                .andExpect(MockMvcResultMatchers.status().isOk())
                .andExpect(content().string(containsString("NEWSTOCK")));
    }
}
```

## Conclusão
Para uma classe simples foi bem tranquilo utilizar os conceitos de testar uma parte separada. Foi muito satisfatório utilizar o mockMvc pois para o primeiro teste eu tinha me esquecido de passar o parâmetro `howMany` e o teste quebrou mostrando o que poderia acontecer. Agora pretendo fazer alguns testes mais robustos com filters no controller para ver o quão simples vai ser além de tentar aplicar um TDD nessa parte para ver se faz sentido.

De qualquer forma penso que pelo custo que foi de implementar é muito válido utilizar testes assim, são leves e conseguem controlar a saída de dados como dados, tipo, httpStatus... Então é mais uma forma leve de cercar sua aplicação, na minha opinião utilize o maior ferramental de testes unitários que os desenvolvedores vão ficar mais felizes.

Até a próxima!! =)

## _Want to follow me?_
 
_You can get in contact me on this social media._

    
 GitHub: [luizleite-hotmart](https://github.com/luizleite-hotmart)
    
 Twitter: luizleite_
    
 Twitch: [coffee_and_code](https://www.twitch.tv/coffee_and_code)
    
 Linkedin: [luizleiteoliveira](https://www.linkedin.com/in/luizleiteoliveira/)
    
 dev.to: [luizleite_](https://dev.to/luizleite_)
 