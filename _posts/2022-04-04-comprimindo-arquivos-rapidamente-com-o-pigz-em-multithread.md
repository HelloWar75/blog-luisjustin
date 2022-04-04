---
layout: post
date: '2022-04-04 00:45 -0300'
published: false
title: Comprimindo arquivos rapidamente com o Pigz em multithread
---
Ontem tive de efetuar um backup de uma base de dados de 800GB para poder efetuar a tarefa comecei a pesquisar se existiam soluções de compactação multithread para linux foi onde me deparei com o ´Pigz´ um software que utiliza uma implementação do `Gzip` para compactação em multithread.

Mas porque resolvi utilizar o `PigZ` e não o `Gzip` ? Pelo fato que o `Gzip` embora seja rápido e eficiente ele não tem a principal função do `PigZ` que é justamente o multithread, mas vamos ver agora como ele funciona e como instalar ele em seu Linux.

## Teste



[Teste Link](#Teste)
