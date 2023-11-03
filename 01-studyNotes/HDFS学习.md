---
author: wufm
title: HDFS学习
rating: 8
time: 2023-08-30 周三
tags:
  - HDFS
  - Hadoop
  - 分布式文件系统
代码: bigdata项目的下的hadoop-hdfs
---
## 一、HDFS概述

### 1.1 HDFS产生背景及定义

#### 1.1.1 HDFS产生背景

随着数据量越来越大，在一个操作系统存不下所有的数据，那么就分配到更多的操作系统的磁盘中，但是不方便管理和维护，迫切**需要一种系统来管理多台机器上的文件，这就是分布式文件管理系统。HDFS只是分布式文件管理系统的一种。**
#### 1.1.1 HDFS定义

**HDFS是分布式文件管理系统**。用于存储文件，通过目录树来定位文件，由很多服务器联合起来实现其功能，集群中的服务器有各自的角色。

**HDFS的使用场景：适合一次写入，多次读出的场景。**
### 1.2 HDFS优缺点

#### 1.2.1 HDFS优点

1. 高容错性
	- 数据自动保存多个副本。
	- 某一个副本丢失以后，它可以自动恢复
2. 适合处理大数据
	- 数据规模：能够处理数据规模达到GB、TB甚至PB级别的数据
	- 文件规模：能够处理百万规模以上的文件数据，数据相当之大
3. 可构建在廉价机器上，通过多副本机制，提高可靠性
#### 1.2.1 HDFS缺点

1. 不适合做低延时数据访问，比如毫秒级的存储数据
2. 无法高效的对大量小文件进行存储
	- 存储大量小文件的话，它会占用NameNode大量内存来存储文件目录和块信息
	- 小文件存储的寻址时间会超过读取时间，它违反了HDFS的设计目标
3. 不支持并发写入、文件随机修改
	- 一个文件只能有一个写，不允许多个线程同时写
	- 仅支持数据追加，不支持文件的随机修改
### 1.3 HDFS工作机制

![[Pasted image 20230830160500.png|400]]

1、客户把一个文件存入hdfs，其实hdfs会把这个文件切块后，分散存储在N台linux机器系统中（==负责存储文件块的角色DataNode==）<准确来说：**切块的行为是由客户端决定的**>

2、一旦文件被切块存储，那么，hdfs中就必须有一个机制，来记录用户的每一个文件的切块信息，及每一块的具体存储机器（==负责记录块信息的角色是：NameNode==）

3、为了保证数据的安全性，hdfs可以将每一个文件块在集群中存放多个副本（**到底存几个副本，是由当时存入该文件的客户端指定的**）

==综述：一个HDFS系统，由一台运行的NameNode服务器，和N台运行的DataNode服务器组成！==
### 1.4 HDFS文件块大小

==**HDFS块大小设置取决于磁盘传输速率**==

（1）HDFS的块设置太小，会增加寻址时间，程序一直在找块的开始位置
（2）HDFS的块设置太大，从磁盘传输数据的时间会明显大于定位这个块开始位置所需时间。导致程序在处理这块数据时，会非常慢

HDFS的文件在物理上是分块存储，块大小可以通过配置参数来规定，默认大小在Hadoop2.x/3.x版本中是128M，1.x版本是64M。

普通磁盘的传输速率普遍为100M/s，固态硬盘大概是200-300M/s。

![[Pasted image 20230830161920.png|400]]
## 二、HDFS客户端操作

操作HDFS有多种形式：
1. 网页形式
2. 命令行形式
3. API形式

==客户端在哪里运行，没有约束，只要运行客户端的机器能够跟hdfs集群联网==

