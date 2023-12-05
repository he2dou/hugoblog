---
title: docker-compose 定义和运行容器
date: 2022-04-04T05:00:00Z
categories:
  - 技术
tags:
  - docker
---

<img src="https://pic.imgdb.cn/item/656f2c2ec458853aef77ecd0.jpg" title="source: imgur.com" />

Docker Compose 是 docker 提供的一个命令行工具，用来定义和运行由多个容器组成的应用。

<!--more-->

{{< toc >}}

## 安装

确保本机已经安装了docker，参考官方链接：[https://docs.docker.com/compose/install/](https://docs.docker.com/compose/install/)

运行此命令 **下载docker compose** 的当前稳定版本：

```sh
sudo curl -L https://download.fastgit.org/docker/compose/releases/download/1.27.4/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
```

**对二进制文件应用可执行权限**


```sh
sudo chmod +x /usr/local/bin/docker-compose
```

**创建软链接**


```sh
ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
```

**查看版本测试是否安装成功**


```sh
docker-compose --version
```

显示：docker-compose version 1.24.1, build 4667896b，则安装成功

## 命令

**启动容器**

```sh
docker-compose up -d
```

**停止容器**

```sh
docker-compose down
```

## 其它

### mysql

```yaml

version: "3"

services:
  mysql:
    image: mysql
    container_name: mysql
    command:
      --default-authentication-plugin=mysql_native_password
      --character-set-server=utf8mb4
      --collation-server=utf8mb4_general_ci
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: 123456
      MYSQL_ROOT_HOST: '%'
      TZ: Asia/Shanghai
    ports:
      - 3306:3306
    networks:
      - adv-network
    volumes:
      - /home/docker/mysql/data:/var/lib/mysql
      - /home/docker/mysql/conf:/etc/mysql/conf.d
      - /home/docker/mysql/logs:/logs
    logging:
      options:
        max-size: "1g"

```


### nginx

```yaml
version: "3.0"

services:
    nginx:
        image: nginx:latest
        container_name: nginx
        volumes:
            - /home/docker/nginx/conf.d:/etc/nginx/conf.d
            - /home/docker/nginx/nginx.conf:/etc/nginx/nginx.conf
            - /home/docker/nginx/logs:/var/log/nginx
            - /home/docker/nginx/www:/usr/share/nginx
        environment:
            TZ: Asia/Shanghai
        ports:
            - "80:80"
            - "443:443"
        restart: always
        logging:
            options:
                max-size: "1g"
```

### rabbitmq

```yaml
version: "3"

services:
  rabbitmq:
      image: rabbitmq:management-alpine
      container_name: rabbitmq
      restart: always
      environment:
        RABBITMQ_DEFAULT_USER: root
        RABBITMQ_DEFAULT_PASS: 123456
        TZ: Asia/Shanghai
      ports:
        - 5672:5672
        - 15672:15672
      networks:
        - adv-network
      volumes:
        - /home/docker/rabbitmq/data:/var/lib/rabbitmq
        - /home/docker/rabbitmq/log:/var/log/rabbitmq
      logging:
        options:
          max-size: "1g"



```

### mongo

```yaml

version: "3"

services:
  mongodb:
    image: mongo:4.4.0
    container_name: mongodb
    restart: always
    environment:
        - MONGO_INITDB_ROOT_USERNAME=admin
        - MONGO_INITDB_ROOT_PASSWORD=1234567
    volumes:
        - /home/docker/mongodb/data:/data/db
        - /home/docker/mongodb/logs:/data/logs
    ports:
        - 27017:27017

```
