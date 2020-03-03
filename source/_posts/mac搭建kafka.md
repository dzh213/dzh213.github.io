---
title: mac搭建kafka
date: 2019-11-17 13:24:23
tags:
  - kafka
categories:
  - kafka  
---

### 安装
```bash 
brew install kafka
```
安装的过程中会自动安装zk
### 启动zk
因为我之前单独安装过zk，所以直接启动之前的
```bash
zkServer start
```
### 启动kafka
```bash
brew services start kafka
```
### 查看所有的topic
```bash
kafka-topics --list --zookeeper localhost:2181
```
### 创建一个名为test1的topic
```bash
kafka-topics --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic test1
```
### 使用生产一条消息
```bash
kafka-console-producer --broker-list localhost:9092 --topic test1
```
### 消费消息
```bash
kafka-console-consumer --bootstrap-server localhost:9092 --topic test1 --from-beginning
```
![](https://dzh213.oss-cn-beijing.aliyuncs.com/blog/kafka的生产和消费.png)