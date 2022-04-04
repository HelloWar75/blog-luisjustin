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
```shell
pigz nome_do_arquivo.txt
```

Para algumas pessoas o jeito que ele ira executar este comando pode ser um problema pois ele executa e costuma **deletar o arquivo por padrão** para evitarmos isso basta colocar junto o argumento **-k** ficando assim o comando

```shell
pigz -k nome_do_arquivo.txt
```

Já sobre seus niveis de compressão eu descobri que ele suporta vários variando do **1 ao 9** para utilizar basta colocar como argumento **-1, -2, -3 ... -9** o comando ira ficar assim
```shell
pigz -9 -k nome_do_arquivo.txt
```
Lembrando que neste comando mantive para ele não deletar o arquivo ou seja o parametro **-k**

Agora para compactar pastas como fazemos ? Infelizmente o pigz não suporta compactação de pastas mas existe um jeito sim de utilizar ele para isso na verdade 2 jeitos o primeiro e mais simplês é

### Método 1

```shell
tar --use-compress-program="pigz -k -9" -cf arquivo_compactado.tar.gz pasta_para_compactar/
```

Vou colocar uma breve legenda aqui para facilitar o entedimento de todo esse comando:
`--use-compress-program` estamos dizendo ao **tar** que mesmo que ele crie um arquivo tar a compressão do seu conteudo deve usar um software externo ou seja o **pigz**
`-cf` informamos que queremos criar um arquivo no caso do exemplo que se chama **arquivo_compactado.tar.gz** e o que vem logo após é o nome da pasta que queremos compactar.

E agora para descompactar esse arquivo ? Então no caso do arquivo não ter pasta somente arquivos mesmos pode se usar os comandos abaixo
```shell
pigz -d arquivo_compactado.tar.gz
```

ou

```shell
unpigz arquivo_compactado.tar.gz
```

Lembrando queo **Pigz** pode descompactar qualquer arquivo **.gz** tendo ou não pastas dentro dele

Mas agora continuando sobre nosso exemplo que tem pastas dentro como se deve fazer
```shell
tar --use-compress-program="pigz -d" -xvf arquivo_compactado.tar.gz
```

### Método 2

Temos também esse segundo metodo que foi o que usei pois foi o que aprendi primeiro na hora do desespero para compactar meu arquivo gigante

Para comprimir um arquivo com pastas ou não basta usar

```shell
tar -cf - pasta_onde_esta_os_arquivos/ | pigz > arquivo_compactado.tar.gz
```

e para descompactar esse arquivo mais facil ainda

```shell
pigz -dc arquivo_compactado.tar.gz | tar -xf -
```

ele vai criar a pastas no local onde está seu arquivo


