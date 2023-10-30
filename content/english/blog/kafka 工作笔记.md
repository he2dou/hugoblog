---
title: kafka 工作笔记
date: 2022-04-04T05:00:00Z
image: /images/image-placeholder.png
categories:
  - 技术
author: John Doe
tags:
  - kafka
draft: false
---
<!--more-->

{{< toc >}}

```shell
version: '3' services: zookeeper: image: zookeeper:3.8.0 container_name: zookeeper ports: - "2181:2181" restart: always kafka1: image: wurstmeister/kafka container_name: kafka1 ports: - "9093:9092" depends_on: - zookeeper environment: KAFKA_BROKER_ID: 1 KAFKA_NUM_PARTITIONS: 3 KAFKA_DEFAULT_REPLICATION_FACTOR: 2 KAFKA_ZOOKEEPER_CONNECT: zookeeper KAFKA_LISTENERS: PLAINTEXT://0.0.0.0:9092 KAFKA_ADVERTISED_LISTENERS: PLAINTEXT://172.16.95.61:9093 KAFKA_LOG_RETENTION_HOURS: 12 KAFKA_LOG_CLEANUP_POLICY: delete KAFKA_LOG_RETENTION_CHECK_INTERVAL_MS: 300000 KAFKA_LOG_SEGMENT_BYTES: 1073741824 volumes: - /alidata/kafka/data1:/kafka - /var/run/kafka1.sock:/var/run/docker.sock restart: always kafka2: image: wurstmeister/kafka container_name: kafka2 ports: - "9094:9092" depends_on: - zookeeper environment: KAFKA_BROKER_ID: 2 KAFKA_NUM_PARTITIONS: 3 KAFKA_DEFAULT_REPLICATION_FACTOR: 2 KAFKA_ZOOKEEPER_CONNECT: zookeeper KAFKA_LISTENERS: PLAINTEXT://0.0.0.0:9092 KAFKA_ADVERTISED_LISTENERS: PLAINTEXT://172.16.95.61:9094 KAFKA_LOG_RETENTION_HOURS: 12 KAFKA_LOG_CLEANUP_POLICY: delete KAFKA_LOG_RETENTION_CHECK_INTERVAL_MS: 300000 KAFKA_LOG_SEGMENT_BYTES: 1073741824 volumes: - /alidata/kafka/data2:/kafka - /var/run/kafka2.sock:/var/run/docker.sock restart: always kafka3: image: wurstmeister/kafka container_name: kafka3 ports: - "9095:9092" depends_on: - zookeeper environment: KAFKA_BROKER_ID: 3 KAFKA_NUM_PARTITIONS: 3 KAFKA_DEFAULT_REPLICATION_FACTOR: 2 KAFKA_ZOOKEEPER_CONNECT: zookeeper KAFKA_LISTENERS: PLAINTEXT://0.0.0.0:9092 KAFKA_ADVERTISED_LISTENERS: PLAINTEXT://172.16.95.61:9095 KAFKA_LOG_RETENTION_HOURS: 12 KAFKA_LOG_CLEANUP_POLICY: delete KAFKA_LOG_RETENTION_CHECK_INTERVAL_MS: 300000 KAFKA_LOG_SEGMENT_BYTES: 1073741824 volumes: - /alidata/kafka/data3:/kafka - /var/run/kafka3.sock:/var/run/docker.sock restart: always kafka-manager: image: sheepkiller/kafka-manager:latest container_name: kafka-manager ports: - "9000:9000" # network_mode: host depends_on: - kafka1 - kafka2 - kafka3 - zookeeper environment: ZK_HOSTS: zookeeper:2181 APPLICATION_SECRET: letmein KM_ARGS: -Djava.net.preferIPv4Stack=true
```

内网：

