---
layout: post
title: "Entendendo os escopos ao importar de dependências do Maven"
summary: Nest post será mostrado quais são e como utilizar corretamente a parte de _SCOPE_ na hora de importar as dependencias do Maven. 
author: luizleite
date: '2022-04-28 19:00:23 +0530'
category: ['maven', 'java']
thumbnail: /assets/img/posts/maven.png
---

# O Resumo

Maven é uma das ferramentas mais conhecidas e populares para build de aplicação _Java_, além dessa função, também é utilizado como
gestor de dependências. Dentro dessa parte existe uma _tag_ chamada _scope_ em que é possível definir qual o escopo esta será utilizada.
Existem 6 valores possíveis para essa parte **compile, runtime, provided, system, test, and import**. Caso não seja declarado nenhum tipo 
o valor default é _compile_. Vamos agora comentar o que significa cada tipo utiilizando com.

É importante lembrar que existem dois tipos de dependências no Maven:

 - Diretas: São as que foram incluídas diretamente no código e podem ser vistas explicitamente no _pom.xml_ do projeto.
 - Transitivas: Importadas através das diretas declaradas no projeto.

Para verificar todas as dependencias transitivas ou diretas rode o comando `mvn dependency:tree` isso vai gerar um código do tipo

```shell
[INFO] +- org.jetbrains.kotlin:kotlin-stdlib-jdk8:jar:1.4.21:compile
[INFO] |  \- org.jetbrains.kotlin:kotlin-stdlib-jdk7:jar:1.4.21:compile
```

Nesse caso `kotlin-stdlib-jdk8` veio de forma direta e `kotlin-stdlib-jdk7` como transitiva ambas como _compile_. Dependendo 
do escopo declarado as dependências transitivas serão afetadas assim como o _classpath_ da aplicação.


## compile

## runtime

## provided

## system

## test

## import




## _Quer me acompanhar?_
 
_Aqui estão algumas das minhas redes sociais._

    
 GitHub: [luizleite-hotmart](https://github.com/luizleite-hotmart)
    
 Twitter: luizleite_
    
 Twitch: [coffee_and_code](https://www.twitch.tv/coffee_and_code)
    
 Linkedin: [luizleiteoliveira](https://www.linkedin.com/in/luizleiteoliveira/)
 
