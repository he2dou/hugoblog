---
title: nginx 轻量级Web服务器
date: 2023-04-04T05:00:00Z
tags:
  - nginx
categories:
  - 技术
author: harry
---

<img src="https://s2.loli.net/2023/12/05/z7I3f8TZlyAqLsP.jpg" />

Nginx 是一款轻量级的Web服务器、反向代理，内存占用少，启动快，高并发能力强，在互联网项目中广泛应用。

<!--more-->

{{< toc >}}

## 安装

以下是**docker-compose.yaml**文件示例


```yaml
version: "3.0"

services:
    nginx:
        image: nginx:latest
        container_name: nginx   
        environment:
            TZ: Asia/Shanghai
        volumes:
            - /alidata/docker/nginx/conf.d:/etc/nginx/conf.d
            - /alidata/docker/nginx/nginx.conf:/etc/nginx/nginx.conf
            - /alidata/docker/nginx/logs:/var/log/nginx
            - /alidata/docker/nginx/www:/usr/share/nginx
        ports:
            - "80:80"
            - "443:443"
        restart: always
        logging:
            options:
                max-size: "1g"
```

然后运行 **docker-compose up -d** 拉取和启动容器服务

---

## 使用

**ssl证书配置**

在nginx.conf文件添加如下内容


```shell
server {
        listen       80;
        server_name  xxx.com;
        rewrite ^/(.*) https://$server_name$request_uri? permanent;
}


server {
        listen       443 ssl;
        server_name  xxx.com;

        ssl_certificate /etc/nginx/conf.d/ssl/xxx.pem;
        ssl_certificate_key /etc/nginx/conf.d/ssl/xxx.key;
        ssl_session_timeout 5m;
        ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
        ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
        ssl_prefer_server_ciphers on;

        location / {
                root   /usr/share/nginx/xxx.com;
                 try_files $uri $uri/ /index.html;
                index  index.html index.htm;
        }

        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
                root   html;
        }

  location /api/ {

    proxy_set_header X-Real-IP  $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
    proxy_set_header Host $http_host;

    add_header Access-Control-Allow-Origin *;
    add_header Access-Control-Allow-Methods 'GET,POST,OPTIONS,DELETE,PUT';
    add_header Access-Control-Allow-Headers 'DNT,X-Mx-ReqToken,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Authorization';

    if ($request_method = 'OPTIONS') {
        return 204;
    }
    #proxy_set_header X-Real-IP  $remote_addr;
    #proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_pass http://ip:port;
  }

}
```

**跨域支持**

在nginx.conf文件添加如下内容

```shell
    add_header Access-Control-Allow-Origin *;
    add_header Access-Control-Allow-Methods 'GET,POST,OPTIONS,DELETE,PUT';
    add_header Access-Control-Allow-Headers 'DNT,X-Mx-ReqToken,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Authorization';
```

**代理端口服务**

在nginx.conf文件添加如下内容


```shell
location /api {
    proxy_set_header X-Real-IP  $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
    proxy_set_header Host $http_host;

    add_header Access-Control-Allow-Origin *;
    add_header Access-Control-Allow-Methods 'GET,POST,OPTIONS,DELETE,PUT';
    add_header Access-Control-Allow-Headers 'DNT,X-Mx-ReqToken,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Authorization';

    if ($request_method = 'OPTIONS') {
        return 204;
    }
    proxy_pass http://ip:port;
  }
```


**websocket 配置**
```sh

location /ws {
     proxy_pass http://ip:port;
     proxy_http_version 1.1;
     proxy_set_header Upgrade $http_upgrade;
     proxy_set_header Connection "upgrade";
  }

```

---

## 其它

容器nginx配置重新加载

```shell
docker exec -it babc5ab66db7 nginx -s reload
```

测试nginx配置文件是否ok

```shell
docker exec -it babc5ab66db7 nginx -t

```