# 创建主题 ./kafka-topics.sh --create --zookeeper 192.168.0.170:2182 --replication-factor 2 --partitions 3 --topic partopic # 查看kafka topic列表 ./kafka-topics.sh --list --zookeeper 192.168.0.170:2182 ./kafka-topics.sh --delete --zookeeper 192.168.0.170:2182 --topic partopic # 查看kafka topic详情 ./kafka-topics.sh --zookeeper 192.168.0.170:2182 --topic oceanengine_ad --describe # 查看消费者组列表 ./kafka-consumer-groups.sh --bootstrap-server 192.168.0.170:9093,192.168.0.170:9094,192.168.0.170:9095 --list # 查看指定消费者组详情 ./kafka-consumer-groups.sh --bootstrap-server 192.168.0.170:9093,192.168.0.170:9094,192.168.0.170:9095 --group mapbridge --describe # 创建消费者 ./kafka-console-producer.sh --broker-list 192.168.0.170:9093,192.168.0.170:9094,192.168.0.170:9095 --topic partopic # 创建生产者 ./kafka-console-consumer.sh --bootstrap-server 192.168.0.170:9093,192.168.0.170:9094,192.168.0.170:9095 --topic partopic --from-beginning # 查看主题消息总条数： ./kafka-run-class.sh kafka.tools.GetOffsetShell --broker-list 192.168.0.170:9093,192.168.0.170:9094,192.168.0.170:9095 --topic partopic --time -1 # 主题生产消息 ./kafka-producer-perf-test.sh --topic partopic --throughput -1 --record-size 10 --num-records 500000 --producer-props bootstrap.servers=192.168.0.170:9093,192.168.0.170:9094,192.168.0.170:9095

docker exec -it kafka1 bash

cd /opt/kafka/bin

./kafka-consumer-groups.sh --bootstrap-server 192.168.0.170:9093,192.168.0.170:9094,192.168.0.170:9095 --group mapbridge --describe

外网：

./kafka-topics.sh --create --zookeeper 172.16.95.61:2181 --replication-factor 2 --partitions 3 --topic partopic ./kafka-topics.sh --delete --zookeeper 172.16.95.61:2181 --topic partopic ./kafka-topics.sh --list --zookeeper 172.16.95.61:2181 # 查看kafka topic详情 ./kafka-topics.sh --zookeeper 172.16.95.61:2181 --topic partopic --describe # 查看消费者组列表 ./kafka-consumer-groups.sh --bootstrap-server 172.16.95.61:9093,172.16.95.61:9094,172.16.95.61:9095 --list # 查看指定消费者组详情 ./kafka-consumer-groups.sh --bootstrap-server 172.16.95.61:9093,172.16.95.61:9094,172.16.95.61:9095 --group mapbridge --describe ./kafka-console-producer.sh --broker-list 172.16.95.61:9093,172.16.95.61:9094,172.16.95.61:9095 --topic partopic ./kafka-console-consumer.sh --bootstrap-server 172.16.95.61:9093,172.16.95.61:9094,172.16.95.61:9095 --topic partopic --from-beginning

线上查看消费情况

docker exec -it kafka1 bash cd /opt/kafka/bin ./kafka-consumer-groups.sh --bootstrap-server 10.3.0.34:9093,10.3.0.34:9094,10.3.0.34:9095 --group mapbridge --describe

测试查看消费清空

docker exec -it kafka1 bash cd /opt/kafka/bin ./kafka-consumer-groups.sh --bootstrap-server 192.168.0.170:9093,192.168.0.170:9094,192.168.0.170:9095 --group mapbridge --describe

[https://www.jianshu.com/p/f21e9b3b0ff6](https://www.jianshu.com/p/f21e9b3b0ff6)

./kafka-topics.sh --create --zookeeper 172.16.95.61:2182 --replication-factor 2 --partitions 3 --topic partopic

./kafka-console-consumer.sh --bootstrap-server 172.16.95.61:9093,172.16.95.61:9094,172.16.95.61:9095 --topic oceanengine_creative --from-beginning

查看主题消息条数：

./kafka-run-class.sh kafka.tools.GetOffsetShell --broker-list 172.16.95.61:9093,172.16.95.61:9094,172.16.95.61:9095 --topic oceanengine_creative --time -1

./kafka-run-class.sh kafka.tools.GetOffsetShell --broker-list 172.16.95.61:9093,172.16.95.61:9094,172.16.95.61:9095 --topic oceanengine_creative_cost --time -1

./kafka-run-class.sh kafka.tools.GetOffsetShell --broker-list 172.16.95.61:9093,172.16.95.61:9094,172.16.95.61:9095 --topic partopic --time -1

主题生产消息

./kafka-producer-perf-test.sh --topic partopic --throughput -1 --record-size 10 --num-records 500000 --producer-props bootstrap.servers=172.16.95.61:9093,172.16.95.61:9094,172.16.95.61:9095

kafka设置某个topic的数据过期时间

log.retention.hours=48

log.cleanup.policy=delete