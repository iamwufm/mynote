---
author: wufm
title: Hbase2.x安装
rating: 4
time: 2023-10-12 周四
tags:
  - Hbase
代码:
---
## 一、集群的组成

HBASE是一个分布式系统
- 其中有一个管理角色：  HMaster(一般2台，一台active，一台backup)
- 其他的数据节点角色：  HRegionServer(很多台，看数据容量)

- 首先，要有一个HDFS集群，并正常运行； ==regionserver应该跟hdfs中的datanode在一起==
- 其次，还需要一个zookeeper集群，并正常运行
- 然后，安装HBASE

| hadoop101     | hadoop102     | hadoop103     |
| ------------- | ------------- | ------------- |
| HMaster       | HMaster       |               |
| region server | region server | region server |
## 二、环境准备

### 2.1 安装hdfs集群

见[[Hadoop3.x集群安装]]
### 2.2 安装zookpeeper集群

见[[Zookeeper3.x安装]]
## 三、安装Hbase集群

### 3.1 下载Hbase安装包

安装包从官网网址：[Apache HBase – Apache HBase™ Home](https://hbase.apache.org/)

![[Pasted image 20231012184429.png|275]]

![[Pasted image 20231012184455.png|650]]

![[Pasted image 20231012184524.png|400]]

### 3.2 安装Hbase

在hadoop101机器上进行

```shell
# 1.把安装包放在/opt/software/目录下
cd /opt/software/
rz

# 2.解压hbase到/opt/module 目录下
tar -zxf hbase-2.5.5-bin.tar.gz -C /opt/module/

# 3.配置Hadoop环境变量
sudo vim /etc/profile.d/my_env.sh

# 添加如下内容
#HBASE_HOME
export HBASE_HOME=/opt/module/hbase-2.5.5
export PATH=$PATH:$HBASE_HOME/bin

# 4.让环境变量生效
source /etc/profile
```
### 3.3 修改Hbase配置文件

```shell
cd /opt/module/hbase-2.5.5/conf
```

```shell
# 修改hbase-env.sh
vim hbase-env.sh

# 在文件最后添加如下信息,也可以找到这些行注释，然后修改
export JAVA_HOME=/opt/module/jdk1.8.0_144
# false意思是用自己搭建的zookeeper集群
export HBASE_MANAGES_ZK=false
export HBASE_DISABLE_HADOOP_CLASSPATH_LOOKUP="true" # 不写，会导致HBase的jar包和Hadoop的jar包有冲突，服务起不来
```

```shell
# 1.修改hbase-site，删除configuration内容，添加如下内容
vim hbase-site.xml
```

```xml
<configuration>
<!-- 指定hbase在HDFS上存储的路径 -->
<property>
	<name>hbase.rootdir</name>
	<value>hdfs://hadoop101:8020/hbase</value>
</property>

<!-- 指定hbase是分布式的 -->
<property>
	<name>hbase.cluster.distributed</name>
	<value>true</value>
</property>

<!-- 指定zk的地址，多个用“,”分割 -->
<property>
	<name>hbase.zookeeper.quorum</name>
	<value>hadoop101,hadoop102,hadoop103:2181</value>
</property>

 <property>
    <name>hbase.unsafe.stream.capability.enforce</name>
    <value>false</value>
  </property>
</configuration>
```

```shell
vim regionservers

# 添加如下内容，一键启动，需要遍历启动region server
hadoop101
hadoop102
hadoop103
```

（可选）配置高可用（也可以通过命令在其他机器启动hmaster）
```shell
touch backup-masters
echo hadoop102 > backup-masters
```

### 3.4 分发脚本

```shell
cd /opt/module
# 在hadoop101上把内容复制给hadoop102和hadoop103

# 1.同步hadoop
# 也可以写成scp -r /opt/module/hadoop-3.3.6  hadoop03:/opt/module
scp -r hbase-2.5.5 hadoop102:$PWD
scp -r hbase-2.5.5 hadoop103:$PWD
```

去hadoop102和hadoop103机器上添加hbase的环境变量

```shell
sudo vim /etc/profile.d/my_env.sh

# 添加如下内容
#HBASE_HOME
export HBASE_HOME=/opt/module/hbase-2.5.5
export PATH=$PATH:$HBASE_HOME/bin

# 4.让环境变量生效
source /etc/profile
```
## 四、启动Hbase集群

（1）先启动zookpeeper集群

```shell
# 在hadoop101上启动编写好得一键启动脚本
# 1.启动单个QuorumPeerMain（半数以上启动才能正常工作）
myzk.sh start

# 2.关闭QuorumPeerMain
# myzk.sh stop
```

（2）启动hdfs集群

```shell
# 在hadoop101上执行
# 开启hdfs集群
start-dfs.sh 

# 关闭hdfs集群
# stop-dfs.sh 
```

（3）启动hbase集群

```shell
# 在hadoop101-103任一机器都可以
# 启动hbase集群
start-hbase.sh

# 还可以在集群中找任意一台机器启动一个备用的master,新启的这个master会处于backup状态
# 也可以通过配置文件启动，如上面文件的配置
hbase-daemon.sh start master

# 关闭hbase集群
# stop-hbase.sh
```

也可以单点启动

```shell
hbase-daemon.sh start master
hbase-daemon.sh start regionserver

hbase-daemon.sh stop master
hbase-daemon.sh stop regionserver
```

（4）网页访问：hadoop101:16010

```shell
# 1.在hmaster所在机器查看hmaster进程id(jps可以监听所有的java程序)
jps

# 2.通过进程id查找hmaster监控的端口,如 13246
netstat -ntlp|grep 进程id
# 3.浏览访问hmaster提供的web:
hadoop101:端口号
```

![[Pasted image 20231013091250.png|550]]

![[Pasted image 20231013092130.png|550]]
## 五、测试Hbase集群

打开客户端

```shell
hbase shell
```

```shell
# 查看表
list

# 查看集群状态
status

# 查看集群版本
version

# 查看帮助命令，能够展示 HBase 中所有能使用的命令，主要使用的命令有 namespace 命令空间相关，DDL 创建修改表格，DML 写入读取数据。
help
```



