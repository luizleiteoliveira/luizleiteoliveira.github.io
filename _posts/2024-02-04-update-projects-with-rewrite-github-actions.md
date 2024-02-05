---
layout: post
title: "Atualizando Projetos Automaticamente com OpenRewrite no GitHub"
summary: Neste artigo, você aprenderá como manter seus projetos atualizados com base em uma receita predefinida usando OpenRewrite 
author: luizleite
date: '2024-02-04 7:00:23 +0500'
category: [ 'github', 'github-actions', 'rewrite', 'open-rewrite' ]
thumbnail: 
---

## Intro

Antes de mergulharmos nisso, é crucial entender o que essa ferramenta faz. Para simplificar, vamos chamá-la de 
"executadora de receitas predefinidas". Ela executa essas receitas por meio de um comando mvn e uma lista de receitas
prédefinidas. Uma das grandes vantagens do OpenRewrite é a participação ativa da comunidade e de grandes empresas, como 
a Spring, que estão desenvolvendo e compartilhando suas próprias receitas. Isso significa que você pode confiar nas 
receitas recomendadas por especialistas para evoluir seus projetos com segurança e confiabilidade.

### Criação do Workflow

É necessário criar um arquivo `rewrite.yml` que vai conter todas as alterações que você considera importantes para ser 
a stack padrão dos seus projetos. Segue um exemplo:

```yaml
---
type: specs.openrewrite.org/v1beta/recipe
name: com.luizleiteoliveira.BestModuleRecipe
displayName: Upgrade Maven dependency version example
recipeList:
  - org.openrewrite.java.spring.boot3.UpgradeSpringBoot_3_2
  - org.openrewrite.maven.UpgradeDependencyVersion:
      groupId: com.fasterxml.jackson*
      artifactId: jackson-module*
      newVersion: 29.X
      versionPattern: '-jre'
      retainVersions: com.jcraft:jsch

```
[Neste link](https://docs.openrewrite.org/recipes/) existem diversas receitas para serem aplicadas você só precisa coloca-las
na lista acima.

Agora é necessário criar um arquivo YAML na pasta .github/workflows do repositório. Este arquivo conterá as 
configurações essenciais para a GitHub Action nesse arquivo será prédefinido como executar a receita:

```yaml

name: Runs open rewrite

on:
  workflow_call:
    secrets:
      SOME_SECRET:
        required: false

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-java@v4
        with:
          distribution: 'temurin' # See 'Supported distributions' for available options
          java-version: '21'
          cache: maven
      - name: download recipe file
        shell: bash
        run: |
          wget https://raw.githubusercontent.com/luizleiteoliveira/tutorials/main/.github/workflows/rewrite.yml
      - name: Build with Maven
        id: mvn-dryRun
        run: mvn org.openrewrite.maven:rewrite-maven-plugin:5.21.0:dryRun -DactiveRecipes=com.luizleiteoliveira.BestModuleRecipe -Drewrite.recipeArtifactCoordinates=org.openrewrite.recipe:rewrite-spring:RELEASE > mvnResult.txt 2>&1
      - name: Validar se tem atualizaçoes a serem feitas
        run: |
          if grep -q "\[WARNING\] These recipes would make changes to" mvnResult.txt; then
            echo "::warning ::Changes to be made"
            exit 1
          else
            echo "Project is on right track and fully updated"
          fi

```


Esse arquivo pode ser encontrado [aqui](https://github.com/luizleiteoliveira/tutorials/blob/main/.github/workflows/openrewrite_update_latest.yml) 
E os passos dele basicamente fazem:

- Checkout do projeto
- Configuram um ambiente java para execução do mvn
- Fazen download do arquivo rewrite.yml
- Roda `mvn org.openrewrite.maven:rewrite-maven-plugin:5.21.0:dryRun -DactiveRecipes=com.luizleiteoliveira.BestModuleRecipe -Drewrite.recipeArtifactCoordinates=org.openrewrite.recipe:rewrite-spring:RELEASE` e valida se existe alguma alteração a ser feita. A validação pode ser feita pegando o arquivo da receita e adicionando na raiz do projeto.

E para finalizar basta criar um outro workflow este como referência, exemplo:

```yaml
name: rewrite validation
on:
  push:
    branches: [ "main" , "master"]
  pull_request:
    branches: [ "main" , "master"]

jobs:
  check-updates:
    uses: luizleiteoliveira/tutorials/.github/workflows/openrewrite_update_latest.yml@main
```



### O que fazer com isso 

Com esse workflow no seu projeto sempre que for criado um pull request ele vai validar se está em uma configuração pré-definida,
com isso, todos os projetos vão passar por uma receita e vai ter uma quebra de pipeline caso não estejam na ultima configuração. 

## _Quer me acompanhar?_

_Aqui estão algumas das minhas redes sociais._

GitHub: [luizleite-hotmart](https://github.com/luizleite-hotmart)

Twitter: luizleite_

Twitch: [coffee_and_code](https://www.twitch.tv/coffee_and_code)

Linkedin: [luizleiteoliveira](https://www.linkedin.com/in/luizleiteoliveira/)
 
