---
title: mysql 工作笔记
date: 2022-04-04T05:00:00Z
image: /images/image-placeholder.png
categories:
  - 技术
tags:
  - mysql
---
<!--more-->

{{< toc >}}

## 数据库安装
docker-compose命令安装，脚本如下



**创建数据库**


```
create database hbfmxt DEFAULT CHARSET utf8mb4 COLLATE utf8mb4_general_ci;

GRANT ALL PRIVILEGES ON hbfmxt.* TO 'ppl'@'%' IDENTIFIED BY 'ppl_123456!' WITH GRANT OPTION;

flush privileges;
```

**任务**

- 熟悉项目代码和搭建环境
- 调研和选型数据同步方案


**调研**

**1.Binlog三种模式**

- statement

每一条会修改数据的 SQL 都会记录到 master 的 bin-log 中。slave 在复制的时候 SQL 进程会解析成和原来 master 端执行过的相同的 SQL 再次执行。

**注意：只记录每条数据的变化，但是某些情况下会导致数据同步缺失**

- mixed

在 Mixed 模式下，MySQL 会根据执行的每一条具体的 SQL 语句来区分对待记录的日志形式，也就是在 statement 和 row 之间选择一种。

**注意：一般情况下以S模式处理，特殊情况下为R模式，存在不确定性**

- row 日志中会记录成每一行数据被修改的形式，然后在 slave 端再对相同的数据进行修改。

**注意：记录每条数据的变化及上下文相关信息，在日志量和性能上有影响**

**2.Mysql 操作**

**创建**

创建数据库


```
CREATE DATABASE test DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;
```

创建数据表


```
CREATE TABLE IF NOT EXISTS runoob_tbl( runoob_id INT UNSIGNED AUTO_INCREMENT, runoob_title VARCHAR(100) NOT NULL, runoob_author VARCHAR(40) NOT NULL, submission_date DATE, PRIMARY KEY ( runoob_id ) )ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;
```

**查询**

# 查看所有数据库


```
show databases;
```

# 查看所有数据表


```
show tables;
```

# 查询数据


```
select * from runoob_tbl;
```

**插入**


```
INSERT INTO runoob_tbl (runoob_title, runoob_author, submission_date) 
VALUES ("学习 MySQL", "菜鸟教程", NOW()); 

INSERT INTO runoob_tbl (runoob_title, runoob_author, submission_date) 
VALUES ("学习 ClickHouse", "菜鸟教程", NOW());
```

**更新**


```
update runoob_tbl set runoob_author='1' where runoob_id=1;
```

**删除**

# 库


```
DROP database test;
```

# 表



```
DROP table runoob_tbl;
```

# 行数据


```
delete from runoob_tbl where runoob_id=1;
```