文件的切块大小和存储的副本数量，都是由客户端决定！所谓的由客户端决定，是通过配置参数来定的。参考[[Hadoop3.x集群安装#3.2.1 hdfs集群配置文件]]如，读取hdfs-default.xml的块大小（dfs.blocksize）和副本数（dfs.replication）配置
### 2.1 HDFS客户端命令行操作（shell）（重点）

基本语法：`hadoop fs 具体命令` 或者 `hdfs dfs 具体命令`

```shell
# 可以通过以下命令来查看所有的具体命令
hadoop fs
# 或者
hdfs dfs

# 查看具体某个命令
hadoop fs -help rm
```

```shell
# 准备数据测试

# 进入某个目录
cd /home/hadoop/test

vim weiguo.txt

# 添加如下内容
caocao
caozhi
caopi

vim .txt
```

```shell
# 1.创建文件夹 （加上-p 可以创建多层目录）
hadoop fs -mkdir /sanguo

# 2.上传（hadoop fs -put 本地目录 hdfs目录） 等价于copyFromLocal
hadoop fs -put /home/hadoop/test/weiguo.txt /sanguo

# 3.下载（hadoop fs -get hdfs目录 本地目录） 等价于copyToLocal
hadoop fs -get /sanguo/weiguo.txt /home/hadoop

# 4.查看
# 查看目录信息
hadoop fs -ls /sanguo
查看文件信息
hadoop fs -cat /sanguo/weiguo.txt
hadoop fs -tail /sanguo/weiguo.txt
hadoop fs -head /sanguo/weiguo.txt

# 文件的更名或移动（hadoop fs -mv /hdfs的路径  /hdfs的另一个路径）
hadoop fs -mv /sanguo/weiguo.txt /
hadoop fs -mv /weiguo.txt /sanguo/weiguo2.txt

# 5.删除
# 递归删除目录及目录的内容
hadoop fs -rm -r /sanguo
```
### 2.2 HDFS客户端API操作（java）

在window上安装hadoop客户端。具体操作参见[[Windows安装hadoop]]

（1）增删查改操作：com.bigdata.hdfs.HdfsClientTestMian

（2）把日志上传到hdfs：com.bigdata.datacollect.DataCollectMain

模拟产生日志的代码在web-log模块下的com.bigdata.CollectLogMain

```txt
在业务系统的服务器上，业务程序会不断生成业务日志（比如网站的页面访问日志）
业务日志使用log4j生成的，会不断地切出日志文件

需求：
需要定期（比如每小时）从业务服务器上地日志目录中，探测需要采集地日志文件（access.log不能采），发往HDFS
当天采集到日志要放在hdfs地当天目录中
采集完成地日志文件，需要移动到日志服务器一个备份目录中
定期检查（比如每小时）备份目录，将备份时长超过24小时地日志文件清除

注意点：业务服务器可能有多台（hdfs上地文件名不能直接用日志服务器上地文件名）
```

（3）统计单词次数：com.bigdata.wordcount.HdfsWordcountMain

```txt
单词统计

统计某个目录下的所有文件内容的单词出现次数
文件内容以空格切分

把统计结果放在hdfs某目录下
```
## 三、HDFS的核心工作原理

### 3.1 NameNode元数据管理机制

![[NameNode元数据管理机制.excalidraw|600]]
#### 3.1.1 Fsimage和Edits概念

![[Pasted image 20230904113203.png]]

```txt
1. 每次NameNode启动时都会把Fsimage文件读入内存，加载Edits里面的更新操作，保证内存中的元数据时最新的、同步的
2. Fsimage文件：HDFS文件系统元数据的一个永久性的检查点，其中包含HDFS文件系统的所有目录和文件inode的序列化信息
3. Edits文件：存放HDFS文件系统的所有更新操作（增删改）
4. seen_txid文件：保存最后一个idits_的数字
```

```shell
# 查看Fsimage文件
# hdfs oiv -p 文件类型 -i 镜像文件 -o 转换后文件输出路径
hdfs oiv -p XML -i fsimage_0000000000000000599 -o ./fsimage.xml

# 查看 Edits 文件
hdfs oev -p 文件类型 -i 编辑日志 -o 转换后文件输出路径
```
#### 3.1.2 NameNode和SecondaryNameNode

思考：NameNode中的元数据是存储在哪里的？

首先，我们做个假设，如果存储在NameNode节点的磁盘中，因为经常需要进行随机访问，还有响应客户请求，必然效率过低。因此，**元数据需要存放在内存中**。

但如果只存在内存中，一旦断电，元数据丢失，整个集群无法工作，因此产生在**磁盘备份元数据的Fsimage**。
这样又会带来新的问题，当在内存的元数据更新时，如果同时更新Fsimage，就会导致效率过低，如果不更新，就会发生一致性问题，一旦NameNode节点断电，就会产生数据丢失。因此，**引入Edits文件（只进行追加操作，效率很高）**。

**每当元数据有更新或者添加元数据时，修改内存中的元数据并追加到Edits中**，一旦NameNode节点断电，可以通过Fsimage和Edits的合并，合成元数据。

但是如果长时间添加数据到Edits中，会导致该文件数据过大，效率降低，而且一旦断电，恢复元数据需要的时间过长。因此需要定期进行Fsimage和Edits的合并，如果这个操作由NameNode节点完成，又会效率过低。因此，**引入一个新的节点SecondaryNameNode，专门用于Fsimage和Edits的合并**。
#### 3.1.3 checkpoit时间点

1. 通常情况下，SecondaryNameNode每隔一个小时执行一次
2. 一分钟检查一次操作次数，当操作次数达到一百万时，SecondaryNameNode执行一次

[hdfs-default.xml]

```xml
<!--每隔一个小时执行一次-->
<property>
<name>dfs.namenode.checkpoint.period</name>
<value>3600s</value>
</property>

<!--操作动作次数-->
<property>
<name>dfs.namenode.checkpoint.txns</name>
<value>1000000</value>
</property>

<!--1 分钟检查一次操作次数-->
<property>
<name>dfs.namenode.checkpoint.check.period</name>
<value>60s</value>
</property>
```

### 3.2 客户端写数据到HDFS的流程

![[客户端写数据到HDFS的流程.excalidraw|600]]

副本存储节点选择：
- 第一个副本在客户端所处的节点上。==如果客户端在集群外，随机选一个==
- 第二个副本在另一个机架的随机一个节点
- 第三个副本在第二个副本所在的机架的随机节点

副本数和切块大小可在客户端上操作。

hdfs-default.xml

```xml
<!--副本数-->
<property>
<name>dfs.replication</name>
<value>4</value>
</property>

<!--切块大小-->
<property>
<name>dfs.blocksize</name>
<value>16m</value>
</property>
```
### 3.3 客户端从HDFS中读数据的流程

![[客户端从HDFS中读数据的流程.excalidraw|600]]

### 3.4 DataNode的工作机制

![[DataNode的工作机制.excalidraw|600]]

1. datanode节点不可用：
	- 超过10分钟+30秒没有收到datanode的心跳，则认为该节点不可用。如果定义超时时间为TimeOut，则超时时长的计算公式为`TimeOut = 2 * dfs.namenode.heartbeat.recheck-interval + 10 * dfs.heartbeat.interval`。而默认的dfs.namenode.heartbeat.recheck-interval 大小为5分钟，dfs.heartbeat.interval默认为3秒。

2. DataNode 节点保证数据完整性的方法
	- 当 DataNode 读取 Block 的时候，它会计算 CheckSum。如果计算后的 CheckSum，与 Block 创建时值不一样，说明 Block 已经损坏。Client 读取其他 DataNode 上的 Block。
	- 常见的校验算法crc（32），md5（128），sha1（160）
	- DataNode 在其文件创建后周期验证 CheckSum。

hdfs-default.xml

```xml
<!--上报块信息时间间隔，默认6小时（毫秒单位）-->
<property>
<name>dfs.blockreport.intervalMsec</name>
<value>21600000</value>
</property>

<!--DN 扫描自己节点块信息列表的时间，默认6小时-->
<property>
<name>dfs.datanode.directoryscan.interval</name>
<value>21600s</value>
</property>

<!--namenode心跳检查间隔5分钟（毫秒）-->
<property>
<name>dfs.namenode.heartbeat.recheck-interval</name>
<value>300000</value>
</property>

<!--心跳间隔3秒（秒）-->
<property>
<name>dfs.heartbeat.interval</name>
<value>3</value>
</property>
```
