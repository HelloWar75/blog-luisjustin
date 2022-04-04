---
layout: post
title: How to configure your nginx to run a Laravel application
date: 2020-12-09
categories: ["php", "laravel", "nginx"]
---

![Stack nginx Php Laravel](https://luisjustin.com.br/assets/images/nignx-laravel-php.png)

These days I came across the need to configure Nginx to run Laravel, so I started searching hard on the internet for the best way to do it, so now that I know how to do it and I already do it at the company I work for I decided to pass this knowledge on.

**Remembering that I am no expert on the subject, so there may be something that I have forgotten to talk about so I ask that you let me know if something is missing that I correct the tutorial.**

First we have to evaluate the question of which operating system you are using, because each operating system has a way to configure. When I say that has a way for each does not mean that the nginx changes its operation in each OS, but changes the organization of folders.

Here I will use Ubuntu 20.04, but remembering that the same steps can be reproduced in any operating system, just analyze how the folder organization of your OS is.

Remembering that this does not apply only to Laravel, you can use this configuration to run any PHP system, but this tutorial will focus on Laravel 8.0.

## 1. Installing Nginx
In ubuntu 20.04 installing nginx is a relatively simple task using the package manager just type the following command:

```shell
sudo apt install nginx
```

After you have completed the installation you need to start your nginx server with the following command:

```shell
sudo systemctl start nginx
```
## 2. Installing PHP-FPM
To install PHP-FPM in its latest version you only need to type the following command:

```shell
sudo apt install php-fpm
```

After that you will notice that php-fpm is already installed and running.

## 3. Installing Dependencies that Laravel needs to work
To install the necessary dependencies just copy and paste the following command into your terminal:

```shell
sudo apt install php-mysql php-pdo php-mbstring php-xml php-bcmath php-zip
```

## 4. Configuring Nginx to host Laravel
First you need to open the nginx configuration file, the famous “default” file

```shell
nano /etc/nginx/sites-available/default
```
I will teach you 2 ways to create this configuration, because two ways? For the fact that you can use it both when it is to access by IP server as when you are setting up multiple domains with several applications same or different in laravel.After that you will see the following screen:

![Terminal config default site nginx](https://luisjustin.com.br/assets/images/2020-12-09-terminal-configuring-nginx-default-site.png)

now we will modify the script to look like this:

```
server {
        listen 80;
        root /var/www/html/public;
        index index.php index.html index.htm index.nginx-debian.html;
        server_name seusite.com.br;
        charset utf-8;
        location / {
                try_files $uri $uri/ /index.php?$query_string;
        }
        location = /favicon.ico { access_log off; log_not_found off; }
        location = /robots.txt  { access_log off; log_not_found off; }
        error_page 404 /index.php;
        location ~ \.php$ {
                include snippets/fastcgi-php.conf;
                fastcgi_pass unix:/var/run/php/php7.2-fpm.sock;
                fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        }
        location ~ /\.ht {
                deny all;
        }
        location ~ /\.(?!well-known).* {
                deny all;
        }
}
```

Don’t forget to replace in server_name with the domain that will be used for access or you can remove this directive.

After that you just need to press “CTRL + S” to save and “CTRL + X” if it asks for some confirmation when closing just click “Y”.

After that we will type

```
sudo ln -s /etc/nginx/sites-available/default /etc/nginx/sites-enabled/
```

This will create a symbolic link by putting the “default” file into nginx sites-enabled as well.

Now let’s type:

```
nginx -t
```

It should show something similar to this:
![Terminal Result](https://luisjustin.com.br/assets/images/2020-12-09-terminal-result.png)

After that we will type
```shell
sudo systemctl reload nginx
```
To have nginx reload the configuration files without rebooting.
## 5. Configure your laravel
Now you need to configure “.env” of your laravel, this is an individual configuration so I will skip this step but I will remind you that you need that the Application URL in laravel only needs to be your domain example:
```
my_website.com
```

## 6. All set, let’s test it!
Now to test it is simple you can open the link to your website in your browser and you will see that laravel is running perfectly.
