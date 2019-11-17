---
title: mac搭建kafka
date: 2019-11-17 13:24:23
tags:
  - kafka
categories:
  - kafka  
---

### 一、安装
```bash 
brew install kafka
```
安装的过程中会自动安装zk
### 二、启动zk
因为我之前单独安装过zk，所以直接启动之前的
```bash
zkServer start
```
### 三、启动kafka
```bash
brew services start kafka
```
### 四、查看所有的topic
```bash
kafka-topics --list --zookeeper localhost:2181
```
### 五、创建一个名为test1的topic
```bash
kafka-topics --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic test1
```
### 六、使用生产一条消息
```bash
kafka-console-producer --broker-list localhost:9092 --topic test1
```
### 七、消费消息
```bash
kafka-console-consumer --bootstrap-server localhost:9092 --topic test1 --from-beginning
```
![](https://dzh213.oss-cn-beijing.aliyuncs.com/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202019-11-17%20%E4%B8%8B%E5%8D%881.39.51.png?Expires=1573975466&OSSAccessKeyId=TMP.hg1pX2dWtuw5bVJno5fRtw7t65y1DRcjtTW16KBT5H2z5sYH6rhPQiBLUaCv1dvdhV66EWWz57HZcBYrzwU8eJCJiHvHDDpAwmUSkhKLQUMfvbrJk1zP7CeUtuejMN.tmp&Signature=q7IGR6xdXvBZu1KNcFz44XWzy6A%3D)