---
author: wufm
title: Flume学习
rating: 2
time: 2023-10-16 周一
tags:
  - Flume
  - 数据采集
代码:
---
## 一、Flume工作机制

![[Flume工作机制示意图.excalidraw|600]]



## 二、Flume安装

解压安装包即可

```shell
# 在hadoop103上执行
# 1.把安装包放在/opt/software/目录下
cd /opt/software/
rz

# 2.解压hadoop到/opt/module 目录下
tar -zxf apache-flume-1.11.0-bin.tar.gz -C /opt/module/

cd /opt/module/ 

# 重命名
mv apache-flume-1.11.0-bin flume-1.11.0

cd /opt/module/hadoop-3.3.6/share/hadoop/common/lib
cp guava-27.0-jre.jar /opt/module/flume-1.11.0/lib/
```

启动采集命令

```shell
cd /opt/module/flume-1.11.0/
# 前端启动，方便观察日志
bin/flume-ng agent --conf conf -conf-file /usr/flume/conf/master.conf  -n a1  -Dflume.root.logger=INFO,console
```

方案编写可以参考：[Flume 1.11.0 User Guide — Apache Flume](https://flume.apache.org/releases/content/1.11.0/FlumeUserGuide.html)

## 二、读目录新文件

```shell
# 启动hdfs集群（在hadoop101上执行）
start-dfs.sh

# 在hadoop103上执行
cd /opt/module/flume-1.11.0/
mkdir job
cd job
vim dir-hdfs.conf

# 添加如下内容
```

```conf
#定义三大组件的名称
ag1.sources = source1
ag1.sinks = sink1
ag1.channels = channel1

# 配置source组件
ag1.sources.source1.type = spooldir
ag1.sources.source1.spoolDir = /home/hadoop/log/
ag1.sources.source1.fileSuffix=.FINISHED
# 行的最大长度，如果超过该值会进行截取5k
ag1.sources.source1.deserializer.maxLineLength=5120

# 配置sink组件
ag1.sinks.sink1.type = hdfs
ag1.sinks.sink1.hdfs.path =hdfs://hadoop101:8020/access_log/%y-%m-%d/%H-%M
ag1.sinks.sink1.hdfs.filePrefix = app_log
ag1.sinks.sink1.hdfs.fileSuffix = .log
# #积攒多少个 Event 才 flush 到 HDFS 一次
ag1.sinks.sink1.hdfs.batchSize= 100
ag1.sinks.sink1.hdfs.fileType = DataStream
ag1.sinks.sink1.hdfs.writeFormat =Text

## roll：滚动切换：控制写文件的切换规则
# 按文件体积（字节）来切 
ag1.sinks.sink1.hdfs.rollSize = 512000
# 按event条数切
ag1.sinks.sink1.hdfs.rollCount = 1000
# 按时间间隔切换文件
ag1.sinks.sink1.hdfs.rollInterval = 60

## 控制生成目录的规则,10分钟生成一次目录
ag1.sinks.sink1.hdfs.round = true
ag1.sinks.sink1.hdfs.roundValue = 10
ag1.sinks.sink1.hdfs.roundUnit = minute

ag1.sinks.sink1.hdfs.useLocalTimeStamp = true

# channel组件配置
ag1.channels.channel1.type = memory
# event条数
ag1.channels.channel1.capacity = 1000
# flume事务控制所需要的缓存容量100条event
ag1.channels.channel1.transactionCapacity = 100

# 绑定source、channel和sink之间的连接
ag1.sources.source1.channels = channel1
ag1.sinks.sink1.channel = channel1
```

```shell
# 开始采集
bin/flume-ng agent --conf conf -conf-file job/dir-hdfs.conf  -n ag1  -Dflume.hadoop.logger=INFO,console

# 往/home/hadoop/log/目录丢文件

```

![[flume读目录中的新文件示意图.excalidraw]]

## 三、读正在写的文件

![[flume读tail-F数据示意图.excalidraw]]


## 四、Flume多级串联

![[flume多级串联示意图.excalidraw|500]]

