---
layout: post
date: '2022-04-04 00:45 -0300'
published: true
title: Compressing files fast with Pigz on multithread
categories:
  - Linux
---
Yesterday I had to make a backup of an 800GB database in order to perform the task I started to research if there were multithreaded compression solutions for Linux that's where I came across ´Pigz´, a software that uses an implementation of `Gzip` to multithreaded compression.

But, why did I decide to use `PigZ` and not `Gzip`? Because `Gzip`, although fast and efficient, does not have the main function of `PigZ`, which is precisely multithreading, but now let's see how it works.

## How does PigZ work?

How does it compress using multithread ? From my research I found that it splits the file into 128KB chunks. Each of them and their control values are calculated in parallel thus accelerating the compression speed. The data that has been compressed is written in order for output and the combined check value is calculated from each individual check value. I know it's a little confusing, but just explain to anyone who wants to understand how he does it. But now let's get to what really matters!

## How to install Pigz

For Debian based distributions just use the following command in your terminal

```shell
sudo apt-get install pigz
```

## How to use

To compress any file just use the following command in your finish

```shell
pigz file.txt
```

For some people, the way it will execute this command can be a problem, because it executes and usually **delete the file by default** to avoid this, just put the argument **-k** together, thus the command

```shell
pigz -k file.txt
```

About its compression levels I found out that it supports several ranging from **1 to 9** to use just put as an argument **-1, -2, -3 ... -9** the command will look like this
```shell
pigz -9 -k file.txt
```
Remembering that in this command I kept it so it doesn't delete the file, that is, the parameter **-k**

Now to compress folders as we do? Unfortunately pigz does not support folder compression, but there is a way to use it, in fact, 2 ways let's check them out

### Method 1

```shell
tar --use-compress-program="pigz -k -9" -cf file.tar.gz folder/
```

I'll put a brief caption here to make this whole command easier to understand:
`--use-compress-program` We are telling **tar** that even if it creates a tar file the compression of its content must use external software, i.e. **pigz**
`-cf` We inform that we want to create a file in the case of the example that is called **file.tar.gz** and what comes right after is the name of the folder that we want to compress.

And now to unzip this file? So in case the file does not have a folder, only the same files can be used the commands below
```shell
pigz -d file.tar.gz
```

or

```shell
unpigz file.tar.gz
```

Remembering that **Pigz** can unzip any **.gz** file

But now continuing on our example that has folders inside how to do
```shell
tar --use-compress-program="pigz -d" -xvf file.tar.gz
```

### Method 2

We also have this second method that I used, because it was what I learned first when I was desperate to compress my giant file

To compress a file with folders or not just use

```shell
tar -cf - pasta_onde_esta_os_arquivos/ | pigz > arquivo_compactado.tar.gz
```

and to unzip this file even easier

```shell
pigz -dc arquivo_compactado.tar.gz | tar -xf -
```

It will create folders where your file is.

## Limiting the number of threads

The **Pigz** usually eats a lot of your processor cores, because it has no limit to make a faster compression it uses all your processor, but we have ways to limit the use we will see below an example that can be applied to everyone previous

To make the processor cores limitation we can use the **-p** parameter followed by the number of cores you want to use your processor. But remember don't put a number that your hardware is not ready to support, follow the example

```shell
pigz -k -p2 file.txt
```

same thing applies to all previous examples.

Thanks for reading the article 😁

