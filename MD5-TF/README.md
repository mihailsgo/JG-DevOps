# JG MD5
### Proposed solution architecture
![image](https://user-images.githubusercontent.com/3802544/228199339-39f1b025-abb5-4327-9f5c-93b29cc36a7a.png)
### Expenses
 - EC-2 servers: **2 x c5d.xlarge instances**
 - Each c5d.xlarge instance represents such configuration:

![image](https://user-images.githubusercontent.com/3802544/228203106-0fc751b8-92c1-48c4-98e2-a1bda6f45a52.png)

|Parameter |Value |
| ------ | ------ |
| CPU | 4 x CPU |
| RAM | 8 GB |
| HDD | 1 x 100 NVMe SSD |
| Network | Up to 10 Gigabit |
| Cost per hour |0.24 $ |

 - Spike pattern:

![image](https://user-images.githubusercontent.com/3802544/228206872-2c5399f3-1c32-447c-b903-98e62db24644.png)
![image](https://user-images.githubusercontent.com/3802544/228206992-6184c85e-1d22-4347-92f2-8cb1024f4952.png)

 - Total cost
 
 **265.93 $ monthly / 3,191.14 $ yearly**
 
 ![image](https://user-images.githubusercontent.com/3802544/228207155-74175691-319a-4bed-bb30-1c6a8303c61a.png)


### AWS EC-2 servers
> - **Mihails-MD4-APP** (t2.micro, us-east-1b)

 **Docker images deployed**:
 
| Image | README |
| ------ | ------ |
| wordpress | [wordpress:latest](https://hub.docker.com/_/c14a56d6-07e4-464b-b71c-4b24dc7f1836?tab=tags) |
| nginx | [nginx:latest](https://hub.docker.com/_/nginx) |

**docker-compose**:

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
Security group: **Mihails-EC2-APP-SG**
![image](https://user-images.githubusercontent.com/3802544/228032800-fe3449a7-e682-418a-b3a2-eadd6dfa3bf4.png)
    
> - **Mihails-MD4-DB** (t2.micro, us-east-1a)

**Docker images deployed**:

| Image | README |
| ------ | ------ |
| mysql | [mysql:latest](https://hub.docker.com/_/mysql) |

**docker-compose**:

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
Security group: **Mihails-MD4-DB-SG**
![image](https://user-images.githubusercontent.com/3802544/228033350-c2a18825-137f-4d35-9fe1-9ad0b9ae8a5a.png)

 ### EC-2 server pre-requisites
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
 
 - **Mihails-MD4-APP**
 ```sh
 cd /opt/wordpress
 docker-compose up
 ```
 - **Mihails-MD4-DB**
  ```sh
 cd /opt/wpmsql
 docker-compose up
 ```
### Deployed WP screenshot
![Image](https://user-images.githubusercontent.com/3802544/227957973-3c98bbbd-4fbd-4b2d-9fb0-0d0c6397146f.png)
