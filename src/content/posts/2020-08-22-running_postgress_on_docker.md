---
title:  "Rodando postgress no docker"
summary: Rodando postgress no docker
author: luizleite
published: 2020-08-22
category: 'docker'
thumbnail: /assets/img/posts/Moby-logo.png
---

## O que é PostgreSQL

PostgreSQL  é um banco de dados bem comum e o melhor de tudo é que ele é opensource, com isso, você pode 
utiliza-lo em serviços sem preocupar com gastos adicionais por causa de licensa.
 
## O que temos precisamos
 - Docker 
  
## Comandos necessários

Para pegar a imagem utilizaremos o seguinte comando:

`docker pull postgres`

Rodando apenas execute o comando.

`docker run --name some-postgres -e POSTGRES_PASSWORD=mysecretpassword -d postgres`

## Conclusão
 Sempre que tiver oportunidade, ao invés de sair criando uma infra complexa, docker é a melhor opção para fazer um teste
rápido e/ou prova de conceito e assim chegar no resultado da maneira mais rápida possível.

## _Want to follow me?_
 
_You can get in contact me on this social media._

    
 GitHub: [luizleite-hotmart](https://github.com/luizleite-hotmart)
    
 Twitter: luizleite_
    
 Twitch: [coffee_and_code](https://www.twitch.tv/coffee_and_code)
    
 Linkedin: [luizleiteoliveira](https://www.linkedin.com/in/luizleiteoliveira/)
    
 dev.to: [luizleite_](https://dev.to/luizleite_)
 