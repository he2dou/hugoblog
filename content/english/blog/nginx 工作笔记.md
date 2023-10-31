---
title: nginx 工作笔记
date: 2023-04-04T05:00:00Z
tags:
  - nginx
image: /images/image-placeholder.png
categories:
  - 技术
author: hht
---
Nginx 是一款轻量级的Web服务器、反向代理，内存占用少，启动快，高并发能力强，在互联网项目中广泛应用。

<!--more-->

{{< toc >}}

## 安装

保存下面脚本 `docker-compose.yaml` ，然后运行 `docker-compose up -d`

COPY

COPY

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
                max-size: "10g"
```

## [](https://littleriver.cc/nginx?source=more_articles_bottom_blogs#heading-ssl-httpsdocslittleriverccv1referencesnginxssl-e8af81e4b9a6 "Permalink")SSL 证书[​](https://docs.littleriver.cc/v1/references/nginx#ssl-%E8%AF%81%E4%B9%A6)

### [](https://littleriver.cc/nginx?source=more_articles_bottom_blogs#heading-httpsdocslittleriverccv1referencesnginxe794b3e8afb7e8af81e4b9a6 "Permalink")申请证书[​](https://docs.littleriver.cc/v1/references/nginx#%E7%94%B3%E8%AF%B7%E8%AF%81%E4%B9%A6)

### [](https://littleriver.cc/nginx?source=more_articles_bottom_blogs#heading-httpsdocslittleriverccv1referencesnginxe8af81e4b9a6e9858de7bdae "Permalink")证书配置[​](https://docs.littleriver.cc/v1/references/nginx#%E8%AF%81%E4%B9%A6%E9%85%8D%E7%BD%AE)

在nginx.conf文件添加如下内容

COPY

COPY

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
    proxy_pass http://172.16.107.255:8005;
  }

}
```

## [](https://littleriver.cc/nginx?source=more_articles_bottom_blogs#heading-httpsdocslittleriverccv1referencesnginxe8b7a8e59f9fe694afe68c81 "Permalink")跨域支持[​](https://docs.littleriver.cc/v1/references/nginx#%E8%B7%A8%E5%9F%9F%E6%94%AF%E6%8C%81)

在nginx.conf文件添加如下内容

COPY

COPY

```shell
    add_header Access-Control-Allow-Origin *;
    add_header Access-Control-Allow-Methods 'GET,POST,OPTIONS,DELETE,PUT';
    add_header Access-Control-Allow-Headers 'DNT,X-Mx-ReqToken,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Authorization';
```

## [](https://littleriver.cc/nginx?source=more_articles_bottom_blogs#heading-iphttpsdocslittleriverccv1referencesnginxe4bba3e79086e79c9fe5ae9eip "Permalink")代理真实IP[​](https://docs.littleriver.cc/v1/references/nginx#%E4%BB%A3%E7%90%86%E7%9C%9F%E5%AE%9Eip)

在nginx.conf文件添加如下内容


```
    proxy_set_header X-Real-IP  $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
    proxy_set_header Host $http_host;
```