---
title: redis 缓存数据技术
date: 2023-12-05
author: hht
categories:
  - 技术
tags:
  - redis
---

<img src="https://pic.imgdb.cn/item/656f2c19c458853aef7773c0.jpg" />

Redis 是一种开源（BSD 许可）内存数据结构存储，可用作数据库、缓存和消息代理。

<!--more-->

{{< toc >}}

## 安装

使用docker容器安装，方便快捷


以下是**docker-compose.yaml**文件示例

```yaml
version: "3"

services:
  redis:
      image: redis:6.2.6
      container_name: redis
      command: redis-server --requirepass you_password --appendonly yes
      restart: always
      environment:
        TZ: Asia/Shanghai
        LANG: en_US.UTF-8
      ports:
        - 6379:6379
      volumes:
        - /home/docker/redis/data:/data
        - /home/docker/redis/conf:/usr/local/etc/redis

```

然后运行 **docker-compose up -d** 拉取和启动容器服务

---

## 使用

**登录redis服务**

```sh
redis-cli -h 主机 -p 6379 -a 密码 --raw
```

**进入redis容器**

```sh
docker exec -it redis bash
```


**清除所有key**


```sh
FLUSHALL
```

**查看所有key**


```sh
keys *
```

---

## 其它

### python批量删除key

```python
import redis

r = redis.Redis(host='127.0.0.1', port=6379, password='you_password', db=0)

for key in r.scan_iter(match='cacheKey:*'):
    print(key)
    r.delete(key)
    #if r.ttl(key) < 14400:
    #    print('----del----', key)
    #    r.delete(key)
```
