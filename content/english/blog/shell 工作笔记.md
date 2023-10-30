---
title: shell 工作笔记
date: 2022-04-04T05:00:00Z
image: /images/image-placeholder.png
categories:
  - 技术
tags:
  - shell
---

<!--more-->

{{< toc >}}

## 实例[​](https://docs.littleriver.cc/v1/references/shell#%E5%AE%9E%E4%BE%8B)

### [](https://littleriver.cc/shell#heading-txtshellhttpsdocslittleriverccv1referencesshelle5b8aee68891e58699e4b880e4b8aae8afbbe58f96txte69687e69cace6af8fe8a18ce5b9b6e68993e58db0e79a84shelle8849ae69cac "Permalink")帮我写一个读取txt文本每行并打印的shell脚本[​](https://docs.littleriver.cc/v1/references/shell#%E5%B8%AE%E6%88%91%E5%86%99%E4%B8%80%E4%B8%AA%E8%AF%BB%E5%8F%96txt%E6%96%87%E6%9C%AC%E6%AF%8F%E8%A1%8C%E5%B9%B6%E6%89%93%E5%8D%B0%E7%9A%84shell%E8%84%9A%E6%9C%AC)

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

COPY

```
chmod +x read_text.sh
```

执行脚本

COPY

```
./read_text.sh filename
```

### [](https://littleriver.cc/shell#heading-clickhousehttpsdocslittleriverccv1referencesshelle689b9e9878fe689a7e8a18cclickhousee695b0e68daee5908ce6ada5 "Permalink")批量执行clickhouse数据同步[​](https://docs.littleriver.cc/v1/references/shell#%E6%89%B9%E9%87%8F%E6%89%A7%E8%A1%8Cclickhouse%E6%95%B0%E6%8D%AE%E5%90%8C%E6%AD%A5)

脚本文件：[test.sh](http://test.sh/)

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
while read table; do
  echo "$table"
  clickhouse-client --host ip --port 9000 --query="insert into database.$table  SELECT * FROM  remote('xxx.com:9000', 'database', $table, 'user', 'password')"
done < $1
```

赋予权限

COPY

```
chmod +x test.sh
```

测试文本: a.txt

COPY

```
table1
table2
```

执行脚本

COPY

```
./test.sh a.txt
```