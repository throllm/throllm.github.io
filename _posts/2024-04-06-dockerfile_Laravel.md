---
layout: post
title:  "Dokerfile for Laravel installation (use for Synology DSM)"
---

I like to use Synology NAS/DSM as a
test and development environment because it gives me an easy access to docker container management and virtualization features.

The following Dockerfile compose the following environment:
- Ubuntu
- Apache Webserver with PHP
- Laravel basic installation (with sqlite Database)
- install all dependent libraries/software (composer, npm)
- set the permissions
- install Laravel Jetstream package ("application starter kit")


```
FROM ubuntu:latest

ENV TZ="Europe/London"

RUN apt-get update

# Apache install
RUN apt-get install -y apache2 && apt-get clean
RUN a2enmod rewrite

# Apache config
COPY laravel.conf /etc/apache2/sites-available
RUN chown 644 /etc/apache2/sites-available/laravel.conf
RUN a2ensite laravel.conf
RUN a2dissite 000-default.conf

# PHP and further extensions/software
RUN apt-get install -y software-properties-common
RUN add-apt-repository ppa:ondrej/php -y 
RUN apt-get update
RUN apt-get upgrade -y

RUN DEBIAN_FRONTEND=noninteractive apt-get install php8.3 -y 
RUN apt-get install joe curl wget php-cli php-zip unzip php-curl php-xml php-sqlite3 sqlite3 npm -y
RUN npm install -g n
RUN n lts
RUN n prune

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Laravel basic instalation
WORKDIR "/var/www/html"
RUN composer create-project --prefer-dist laravel/laravel laravel 
RUN chown -R www-data:www-data /var/www/html/laravel/storage
RUN chmod -R 755 /var/www/html/laravel/bootstrap/cache

RUN chown www-data:www-data /var/www/html/laravel/database/
RUN chown www-data:www-data /var/www/html/laravel/database/database.sqlite

# Laravel packages and functions
WORKDIR "/var/www/html/laravel"

# Laravel Jetstream
RUN composer require laravel/jetstream
RUN php artisan jetstream:install livewire
RUN npm install
RUN npm run build
RUN php artisan migrate

#RUN service apache2 start

EXPOSE 80/tcp
```


laravel.conf for vhost
```
<VirtualHost *:80>                                                                                                    
        DocumentRoot /var/www/html/laravel/public                                
        ErrorLog ${APACHE_LOG_DIR}/error.log                                     
        CustomLog ${APACHE_LOG_DIR}/access.log combined                          
        <Directory /var/www/html/laravel/public>                               
           Options Indexes FollowSymLinks                                           
           AllowOverride All                                                        
           Require all granted                                                      
        </Directory>                                                                     
</VirtualHost>                                                                   
 ```                    

Create der Dockerimage via SSH on Syology NAS. The image will apear on the image list
``` 
  sudo docker build -t myimagename .
```  
Synology Container Manager
![Synology Container Manager](/assets/images/synology_docker.png)
