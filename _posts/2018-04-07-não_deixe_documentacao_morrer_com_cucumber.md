---
layout: post
title:  "Não deixe que a documentação do seu software morra com Cucumber"
summary: Não deixe que a documentação do seu software morra com Cucumber
author: luizleite
date: '2020-02-13 14:35:23 +0530'
category: ['spring-boot', 'test']
thumbnail: /assets/img/posts/cocumber.png
---

Documentação sempre foi um problema em qualquer empresa que trabalhei, mas, de uns tempos pra cá, comecei a reparar se estes problemas tinham solução ou seria uma parte da vida que teria que conviver. Foi assim que me deparei com Cucumber uma ferramenta com a proposta de fazer com que sua documentação fique viva.

## Um pouco de história

Por volta dos anos 2000 algumas pessoas que estavam pensando melhores 
formas de se fazer **TDD (Test Driven Development)**, 
no início desta década Dan North nomeou de **BDD (Behavior Driven Development)**, 
hoje North é considerado o pai do BDD, e continua postando algumas coisas no [twitter](https://twitter.com/tastapod).

Com o conceito criado de um novo modo de desenvolvimento , em que, agora o desenvolvimento seria conduzido pelo comportamento que foi combinado entre clientes, analistas, e desenvolvimento ainda existia um problema. Como garantir que o "contrato" está válido. O cliente não fala a linguagem do analista, que muitas vezes não possui um conhecimento de sistema para garantir que tudo que ele disse foi aplicado.
Com isso, foi criada uma linguagem chamada Gherkin que é uma sintaxe que ajudou muito a criar o vinculo que travava os cenários escritos pelos analistas, e esses cenários vão guiar o desenvolvimento.

 - **OBS**: Note que foi dito guiar o desenvolvimento, os cenários que são escritos não vão definir qual if deve ser feito onde deve haver um loop e qualquer coisa desse tipo.

A seguir temos um exemplo do que é arquivo que comentamos acima


## Explicando o arquivo da Feature

1 - **Feature**: Nome dado para a feature que vai ser desenvolvida mais uma quantidade (Ser verboso é bom).

2 - **Scenario**: Uma explicação do que esse cenário vai fazer.

3 - **Given/AND**: Geralmente aqui são parâmetros ou dados que serão mockados para construção do cenário.

4 - **WHEN**: Momento de chamada da feature.

5 - **THEN/AND**: Validação dos resultados parte que se colocam os asserts.

Agora com o arquivo de feature padronizado pode-se fazer testes que representem estes testes.

## Entendendo o código

- Cucumber foi feito para direcionar a implementação e não refletir.

- Insira narrativas.
  
- Reuse passos sempre sempre que possível.

- Destrinchar passos.

- Cucumber features foram feitas para servir de documentação e devem ser vistas dessa maneira.

## Nem tudo são flores (os contras)

O vínculo da documentação com o código é feito via regex, o que pra alguns pode não ser tão simples devido aos parses que devem ser executados para funcionar.
Além disso eu não acho tão simples solicitar que uma pessoa que não tenha um pensamento de programação consiga escrever um documento de feature que sirva para a orientação do código.

## O cenário ideal para aplicar

Caso o lugar que você trabalhe tenha uma estrutura em que, um desenvolvedor mais experiente tenha como função direcionar e passar as atividades para quem vai executar a atividade, o líder escreve o arquivo e os outros se baseiam nele para executar as atividade pode ter um excelente resultado.

Para ver como esse projeto está segue o link do repositório. [Github](https://github.com/luizleite-hotmart/cucumber_exemplo)


## _Want to follow me?_
 
_You can get in contact me on this social media._

    
 GitHub: [luizleite-hotmart](https://github.com/luizleite-hotmart)
    
 Twitter: luizleite_
    
 Twitch: [coffee_and_code](https://www.twitch.tv/coffee_and_code)
    
 Linkedin: [luizleiteoliveira](https://www.linkedin.com/in/luizleiteoliveira/)
    
 dev.to: [luizleite_](https://dev.to/luizleite_)
 