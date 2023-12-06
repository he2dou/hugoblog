---
title: kafka 消息队列服务
date: 2022-04-04T05:00:00Z
categories:
  - 技术
author: harry
tags:
  - kafka
draft: false
---

<img src="https://pic.imgdb.cn/item/656f2ad9c458853aef7090e2.jpg" />

Kafka 是一个开源的分布式事件流平台，被数千家公司用于高性能数据管道、流分析、数据集成和关键任务应用。

<!--more-->

{{< toc >}}



```shell

# 创建主题 
./kafka-topics.sh --create --zookeeper 192.168.0.170:2182 --replication-factor 2 --partitions 3 --topic partopic 

# 查看kafka topic列表 
./kafka-topics.sh --list --zookeeper 192.168.0.170:2182 ./kafka-topics.sh --delete --zookeeper 192.168.0.170:2182 --topic partopic 

# 查看kafka topic详情 
./kafka-topics.sh --zookeeper 192.168.0.170:2182 --topic oceanengine_ad --describe 

# 查看消费者组列表 
./kafka-consumer-groups.sh --bootstrap-server 192.168.0.170:9093,192.168.0.170:9094,192.168.0.170:9095 --list 

# 查看指定消费者组详情 
./kafka-consumer-groups.sh --bootstrap-server 192.168.0.170:9093,192.168.0.170:9094,192.168.0.170:9095 --group mapbridge --describe 

# 创建消费者 
./kafka-console-producer.sh --broker-list 192.168.0.170:9093,192.168.0.170:9094,192.168.0.170:9095 --topic partopic

# 创建生产者 
./kafka-console-consumer.sh --bootstrap-server 192.168.0.170:9093,192.168.0.170:9094,192.168.0.170:9095 --topic partopic --from-beginning 

# 查看主题消息总条数： 
./kafka-run-class.sh kafka.tools.GetOffsetShell --broker-list 192.168.0.170:9093,192.168.0.170:9094,192.168.0.170:9095 --topic partopic --time -1 

# 主题生产消息
 ./kafka-producer-perf-test.sh --topic partopic --throughput -1 --record-size 10 --num-records 500000 --producer-props bootstrap.servers=192.168.0.170:9093,192.168.0.170:9094,192.168.0.170:9095

```

### 常规命令

```sh
docker exec -it kafka1 bash

cd /opt/kafka/bin

./kafka-consumer-groups.sh --bootstrap-server 192.168.0.170:9093,192.168.0.170:9094,192.168.0.170:9095 --group mapbridge --describe
```

### 查看主题消息条数：

./kafka-run-class.sh kafka.tools.GetOffsetShell --broker-list 172.16.95.61:9093,172.16.95.61:9094,172.16.95.61:9095 --topic partopic --time -1


### kafka设置某个topic的数据过期时间

```sh
log.retention.hours=48

log.cleanup.policy=delete

```