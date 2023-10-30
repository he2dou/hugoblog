---
title: docker 工作笔记
date: 2022-04-04T05:00:00Z
image: /images/image-placeholder.png
tags:
  - docker
categories:
  - 技术
---

[Docker](https://www.docker.com/) 使开发变得高效和可预测，带走了重复的、平凡的配置任务，并在整个开发生命周期中用于快速、简单和可移植的应用开发。

<!--more-->

{{< toc >}}

# 第一节 环境安装和配置
## 安装环境[​](https://docs.littleriver.cc/v1/references/docker#%E5%AE%89%E8%A3%85%E7%8E%AF%E5%A2%83)

查看要求

Docker 要求 CentOS 系统的内核版本高于 3.10 ，查看本页面的前提条件来验证你的CentOS 版本是否支持 Docker

通过 uname -r 命令查看你当前的内核版本


```
uname -r
```

### 系统更新

COPY

```
sudo yum update
```

### [](https://littleriver.cc/docker#heading-httpsdocslittleriverccv1referencesdockere58db8e8bdbde697a7e78988e69cac "Permalink")卸载旧版本[​](https://docs.littleriver.cc/v1/references/docker#%E5%8D%B8%E8%BD%BD%E6%97%A7%E7%89%88%E6%9C%AC)

COPY

```
sudo yum remove docker  docker-common docker-selinux docker-engine
```

### [](https://littleriver.cc/docker#heading-httpsdocslittleriverccv1referencesdockere5ae89e8a385e99c80e8a681e79a84e8bdafe4bbb6e58c85 "Permalink")安装需要的软件包[​](https://docs.littleriver.cc/v1/references/docker#%E5%AE%89%E8%A3%85%E9%9C%80%E8%A6%81%E7%9A%84%E8%BD%AF%E4%BB%B6%E5%8C%85)

COPY

```
sudo yum install -y yum-utils device-mapper-persistent-data lvm2
```

### [](https://littleriver.cc/docker#heading-yumhttpsdocslittleriverccv1referencesdockere8aebee7bdaeyume6ba90 "Permalink")设置yum源[​](https://docs.littleriver.cc/v1/references/docker#%E8%AE%BE%E7%BD%AEyum%E6%BA%90)

COPY

```
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```

### [](https://littleriver.cc/docker#heading-httpsdocslittleriverccv1referencesdockere69fa5e79c8be68980e69c89e4bb93e5ba93e78988e69cac "Permalink")查看所有仓库版本[​](https://docs.littleriver.cc/v1/references/docker#%E6%9F%A5%E7%9C%8B%E6%89%80%E6%9C%89%E4%BB%93%E5%BA%93%E7%89%88%E6%9C%AC)

COPY

```
 yum list docker-ce --showduplicates | sort -r
```

### [](https://littleriver.cc/docker#heading-dockerhttpsdocslittleriverccv1referencesdockere5ae89e8a385docker "Permalink")安装docker[​](https://docs.littleriver.cc/v1/references/docker#%E5%AE%89%E8%A3%85docker)

COPY

```
 sudo yum install docker-ce -y  #由于repo中默认只开启stable仓库，故这里安装的是最新稳定版17.12.0 
 sudo yum install <FQPN>  # 例如：sudo yum install docker-ce-20.10.14-3.el7
```

### [](https://littleriver.cc/docker#heading-httpsdocslittleriverccv1referencesdockere590afe58aa8e5b9b6e58aa0e585a5e5bc80e69cbae590afe58aa8 "Permalink")启动并加入开机启动[​](https://docs.littleriver.cc/v1/references/docker#%E5%90%AF%E5%8A%A8%E5%B9%B6%E5%8A%A0%E5%85%A5%E5%BC%80%E6%9C%BA%E5%90%AF%E5%8A%A8)

COPY

```
sudo systemctl start docker 
sudo systemctl enable docker
```

### [](https://littleriver.cc/docker#heading-httpsdocslittleriverccv1referencesdockere9aa8ce8af81e5ae89e8a385e698afe590a6e68890e58a9f "Permalink")验证安装是否成功[​](https://docs.littleriver.cc/v1/references/docker#%E9%AA%8C%E8%AF%81%E5%AE%89%E8%A3%85%E6%98%AF%E5%90%A6%E6%88%90%E5%8A%9F)

COPY

```
docker version
```

备注

解决普通用户权限问题

COPY

```
sudo groupadd docker     #添加docker用户组
sudo gpasswd -a $USER docker     #将登陆用户加入到docker用户组中
newgrp docker     #更新用户组
sudo systemctl restart docker   # 重启docker
docker ps    #测试docker命令是否可以使用sudo正常使用
```

## [](https://littleriver.cc/docker#heading-httpsdocslittleriverccv1referencesdockere697a5e5bf97e69fa5e8afa2 "Permalink")日志查询[​](https://docs.littleriver.cc/v1/references/docker#%E6%97%A5%E5%BF%97%E6%9F%A5%E8%AF%A2)

### [](https://littleriver.cc/docker#heading-docker100httpsdocslittleriverccv1referencesdockere69fa5e8afa2dockere697a5e5bf97e69c80e5908e100e8a18c "Permalink")查询docker日志，最后100行[​](https://docs.littleriver.cc/v1/references/docker#%E6%9F%A5%E8%AF%A2docker%E6%97%A5%E5%BF%97%E6%9C%80%E5%90%8E100%E8%A1%8C)

COPY

```
docker logs -f draining_admin --tail 100
```

## [](https://littleriver.cc/docker#heading-httpsdocslittleriverccv1referencesdockere5b8b8e794a8e591bde4bba4 "Permalink")常用命令[​](https://docs.littleriver.cc/v1/references/docker#%E5%B8%B8%E7%94%A8%E5%91%BD%E4%BB%A4)

COPY

```
docker ps // 查看所有正在运行容器 
docker stop containerId // containerId 是容器的ID 
docker ps -a // 查看所有容器 
docker ps -a -q // 查看所有容器ID
```

### [](https://littleriver.cc/docker#heading-httpsdocslittleriverccv1referencesdockere588a0e999a4e9959ce5838f "Permalink")删除镜像[​](https://docs.littleriver.cc/v1/references/docker#%E5%88%A0%E9%99%A4%E9%95%9C%E5%83%8F)

COPY

```
# 指定image
docker rmi image
# 所有的
docker rmi $(docker images -q) 
# 删除为none的镜像
docker rmi $(docker images -f "dangling=true" -q)
```

### [](https://littleriver.cc/docker#heading-httpsdocslittleriverccv1referencesdockere585b3e997ade5aeb9e599a8 "Permalink")关闭容器[​](https://docs.littleriver.cc/v1/references/docker#%E5%85%B3%E9%97%AD%E5%AE%B9%E5%99%A8)

COPY

```
# 指定image
docker stop image

# 所有的
docker stop $(docker ps -a -q)

# 删除指定名称的所有容器
docker rmi -f $(docker images  |  grep "registry.cn*"  | awk '{print $3}')
```

### [](https://littleriver.cc/docker#heading-httpsdocslittleriverccv1referencesdockere590afe58aa8e5aeb9e599a8 "Permalink")启动容器[​](https://docs.littleriver.cc/v1/references/docker#%E5%90%AF%E5%8A%A8%E5%AE%B9%E5%99%A8)

COPY

```
# 指定
docker start 容器id
# 所有的
docker start $(docker ps -a -q)
```

### [](https://littleriver.cc/docker#heading-httpsdocslittleriverccv1referencesdockere588a0e999a4e5aeb9e599a8 "Permalink")删除容器[​](https://docs.littleriver.cc/v1/references/docker#%E5%88%A0%E9%99%A4%E5%AE%B9%E5%99%A8)

COPY

```
# 所有的
docker rm $(docker ps -a -q)
```

## [](https://littleriver.cc/docker#heading-httpsdocslittleriverccv1referencesdockere5aeb9e599a8e69e84e5bbba "Permalink")容器构建[​](https://docs.littleriver.cc/v1/references/docker#%E5%AE%B9%E5%99%A8%E6%9E%84%E5%BB%BA)

COPY

```
docker build -t bifrost .
```

## [](https://littleriver.cc/docker#heading-httpsdocslittleriverccv1referencesdockere5aeb9e599a8e8bf90e8a18c "Permalink")容器运行[​](https://docs.littleriver.cc/v1/references/docker#%E5%AE%B9%E5%99%A8%E8%BF%90%E8%A1%8C)

COPY

```
docker run --name=mapbridge_sync -d  -p 21036:21036 -v /docker/mapbridge_sync/etc:/src/mapbridge_sync/etc  bifrost
```

## [](https://littleriver.cc/docker#heading-httpsdocslittleriverccv1referencesdockere7a9bae997b4e6b885e79086 "Permalink")空间清理[​](https://docs.littleriver.cc/v1/references/docker#%E7%A9%BA%E9%97%B4%E6%B8%85%E7%90%86)

COPY

````
# 查看镜像占用空间
docker system df

# 删除所有dangling镜像（即无tag的镜像）：
docker rmi $(docker images | grep "^" | awk "{print $3}")

# 删除所有dangling数据卷（即无用的Volume）：
```shell
docker volume rm $(docker volume ls -qf dangling=true)
````