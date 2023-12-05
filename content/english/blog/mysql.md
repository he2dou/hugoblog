---
title: mysql 数据库服务
date: 2022-04-04T05:00:00Z
categories:
  - 技术
tags:
  - mysql
---

<img src="https://pic.imgdb.cn/item/656f2c0ec458853aef772f5d.jpg" />

MySQL数据库服务是一个完全托管的数据库服务，可使用世界上最受欢迎的开源数据库来部署云原生应用程序。

<!--more-->

{{< toc >}}

## 安装
docker-compose命令安装，脚本如下

```yaml

version: "3"

services:
  mysql:
    image: mysql
    container_name: mysql
    command:
      --default-authentication-plugin=mysql_native_password
      --character-set-server=utf8mb4
      --collation-server=utf8mb4_general_ci
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: 123456
      MYSQL_ROOT_HOST: '%'
      TZ: Asia/Shanghai
    ports:
      - 3306:3306
    networks:
      - adv-network
    volumes:
      - /home/docker/mysql/data:/var/lib/mysql
      - /home/docker/mysql/conf:/etc/mysql/conf.d
      - /home/docker/mysql/logs:/logs
    logging:
      options:
        max-size: "1g"

```

然后运行 **docker-compose up -d** 拉取和启动容器服务


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

## 操作

**一般操作命令**


```sql
create database hbfmxt DEFAULT CHARSET utf8mb4 COLLATE utf8mb4_general_ci;

GRANT ALL PRIVILEGES ON hbfmxt.* TO 'ppl'@'%' IDENTIFIED BY '123456!' WITH GRANT OPTION;

flush privileges;
```

创建数据库


```sql
CREATE DATABASE test DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;
```

创建数据表


```sql
CREATE TABLE IF NOT EXISTS runoob_tbl( 
  runoob_id INT UNSIGNED AUTO_INCREMENT, 
  runoob_title VARCHAR(100) NOT NULL, 
  runoob_author VARCHAR(40) NOT NULL, 
  submission_date DATE, PRIMARY KEY ( runoob_id ) 
  )ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;
```

查看所有数据库


```sql
show databases;
```

查看所有数据表


```sql
show tables;
```

查询数据


```sql
select * from runoob_tbl;
```

**插入数据**


```sql
INSERT INTO runoob_tbl (runoob_title, runoob_author, submission_date) 
VALUES ("学习 MySQL", "菜鸟教程", NOW()); 

INSERT INTO runoob_tbl (runoob_title, runoob_author, submission_date) 
VALUES ("学习 ClickHouse", "菜鸟教程", NOW());
```

**更新数据**


```sql
update runoob_tbl set runoob_author='1' where runoob_id=1;
```

**删除数据库**


```sql
DROP database test;
```

**删除数据表**

```sql
DROP table runoob_tbl;
```

**删除行数据**


```sql
delete from runoob_tbl where runoob_id=1;
```