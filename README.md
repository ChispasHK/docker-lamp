# Docker LAMP
Docker template with LAMP stack (Apache, MySql, Php and PhpMyAdmin)

## Docker install
### The procedure to install docker in ubuntu is with the following commands:

Update your existing list of packages:
```
sudo apt update
```
Install a few prerequisite packages which let apt use packages over HTTPS:
```
sudo apt install apt-transport-https ca-certificates curl software-properties-common
```
Then add the GPG key for the official Docker repository to your system:
```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```
Add the Docker repository to APT sources:
```
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"
```
Update the package database with the Docker packages from the newly added repo:
```
sudo apt update
```
Install Docker:
```
sudo apt install docker-ce
```
Check that it’s running:
```
sudo systemctl status docker
```
Install Docker compose:
```
sudo apt  install docker-compose
``` 

## Contents of Docker-compose.yml and Dockerfile
### The file ``docker-compose.yml``
```
version: "3.1"
services:
#apache
    www:
        container_name: www
        build: .
        ports: 
            - "80:80"
        volumes:
            - ./www:/var/www/html/
        links:
            - db
        networks:
            - default
#mariadb
    db:
        image: mariadb:latest
        ports: 
            - "3306:3306"
        command: --default-authentication-plugin=mysql_native_password
        environment:
            MYSQL_DATABASE: myDb
            MYSQL_USER: user
            MYSQL_PASSWORD: password
            MYSQL_ROOT_PASSWORD: password 
        volumes:
            - ./db_data:/var/lib/mysql
        networks:
            - default
#phpmyadmin
    phpmyadmin:
        container_name: phpmyadmin
        image: phpmyadmin/phpmyadmin
        links: 
            - db:db
        ports:
            - 81:80
        environment:
            MYSQL_USER: user
            MYSQL_PASSWORD: password
            MYSQL_ROOT_PASSWORD: password
volumes:
    persistent:
```
### The file ``Dockerfile``
```
FROM php:7.3-apache 
RUN docker-php-ext-install mysqli
``` 

## Run and stop the docker-compose

Turn on the docker-compose
```
docker-compose up
```
Turn off the docker-compose
```
docker-compose down
```


## Password
### MariaDB
```
      MYSQL_USER: user
      MYSQL_PASSWORD: password
      MYSQL_ROOT_PASSWORD: password 
```

## So we can watch the web in local:
### Apache
```
http://localhost:80/
```
### PhPMyadmin
```
http://localhost:81/
```

## Directory structure
```
Docker-PHP/
├── docker-compose.yml
├── Dockerfile
├── db_data
│   └──
└── www
    └──
```

