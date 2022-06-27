---
layout: post
title: "O que é criptografia"
summary: Neste post vamos abordar o que é criptografia assim como os conceitos básicos dessa técnica 
author: luizleite
date: '2022-06-07 7:00:23 +0500'
category: ['segurança', 'criptografia']
thumbnail: /assets/img/posts/maven.png
---

## Intro

Criptografia é uma técnica de segurança para proteção da informação que permite que apenas aqueles que tem acesso a uma chave podem ler.

A criptografia é uma fórmula matematica que transforma um _PlainText_ (Mensagem que pode ser lida por qualquer um) em um _CypherText_ com auxílio de uma chave.
No caso da criptografia é possível realizar o processo reverso de _CypherText_ para _PlainText_ com uma chave chamado decriptografia. Baseado na primeira e segunda chave a 
técnica de criptografia pode variar, existem dois tipos a __*Criptografia Simétrica*__ e a __*Criptografia Assimétrica*__.  


## Criptografia Simétrica

Nessa técnica é utilizada a mesma chave para criptografia como no processo inverso. O grande problema desse tipo é, caso 
a chave seja descoberta, qualquer um pode ter acesso à mensagem, outro problema é que a chave tem que ser 
repassada entre todos. Seguem alguns algoritmos que implementam criptografia Simétrica:

 - Advanced Encryption Standard (AES)
 - Data Encryption Standard (DES)

## Criptografia Assimétrica/ Criptografia de chave pública

Para eliminar o problema de ter apenas uma chave que controla todo processo de criptografar e decriptografar a informação, foi 
criada essa criptografia em que uma chave pública criptografa a mensagem e outra privada que apenas o recetor tem acesso consegue aplicar
a decriptografia. Exemplos de criptografia assimétrica são:

 - RSA
 - Elliptic Curve Digital Signature Algorithm (Usada pela blockchain)
 - Digital Signature Algorithm (DSA)

## Um pouco de história

Uma das primeiras vezes observada o uso de criptografia foi com a Criptografia de César, imperador romano, que em 100 A.C
enviava mensagens com teor militar com as letras trocadas por a de três posições para direita, ou seja, nesse caso a operação matemática seria 
somar o valor da letra mais 3 para achar a letra correta, sendo assim, A era trocada por um D, B por um E ....

Vale lembrar que essa criptografia pode ser facilmente quebrada, por isso não representa muita segurança nos dias atuais. 

## _Quer me acompanhar?_
 
_Aqui estão algumas das minhas redes sociais._

    
 GitHub: [luizleite-hotmart](https://github.com/luizleite-hotmart)
    
 Twitter: luizleite_
    
 Twitch: [coffee_and_code](https://www.twitch.tv/coffee_and_code)
    
 Linkedin: [luizleiteoliveira](https://www.linkedin.com/in/luizleiteoliveira/)
 
