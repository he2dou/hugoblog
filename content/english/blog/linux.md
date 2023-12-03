---
title: linux 开源操作系统
date: 2022-04-04T05:00:00Z
categories:
  - 技术
tags:
  - linux
author: harry
---

<img src="https://i.imgur.com/4lY5ANB.jpg" title="source: imgur.com" />

Linux遵循GNU通用公共许可证，个人和机构都可以使用Linux所有底层源代码，也可以自由地修改和再发布。

<!--more-->

{{< toc >}}

# Vim

## 查找单词并替换

```shell
:%s/old_word/new_word/g
```



# Natstat

netstat命令的功能是显示网络连接、路由表和网络接口的信息，可以让用户得知有哪些网络连接正在运作。在日常工作中，我们最常用的也就两个参数，即netstat –an，如下所示：

1. [root@tiaobanji ~]# netstat -an  
2. Active Internet connections (servers and established)  
3. Proto Recv-Q Send-Q Local Address               Foreign Address             State        
4. tcp        0      0 0.0.0.0:50020               0.0.0.0:*                   LISTEN        
5. tcp        0      0 127.0.0.1:199               0.0.0.0:*                   LISTEN        
6. tcp        0      0 127.0.0.1:9000              0.0.0.0:*                   LISTEN        
7. tcp        0      0 127.0.0.1:41224             0.0.0.0:*                   LISTEN        
8. tcp        0      0 127.0.0.1:21224             0.0.0.0:*                   LISTEN     


参数中stat（状态）的含义如下：
  
**LISTEN**：侦听来自远方的TCP端口的连接请求；  
**SYN-SENT**：在发送连接请求后等待匹配的连接请求；  
**SYN-RECEIVED**：在收到和发送一个连接请求后等待对方对连接请求的确认；  
**ESTABLISHED**：代表一个打开的连接，我们常用此作为并发连接数；  
**FIN-WAIT-1**：等待远程TCP连接中断请求，或先前的连接中断请求的确认；  
**FIN-WAIT-2**：从远程TCP等待连接中断请求；  
**CLOSE-WAIT**：等待从本地用户发来的连接中断请求；  
**CLOSING**：等待远程TCP对连接中断的确认；  
**LAST-ACK**：等待原来发向远程TCP的连接中断的确认；  
**TIME-WAIT**：等待足够的时间以确保远程TCP连接收到中断请求的确认；  
**CLOSED**：没有任何连接状态；

## 在日常工作中用

我们可以用shell组合命令来查看服务器的TCP连接状态并汇总，命令如下：  

```shell
netstat -an |grep 'ESTABLISHED' |grep 'tcp' |wc -l
```

参数说明：  
**CLOSED**：没有连接活动或正在进行的；  
**LISTEN**：服务器正在等待的进入呼叫；  
**SYN_RECV**：一个连接请求已经到达，等待确认；  
**SYN_SENT**：应用已经开始，打开一个连接；  
**ESTABLISHED**：正常数据传输状态，也可以近似的理解为当前服务器的并发数；  
**FIN_WAIT1**：应用已经完成；  
**FIN_WAIT2**：另一边同意释放；  
**ITMED_WAIT**：等待所有分组死掉；  
**CLOSING**：两边同时尝试关闭；  
**TIME_WAIT**：另一边已初始化一个释放；  
**LAST_ACK**：等待所有分组死掉；  


## 查看端口连接数


```shell
netstat -nat|grep -i "80"|wc -l
```


## 其它使用方法


```shell
一、查看哪些IP连接本机

netstat -an

二、查看TCP连接数

1）统计80端口连接数

netstat -nat|grep -i "80"|wc -l

2）统计httpd协议连接数

ps -ef|grep httpd|wc -l

3）统计已连接上的，状态为“established

netstat -na|grep ESTABLISHED|wc -l

4）查出哪个IP地址连接最多,将其封了.

netstat -na|grep ESTABLISHED|awk {print $5}|awk -F: {print $1}|sort|uniq -c|sort -r +0n netstat -na|grep SYN|awk {print $5}|awk -F: {print $1}|sort|uniq -c|sort -r +0n

5）centOS服务器 netstat命令 查看TCP连接数信息

netstat -an |grep 'ESTABLISHED' |grep 'tcp' |wc -l
```



# SCP

```shell
# 统计句柄数量 
lsof |grep TCP|wc -l 

# 进程句柄 
ll /proc/pid/fd 

# 统计进程句柄数 
ll /proc/pid/fd|wc -l 

# 示例 
scp -r release mint@192.168.28.36:/home/mint 
scp -r Config root@192.168.28.25:/usr/local/tanex.com/match

```

# 磁盘挂载

** **文件脚本**

下载地址：



### 帮我写一个读取txt文本每行并打印的shell脚本

脚本名称：read_[text.sh](http://text.sh/)


```sh
#!/bin/bash

# 检查参数
if [ $# -ne 1 ]; then
  echo "用法：$0 filename"
  exit 1
fi

# 判断文件是否存在
if [ ! -f $1 ]; then
  echo "文件不存在！"
  exit 1
fi

# 读取文件并打印每行内容
while read line; do
  echo "$line"
done < $1
```

赋予权限



```sh
chmod +x read_text.sh
```

执行脚本



```sh
./read_text.sh filename
```