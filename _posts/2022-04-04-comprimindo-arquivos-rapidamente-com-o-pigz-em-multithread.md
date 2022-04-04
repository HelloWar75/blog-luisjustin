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

## Como utilizar

Para comprimir qualquer arquivo basta utilizar o seguinte comando em seu terminar
```
pigz nome_do_arquivo.txt
```

Para algumas pessoas o jeito que ele ira executar este comando pode ser um problema pois ele executa e costuma **deletar o arquivo por padrão** para evitarmos isso basta colocar junto o argumento **-k** ficando assim o comando

```
pigz -k nome_do_arquivo.txt
```

Já sobre seus niveis de compressão eu descobri que ele suporta vários variando do **1 ao 9** para utilizar basta colocar como argumento **-1, -2, -3 ... -9** o comando ira ficar assim
```
pigz -9 -k nome_do_arquivo.txt
```
Lembrando que neste comando mantive para ele não deletar o arquivo ou seja o parametro `-k`