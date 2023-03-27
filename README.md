# JG MD4
#### SOLUTION ARCHITECTURE
Diagram will follow
#### AWS EC-2 servers
> - **Mihails-MD4-APP** (t2.micro, us-east-1b)
     > -- wordpress:latest, nginx:latest container images
     > -- docker compose:
```sh
version: '3.7'
services:
    wordpress:
        container_name: wordpress
        image: wordpress:latest
        depends_on:
            - nginx
        restart: always
        environment:
            - "WORDPRESS_DB_HOST=172.31.0.115:3306"
            - WORDPRESS_DB_USER=wordpress
            - WORDPRESS_DB_PASSWORD=wordpress
            - WORDPRESS_DB_NAME=wordpress
    nginx:
        image: nginx:latest
        restart: unless-stopped
        ports:
            - "443:443"
            - "80:80"
        volumes:
            - "/opt/nginx/:/etc/nginx/conf.d"
            - "/opt/nginx/ssl/:/opt/nginx/ssl/"         
```     
    
> - **Mihails-MD4-DB** (t2.micro, us-east-1a)
    > -- mysql:latest container image
    > -- docker compose
```sh
version: '3.7'
services:
    db:
        image: mysql:latest
        volumes:
            - /opt/wpmsql/db-data:/var/lib/mysql
        restart: always
        ports:
            - "3306:3306"
        environment:
            - MYSQL_ROOT_PASSWORD=wordpress
            - MYSQL_DATABASE=wordpress
            - MYSQL_USER=wordpress
            - MYSQL_PASSWORD=wordpress

```
 #### EC-2 server pre-requisites
1. Install docker engine and docker compose plugin to both servers in order to run docker-compose
 ```sh
 #SETUP DOCKER
sudo yum update
sudo yum install -y docker
sudo usermod -a -G docker ec2-user
sudo service docker start
sudo chkconfig docker on
#SETUP DOCKER COMPOSE
sudo curl -L https://github.com/docker/compose/releases/download/v2.17.1/docker-compose-`uname -s`-`uname -m` | sudo tee /usr/local/bin/docker-compose > /dev/null
sudo chmod +x /usr/local/bin/docker-compose
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
 ```
 2. Run docker-compose up
 2.1. Mihails-MD4-APP
 ```sh
 cd /opt/wordpress
 docker-compose up
 ```
 2.2. Mihails-MD4-DB
  ```sh
 cd /opt/wpmsql
 docker-compose up
 ```
#### Deployed WP screenshot
![Image](https://user-images.githubusercontent.com/3802544/227957973-3c98bbbd-4fbd-4b2d-9fb0-0d0c6397146f.png)
