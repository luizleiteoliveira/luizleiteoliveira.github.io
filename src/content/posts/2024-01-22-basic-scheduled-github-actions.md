---
title: "Como criar uma action no github como scheduled"
summary: Nesse post vamos mostrar como é fácil criar uma action no github que roda 
author: luizleite
published: 2024-01-21
category: 'github'
thumbnail: 
---

## Intro

Neste post, abordaremos a criação de uma GitHub Action para a execução programada de tarefas automatizadas. Isso pode 
ser uma maneira eficiente de otimizar processos recorrentes, economizando tempo e assegurando a consistência nas operações.

### Criação do Arquivo de Workflow

Primeiramente, é necessário criar um arquivo YAML na pasta .github/workflows do repositório. Este arquivo conterá as 
configurações essenciais para a GitHub Action. Abaixo está um exemplo prático:

```yaml

name: Tarefa Agendada

on:
  schedule:
    - cron: '0 0 * * *' # Define a execução diária às 00:00 UTC

jobs:
  execute-tarefa:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout do código
        uses: actions/checkout@v2

      - name: Executar Tarefa Agendada
        run: |
          # Comandos para executar a tarefa aqui

```

### Configuração do Agendamento

No exemplo acima, a seção `on` define o agendamento por meio da sintaxe cron. Neste caso, a ação está programada para ser 
executada diariamente à meia-noite (UTC). É possível personalizar o cron conforme as necessidades de agendamento específicas.

### Definição da Tarefa a ser Executada

Dentro do bloco `run` na seção execute-tarefa, é necessário adicionar os comandos correspondentes para realizar a tarefa
desejada. Isso pode incluir `scripts`, chamadas de API ou qualquer outra ação a ser automatizada.


### O que fazer com isso 

Em outro post vamos mostrar melhor como fazer isso mas esse tipo de action é extremamente útil para fazer validações de
tempos em tempos, como, ver se alguma lib sofreu atualização ou validar se existe uma receita de openRewrite e aplicar 
para atualizar o seu projeto.

## _Quer me acompanhar?_

_Aqui estão algumas das minhas redes sociais._

GitHub: [luizleite-hotmart](https://github.com/luizleite-hotmart)

Twitter: luizleite_

Twitch: [coffee_and_code](https://www.twitch.tv/coffee_and_code)

Linkedin: [luizleiteoliveira](https://www.linkedin.com/in/luizleiteoliveira/)
 
