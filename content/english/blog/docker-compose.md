---
title: docker-compose 工作笔记
date: 2022-04-04T05:00:00Z
categories:
  - 技术
tags:
  - docker
---

<img src="https://i.imgur.com/SZ0I0KW.jpg" title="source: imgur.com" />

Docker Compose 是 docker 提供的一个命令行工具，用来定义和运行由多个容器组成的应用。

<!--more-->

{{< toc >}}

## 安装[​](https://docs.littleriver.cc/v1/references/docker-compose#%E5%AE%89%E8%A3%85)

确保本机已经安装了docker，关于docker安装请参考链接：[https://www.cnblogs.com/xiao987334176/p/11771657.html](https://www.cnblogs.com/xiao987334176/p/11771657.html)

> 参考官方链接：[https://docs.docker.com/compose/install/](https://docs.docker.com/compose/install/)

### 运行此命令下载docker compose的当前稳定版本：



```
sudo curl -L https://download.fastgit.org/docker/compose/releases/download/1.27.4/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
```

### 对二进制文件应用可执行权限



```
sudo chmod +x /usr/local/bin/docker-compose
```

### 创建链接



```
ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
```

### 测试

查看版本



```
docker-compose --version
```

docker-compose version 1.24.1, build 4667896b

## 命令

### 启动



```
docker-compose up -d
```

### 停止



```
docker-compose down
```