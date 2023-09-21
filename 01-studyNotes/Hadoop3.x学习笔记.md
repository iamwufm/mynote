---
author: wufm
title: Hadoop3.x学习笔记
rating: 0
time: 2023-08-24 周四
tags:
 - Hadoop
 - HDFS
 - MapReduce
 - Yarn
---
##  一、Hadoop入门

### 1.1 概念
Hadoop官网：[Apache Hadoop](https://hadoop.apache.org/)

Hadoop是一个由Apache基本会所开发的==分布式系统基础架构==。主要解决**海量数据的存储和分析计算**问题。从广义来说，Hadoop通常是指一个更广泛的概念-**Hadoop生态圈**。
![[Pasted image 20230824173519.png]]

#### 1.1.1 Hadoop三大发行版本

Hadoop三大发行版本：Apache、Cloudera和Hortonworks。

- **Apache**版本最原始（最基础）的版本，对于入门学习最好。2006
- Cloudera内部集成了很多大数据架构，对应的产品**CDH**。2008
- Hortonworks文档较好，对应产品**HDP**。2011
-  Hortonworks已经被 Cloudera公司收购，推出新的品牌**CDP**。

#### 1.1.2 Hadoop优势

1. 高可靠性：Hadoop底层维护多个数据副本。就是某台机器出现故障，也不会导致数据丢失。
2. 高扩展性：可方便地扩展数以千计地节点
3. 高效性：在MapReduce的思想下，Hadoop是并行工作的，以加快任务处理速度。
4. 高容错性：能够自动将失败的任务重新分配

#### 1.1.3 Hadoop的组成

1. HDFS：分布式文件系统（实现将文件分布式存储在很多的服务器上）
	1. NameNode（nn）：存储文件的**元数据**，如文件名、文件目录结构、文件属性、以及每块文件的**块列表**和**块所在的DataNode**等
	2. DataNode（dn）：在本地文件系统**存储文件块数据和块数据的校验和**
	3. Secondary NameNode（2nn）：**每隔一段时间对NameNode元数据备份**
2. MapReduce：分布式计算框架（实现在很多机器上分布式并行运算）
	1. Map阶段：并行处理输入数据
	2. Reduce阶段：对Map结果进行汇总
3. Yarn：分布式资源管理器（帮用户调度大量的mapreduce程序，并合理分配运算资源）
		1. ResourceManager（RM）：整个集群资源（内存、CPU等）的老大
			1. 处理客户端请求
			2. 监控NodeManager
			3. 启动或监控ApplicationMaster
			4. 资源的分配与调度
		2. NodeManager（NM）：单个节点服务器资源的老大
			1. 管理单个节点的资源
			2. 处理来自ResourceManager的命令
			3. 处理来自ApplicationMaster的命令
		3. ApplicationMaster（AM）：单个任务运行的老大
			1. 为应用程序申请资源并分配内部任务
			2. 任务的监控与容错
		4. Container：容器，相当于一台独立的服务器，里面封装了任务运行所需要的资源，如内存、CPU、磁盘、网络等
![[Pasted image 20230824184844.png]]
### 1.2 Hadoop安装

hadoop运行模式：
1. 本地模式：单机运行，文件存储在本地磁盘。生产环境不可用。
2. 伪分布模式：单机运行，文件存储在HDFS。一台服务器模拟一个分布式环境。生成环境不用。
3. **完全分布式模式**：多台服务器组成分布式环境。**生产环境适用。**

==决定以哪种模式运行的关键点是：==
参数（local是本地模式，yarn是分布模式） **mapreduce.framework.name = yarn | local**
同时，如果要运行在yarn上，以下两个参数也需要配置
参数 yarn.resourcemanager.hostname = ....
参数 fs.defaultFS = ....


本次安装按照完全分布式模式进行安装的。
[[Hadoop3.x集群安装]]]
## 二、 HDFS

参见[[HDFS学习]]

## 三、MapReduce

参见[[MapReduce学习]]
## 四、Yarn

参见[[Yarn学习]]

## 五、Hadoop生产调优手册

参见[[Hadoop生产调优手册]]

## 六、Hadoop源码解析


