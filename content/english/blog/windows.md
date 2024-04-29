---
title: windows 使用笔记
date: 2022-04-04T05:00:00Z
image: /images/image-placeholder.png
categories:
  - Technology
tags:
  - windows
draft: true
image: /images/image-placeholder.png
---
微软视窗是由微软公司开发和销售的几个专有图形操作系统系列组成的。

<!--more-->

{{< toc >}}
## 如何在windows使用gcc和make命令

使用clion设置mingw

- 把C:\Program Files\JetBrains\CLion 2022.3.3\bin\mingw\bin路径加入path中
- 复制mingw32-make，然后重命名为make

# 常用命令

### 设置cmd命令行终端的代理


```
set http_proxy=http://127.0.0.1:7890
set https_proxy=http://127.0.0.1:7890
```



## 交叉编译

### Mac 下编译， Linux 或者 Windows 下去执行

复制代码

`# linux 下去执行 CGO_ENABLED=0  GOOS=linux  GOARCH=amd64  go build main.go # Windows 下去执行 CGO_ENABLED=0 GOOS=windows  GOARCH=amd64  go  build  main.go`

### Linux 下编译 ， Mac 或者 Windows 下去执行

复制代码

`# Mac  下去执行 CGO_ENABLED=0 GOOS=darwin  GOARCH=amd64  go build main.go # Windows 下执行 CGO_ENABLED=0 GOOS=windows  GOARCH=amd64  go build main.go`

### Windows 下执行 ， Mac 或 Linux 下去执行

需要写一个批处理程序，在里面去设置，因为windows 下的 terminal 不支持shell , 这跟 Mac 和 Linux下的有点不同

复制代码

`# Mac 下执行 SET  CGO_ENABLED=0 SET GOOS=darwin SET GOARCH=amd64 go build main.go`

复制代码

`# Linux 去执行 SET CGO_ENABLED=0 SET GOOS=linux SET GOARCH=amd64 go build main.go`
