---
title: redis 工作笔记
date: 2022-04-04T05:00:00Z
image: /images/image-placeholder.png
author: hht
categories:
  - 技术
tags:
  - redis
---
Redis 是一种开源（BSD 许可）内存数据结构存储，可用作数据库、缓存和消息代理。

<!--more-->

{{< toc >}}

## 安装[​](https://docs.littleriver.cc/v1/references/redis#%E5%AE%89%E8%A3%85)

## [](https://littleriver.cc/redis#heading-httpsdocslittleriverccv1referencesredise591bde4bba4 "Permalink")命令[​](https://docs.littleriver.cc/v1/references/redis#%E5%91%BD%E4%BB%A4)

### [](https://littleriver.cc/redis#heading-redis-cli-httpsdocslittleriverccv1referencesredise4bdbfe794a8redis-cli-e799bbe5bd95e4b8bbe69cba "Permalink")使用redis-cli 登录主机[​](https://docs.littleriver.cc/v1/references/redis#%E4%BD%BF%E7%94%A8redis-cli-%E7%99%BB%E5%BD%95%E4%B8%BB%E6%9C%BA)

COPY

```
redis-cli -h 主机 -p 6379 -a 密码 --raw
```

### [](https://littleriver.cc/redis#heading-redis-dockerhttpsdocslittleriverccv1referencesredise799bbe5bd95redis-dockere5aeb9e599a8 "Permalink")登录redis docker容器[​](https://docs.littleriver.cc/v1/references/redis#%E7%99%BB%E5%BD%95redis-docker%E5%AE%B9%E5%99%A8)

COPY

```
docker exec -it redis bash
```

### [](https://littleriver.cc/redis#heading-httpsdocslittleriverccv1referencesredise5af86e7a081e8bf9ee68ea5 "Permalink")密码连接[​](https://docs.littleriver.cc/v1/references/redis#%E5%AF%86%E7%A0%81%E8%BF%9E%E6%8E%A5)

COPY

```
redis-cli -a donson.redis
```

### [](https://littleriver.cc/redis#heading-keyhttpsdocslittleriverccv1referencesredise6b885e999a4e68980e69c89key "Permalink")清除所有key[​](https://docs.littleriver.cc/v1/references/redis#%E6%B8%85%E9%99%A4%E6%89%80%E6%9C%89key)

COPY

```
FLUSHALL
```

### [](https://littleriver.cc/redis#heading-keyhttpsdocslittleriverccv1referencesredise69fa5e79c8be68980e69c89key "Permalink")查看所有key[​](https://docs.littleriver.cc/v1/references/redis#%E6%9F%A5%E7%9C%8B%E6%89%80%E6%9C%89key)

COPY

```
keys *
```

## [](https://littleriver.cc/redis#heading-httpsdocslittleriverccv1referencesredise9ab98e7baa7 "Permalink")高级[​](https://docs.littleriver.cc/v1/references/redis#%E9%AB%98%E7%BA%A7)

### [](https://littleriver.cc/redis#heading-redis-shake-httpsdocslittleriverccv1referencesredisredis-shake-e5908ce6ada5e5b7a5e585b7 "Permalink")redis-shake 同步工具[​](https://docs.littleriver.cc/v1/references/redis#redis-shake-%E5%90%8C%E6%AD%A5%E5%B7%A5%E5%85%B7)

./redis-shake.linux -conf=redis-shake.conf -type=sync

## [](https://littleriver.cc/redis#heading-httpsdocslittleriverccv1referencesredise99984e5bd95 "Permalink")附录[​](https://docs.littleriver.cc/v1/references/redis#%E9%99%84%E5%BD%95)

### [](https://littleriver.cc/redis#heading-5a6i5oi356uv6l2v5lu25o6o6i2q "Permalink")客户端软件推荐