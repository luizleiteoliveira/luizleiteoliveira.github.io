---
layout: post
title: "Como GraalVM e compilação AOT geram imagens menores"
summary: Como GraalVM e compilação AOT geram imagens menores
author: luizleite
date: '2023-07-13 7:00:23 +0500'
category: [ 'graalvm' ]
thumbnail: /assets/img/quarkus_logo.svg
---

## Intro

GraalVM é uma máquina virtual Java que pode gerar imagens de aplicação muito menores do que as
imagens de aplicação geradas por uma JVM tradicional. Isso ocorre porque GraalVM usa uma técnica
chamada compilação antecipada (AOT) para gerar código nativo para um sistema operacional específico.

### AOT vs JIT

Uma das principais razões para a diferença de tamanho entre as imagens geradas por GraalVM e uma JVM
tradicional é a otimização do tempo de compilação. Enquanto uma JVM tradicional utiliza a compilação
Just-in-Time (JIT), que compila o código Java para bytecode durante a execução, a GraalVM usa a
técnica de compilação Ahead-of-Time (AOT). Com a compilação AOT, o código Java é convertido
antecipadamente em um formato executável nativo, eliminando a necessidade de interpretar o bytecode
durante a execução. Embora a compilação AOT possa exigir um tempo de compilação inicialmente mais
longo, ela resulta em um código nativo mais eficiente e em imagens menores.

### Remoção de dependências desnecessárias

Outro fator que contribui para a diferença de tamanho é a capacidade do GraalVM de analisar as
dependências do aplicativo e remover aquelas que não são necessárias para a execução. Enquanto uma
JVM tradicional inclui um conjunto completo de bibliotecas e dependências para garantir a
compatibilidade com uma ampla gama de aplicativos, a GraalVM identifica as dependências específicas
do aplicativo e as inclui na imagem final, eliminando qualquer excesso desnecessário. Isso resulta
em imagens mais enxutas e um menor consumo de recursos, mesmo considerando o tempo adicional gasto
na análise e remoção das dependências.

### Benefícios a longo prazo

Embora a compilação AOT da GraalVM possa exigir um tempo de compilação inicialmente mais longo do
que a compilação JIT de uma JVM tradicional, há benefícios a longo prazo. Uma vez que o código é
compilado antecipadamente, o tempo de inicialização da aplicação é reduzido significativamente,
proporcionando uma experiência mais rápida para o usuário final. Além disso, a compilação AOT também
oferece melhor desempenho durante a execução, uma vez que o código nativo é altamente otimizado.
Esses benefícios compensam o tempo adicional gasto na compilação e resultam em uma experiência geral
mais eficiente. Isso ocorre porque o código nativo pode ser executado diretamente pelo sistema
operacional, sem a necessidade de interpretação.

### Como usar

Para gerar uma imagem nativa com GraalVM, você pode usar o seguinte comando Maven:

`mvn clean install -Dgraalvm.native-image.imageName=my-app`

Este comando irá gerar uma imagem nativa chamada `my-app` no diretório `target`.

Para executar a imagem nativa, você pode usar o seguinte comando:

`./my-app`

A imagem nativa será executada e você verá a saída da aplicação.

### Conclusão

A imagem gerada pela GraalVM é menor do que a gerada por uma JVM tradicional graças à otimização
AOT, à remoção de dependências desnecessárias e aos benefícios a longo prazo. Embora a compilação
AOT possa exigir um tempo de compilação inicialmente mais longo, os ganhos em termos de tamanho da
imagem, consumo de recursos, tempo de inicialização e desempenho fazem dela uma escolha vantajosa
para o desenvolvimento e a implantação de aplicativos. À medida que a GraalVM continua a evoluir,
podemos esperar mais avanços nessa área, proporcionando uma experiência de desenvolvimento mais
eficiente, rápida e otimizada.

## _Quer me acompanhar?_

_Aqui estão algumas das minhas redes sociais._

GitHub: [luizleite-hotmart](https://github.com/luizleite-hotmart)

Twitter: luizleite_

Twitch: [coffee_and_code](https://www.twitch.tv/coffee_and_code)

Linkedin: [luizleiteoliveira](https://www.linkedin.com/in/luizleiteoliveira/)
 
