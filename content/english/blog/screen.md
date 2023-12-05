---
title: screen 窗口管理器
date: 2023-11-27T05:00:00Z
categories:
  - 技术
tags:
  - screen
author: harry
---

<img src="https://pic.imgdb.cn/item/656f2ca9c458853aef7aa713.jpg" alt="免费图床网站">

Linux Screen是一个全屏窗口管理器，它可以创建多个窗口，并在其间进行切换。

<!--more-->

### 1. 安装screen

```sh
sudo yum install -y screen
```


### 2. 常用命令

```sh
# 进入工程目录
cd /alidata/redis-shake-v2.0.3

# 新建窗口
screen -S redis-shake

# 执行命令
screen ./redis-shake.linux -conf=redis-shake.conf -type=sync

# 分离会话
ctrl+a，d

# 列出screen
screen -ls

# 进入screen
screen -r redis-shake

# 关闭 screen
screen -X -S redis-shake quit

```



> 参考地址： [https://blog.csdn.net/hejunqing14/article/details/50338161](https://blog.csdn.net/hejunqing14/article/details/50338161)
