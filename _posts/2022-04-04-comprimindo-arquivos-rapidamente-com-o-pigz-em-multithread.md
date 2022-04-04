---
layout: post
date: '2022-04-04 00:45 -0300'
published: false
title: Comprimindo arquivos rapidamente com o Pigz em multithread
---
Ontem tive de efetuar um backup de uma base de dados de 800GB para poder efetuar a tarefa comecei a pesquisar se existiam soluções de compactação multithread para linux foi onde me deparei com o ´Pigz´ um software que utiliza uma implementação do `Gzip` para compactação em multithread.

Mas porque resolvi utilizar o `PigZ` e não o `Gzip` ? Pelo fato que o `Gzip` embora seja rápido e eficiente ele não tem a principal função do `PigZ` que é justamente o multithread, mas vamos ver agora como ele funciona.

## Funcionamento

Como ele compacta usando multithread ? Pelas minhas pesquisas descobri que ele divide o arquivo em pedaços de 128KB. Cada um deles e seus valores de controle são calculados em paralelo assim acelerando a velocidade de compactação. Os dados que foram compactados são gravados em ordem para a saida e o valor de verificação combinado é calculado a partir de cada valor de verificação individual. Sei que é um pouco confuso mas só explique para quem quer entender como ele faz isso. Mas agora vamos para o que realmente importa!

## Como instalar o Pigz

Para distribuições baseadas em debian basta utilizar o seguinte comando no seu terminal

```shell
sudo apt-get install pigz
```



