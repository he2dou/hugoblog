---
title: docker-compose 工作笔记
date: 2022-04-04T05:00:00Z
image: /images/image-placeholder.png
categories:
  - 技术
tags:
  - docker
---

Docker Compose 是 docker 提供的一个命令行工具，用来定义和运行由多个容器组成的应用。使用 compose，管理应用程序的各个服务，并由单个命令完成应用的创建和启动。

<!--more-->

{{< toc >}}
## [](https://littleriver.cc/docker-compose#heading-httpsdocslittleriverccv1referencesdocker-composee5ae89e8a385 "Permalink")安装[​](https://docs.littleriver.cc/v1/references/docker-compose#%E5%AE%89%E8%A3%85)

确保本机已经安装了docker，关于docker安装请参考链接：[https://www.cnblogs.com/xiao987334176/p/11771657.html](https://www.cnblogs.com/xiao987334176/p/11771657.html)

> 参考官方链接：[https://docs.docker.com/compose/install/](https://docs.docker.com/compose/install/)

### [](https://littleriver.cc/docker-compose#heading-docker-composehttpsdocslittleriverccv1referencesdocker-composee8bf90e8a18ce6ada4e591bde4bba4e4b88be8bdbddocker-composee79a84e5bd93e5898de7a8b3e5ae9ae78988e69cac "Permalink")运行此命令下载docker compose的当前稳定版本：[​](https://docs.littleriver.cc/v1/references/docker-compose#%E8%BF%90%E8%A1%8C%E6%AD%A4%E5%91%BD%E4%BB%A4%E4%B8%8B%E8%BD%BDdocker-compose%E7%9A%84%E5%BD%93%E5%89%8D%E7%A8%B3%E5%AE%9A%E7%89%88%E6%9C%AC)



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