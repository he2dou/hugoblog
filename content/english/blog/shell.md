---
title: shell 工作笔记
date: 2022-04-04T05:00:00Z
image: /images/image-placeholder.png
categories:
  - 技术
tags:
  - shell
---
Shell是一个用C 语言编写的程序，它是用户使用Linux 的桥梁。_Shell_ 既是一种命令语言，又是一种程序设计语言。


<!--more-->

{{< toc >}}


## 实例[​](https://docs.littleriver.cc/v1/references/shell#%E5%AE%9E%E4%BE%8B)

### 帮我写一个读取txt文本每行并打印的shell脚本

脚本名称：read_[text.sh](http://text.sh/)

COPY

```
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



```
chmod +x read_text.sh
```

执行脚本



```
./read_text.sh filename
```

### 批量执行clickhouse数据同步

脚本文件：[test.sh](http://test.sh/)



```
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
while read table; do
  echo "$table"
  clickhouse-client --host ip --port 9000 --query="insert into database.$table  SELECT * FROM  remote('xxx.com:9000', 'database', $table, 'user', 'password')"
done < $1
```

赋予权限



```
chmod +x test.sh
```

测试文本: a.txt



```
table1
table2
```

执行脚本



```
./test.sh a.txt
```