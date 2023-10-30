---
title: clickhouse 工作笔记
date: 2022-04-04T05:00:00Z
image: /images/image-placeholder.png
tags:
  - clickhouse
---
<!--more-->

{{< toc >}}
# 第一节 环境安装和配置
## 环境安装

CentOS系统环境

```shell
sudo yum install -y yum-utils
sudo yum-config-manager --add-repo https://packages.clickhouse.com/rpm/clickhouse.repo
sudo yum install -y clickhouse-server clickhouse-client

sudo /etc/init.d/clickhouse-server start

clickhouse-client # or "clickhouse-client --password" if you set up a password.
```

