---
title: docker 虚拟化容器技术
date: 2023-10-30T05:00:00Z
tags:
  - docker
categories:
  - 技术
author: hht
---

<img src="https://pic.imgdb.cn/item/656f2ca9c458853aef7aa66b.jpg" alt="免费图床网站">

[Docker](https://www.docker.com/) 使开发变得高效和可预测，减少重复的配置任务，并在整个开发中用于快速、简单的应用开发。



<!--more-->

{{< toc >}}


Docker 要求 CentOS 系统的内核版本高于 3.10 ，查看本页面的前提条件来验证你的CentOS 版本是否支持 Docker

通过 uname -r 命令查看你当前的内核版本


```shell
uname -r
```

### 系统更新



```shell
sudo yum update -y
```

### 卸载旧版本


```shell
sudo yum remove docker  docker-common docker-selinux docker-engine
```

### 安装需要的软件包



```shell
sudo yum install -y yum-utils device-mapper-persistent-data lvm2
```

### 设置yum源



```shell
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```

### 查看所有仓库版本



```shell
 yum list docker-ce --showduplicates | sort -r
```

### 安装docker



```shell
 sudo yum install docker-ce -y 
```

### 启动并加入开机启动



```shell
sudo systemctl start docker 
sudo systemctl enable docker
```

### 验证安装是否成功

查看docker版本


```shell
docker version
```


 **创建docker用户组, 应用用户加入docker组**



```shell
1. 创建docker用户组

sudo groupadd docker

2. 应用用户加入docker用户组

sudo usermod -aG docker ${USER}

3. 重启docker服务

sudo systemctl restart docker

4. 重新打开终端才可生效登入
```



## 日志查询

### 查询docker日志，最后100行


```shell
docker logs -f draining_admin --tail 100
```

## 常用命令



```sh
# 查看所有正在运行容器 
docker ps 

# 停止容器
docker stop containerId

docker ps -a // 查看所有容器 

docker ps -a -q // 查看所有容器ID

# 进入mysql容器内部
docker exec -it mysql bash

```

### 删除镜像



```shell

# 指定image
docker rmi image

# 所有的
docker rmi $(docker images -q) 

# 删除为none的镜像
docker rmi $(docker images -f "dangling=true" -q)

# 删除指定名称的所有容器
docker rmi -f $(docker images  |  grep "registry.cn*"  | awk '{print $3}')

```

### 关闭容器



```shell
# 指定image
docker stop image

# 所有的
docker stop $(docker ps -a -q)


```

### 启动容器



```shell
# 指定
docker start 容器id
# 所有的
docker start $(docker ps -a -q)
```

### 删除容器



```shell
# 所有的
docker rm $(docker ps -a -q)
```

### 容器构建



```shell
docker build -t bifrost .
```

### 容器运行



```shell
docker run --name=[name] -d  -p 21036:21036 -v /docker/mapbridge_sync/etc:/src/mapbridge_sync/etc  bifrost
```

### 空间清理


````shell
# 查看镜像占用空间
docker system df

# 删除所有dangling镜像（即无tag的镜像）：
docker rmi $(docker images | grep "^" | awk "{print $3}")

# 删除所有dangling数据卷（即无用的Volume）：
docker volume rm $(docker volume ls -qf dangling=true)

# 删除容器内的临时文件
docker exec [name] sh -c "rm -rf /tmp/* /var/tmp/*"

```