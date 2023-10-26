---
author: wufm
title: Hadoop3.x HA 高可用安装
rating: 5
time: 2023-09-26 周二
tags:
  - Hadoop
  - hadoop安装
  - Hadoop高可用
代码:
---
## 一、集群的组成

NameNode和ResourceManager不要安装在同一台机器上，都是容易消耗内存的。

==DataNode和NodeManager应该部署在一起。==


|           | hadoop101 | hadoop102 | hadoop103       | hadoop104       | hadoop105      |
|:--------- |:--------- |:--------- | --------------- | --------------- | -------------- |
| hdfs      | namenode  | namenode  |                 |                 |   namenode             |
| hdfs      | zkfc      | zkfc      |                 |                 |                |
| hdfs      |           |           | JournalNode     | JournalNode     | JournalNode    |
| hdfs      |   datanode        |  datanode         | datanode        | datanode        | datanode       |
| yarn      |           |           | resourceManager | resourceManager |                |
| yarn      |     nodeManager      |     nodeManager      | nodeManager     | nodeManager     | nodeManager    |
| zookeeper |           |           | QuorumPeerMain  | QuorumPeerMain  | QuorumPeerMain |

## 二、环境的准备

### 2.1 准备环境

参见[[Hadoop3.x集群安装#二、环境准备]]

### 2.2 安装zookeeper

参见[[Zookeeper3.x安装]]

安装目录更名为：/opt/module/ha-zookeeper-3.8.2

数据存放路径：/opt/data/ha-zookeeper-3.8.2
### 2.3 ssh配置

一键启动脚本需要：

（1）在hadoop101/hadoop102/hadoop105一键启动hdfs集群，需要hadoop101/hadoop102/hadoop105能免密登陆到hadoop101-105

（2）在hadoop103/hadoop104一键启动yarn集群，需要hadoop103/hadoop104能免密登陆到hadoop101-105。

```shell
# 先后在hadoop101-105执行以下操作

# 在hadoop101上生成公钥和私钥
# 1. 输入后敲（三个回车），就会生成两个文件 id_rsa（私钥）、id_rsa.pub（公钥）
ssh-keygen -t rsa

# 2.将公钥拷贝到要免密登录的目标机器上
ssh-copy-id hadoop101
ssh-copy-id hadoop102
ssh-copy-id hadoop103
ssh-copy-id hadoop104
ssh-copy-id hadoop105

# 3.可以进入相应目录，查看生成的信息
cd /home/hadoop/.ssh
ll
```

## 三、安装高可用hadoop集群

==没特殊说明，以下操作均在hadoop101机器上的hadoop用户下执行。==
### 3.1 安装hadoop

```shell
mkdir /opt/module/ha
# 1.把安装包放在/opt/software/目录下
cd /opt/software/
rz

# 2.解压hadoop到/opt/module/ha-hadoop-3.3.6 目录下
tar -zxvf hadoop-3.3.6.tar.gz -C /opt/module/ha

# 移动目录并更名
cd /opt/module
mv ha/hadoop-3.3.6 ha-hadoop-3.3.6

rm -rf ha
# 3.配置Hadoop环境变量
sudo vim /etc/profile.d/my_env.sh

# 添加如下内容，如果有HADOOP_HOME，记得注释掉
#HADOOP_HOME
export HADOOP_HOME=/opt/module/ha-hadoop-3.3.6
export PATH=$PATH:$HADOOP_HOME/bin
export PATH=$PATH:$HADOOP_HOME/sbin

# 4.让环境变量生效
source /etc/profile

# 5.验证java是否安装成功（如果不显示版本号，可重启虚拟机sudo reboot）
hadoop version
```
### 3.2 修改hadoop配置文件

```shell
# 进入自定义配置文件目录
cd /opt/module/ha-hadoop-3.3.6/etc/hadoop
```
#### 3.2.1 hdfs集群配置文件

```shell
# 1.修改core-site.xml，添加如下内容
vim core-site.xml
```

```xml
<!--在<configuration>标签里新增如下内容-->

<!-- 1.指定hadoop的默认文件系统为：hdfs -->
<property>
	<name>fs.defaultFS</name>
	<value>hdfs://mycluster</value>
</property>

<!-- 指定hadoop数据的存储目录 -->
<property>
	<name>hadoop.tmp.dir</name>
	<value>/opt/data/ha-hadoop-3.3.6</value>
</property>

<!-- 配置HDFS网页登录使用的静态用户为hadoop-->
<property>
	<name>hadoop.http.staticuser.user</name>
	<value>hadoop</value>
</property>

<!-- 指定 zkfc 要连接的 zkServer 地址 -->
<property>
	<name>ha.zookeeper.quorum</name>
	<value>hadoop103:2181,hadoop104:2181,hadoop105:2181</value>
</property>
```

```shell
# 2.修改hdfs-site.xml，添加如下内容
vim hdfs-site.xml
```

```xml
<!--在<configuration>标签里新增如下内容-->

<!-- 完全分布式集群名称 -->
<property>
	<name>dfs.nameservices</name>
	<value>mycluster</value>
</property>

<!-- 集群中 NameNode 节点都有哪些 -->
<property>
	<name>dfs.ha.namenodes.mycluster</name>
	<value>nn1,nn2,nn3</value>
</property>

<!-- NameNode 的 RPC 通信地址 -->
<property>
<name>dfs.namenode.rpc-address.mycluster.nn1</name>
<value>hadoop101:8020</value>
</property>

<property>
	<name>dfs.namenode.rpc-address.mycluster.nn2</name>
	<value>hadoop102:8020</value>
</property>

<property>
	<name>dfs.namenode.rpc-address.mycluster.nn3</name>
	<value>hadoop105:8020</value>
</property>

<!-- NameNode 的 http 通信地址 -->
<property>
	<name>dfs.namenode.http-address.mycluster.nn1</name>
	<value>hadoop101:9870</value>
</property>

<property>
	<name>dfs.namenode.http-address.mycluster.nn2</name>
	<value>hadoop102:9870</value>
</property>

<property>
	<name>dfs.namenode.http-address.mycluster.nn3</name>
	<value>hadoop105:9870</value>
</property>

<!-- 指定 NameNode元数据在JournalNode上的存放位置 -->
<property>
<name>dfs.namenode.shared.edits.dir</name>
<value>qjournal://hadoop103:8485;hadoop104:8485;hadoop105:8485/mycluster</value>
</property>

<!-- 指定JournalNode在本地磁盘存放数据的位置 -->
<property>
	<name>dfs.journalnode.edits.dir</name>
	<value>${hadoop.tmp.dir}/dfs/journaldata</value>
</property>

<!-- JournalNode 数据存储目录 -->
<property>
	<name>dfs.journalnode.edits.dir</name>
	<value>${hadoop.tmp.dir}/dfs/journaldata</value>
</property>

<!-- 开启NameNode失败自动切换 -->
<property>
	<name>dfs.ha.automatic-failover.enabled</name>
	<value>true</value>
</property>

<!-- 访问代理类：client 用于确定哪个 NameNode 为 Active -->
<property>
	<name>dfs.client.failover.proxy.provider.mycluster</name>
	<value>org.apache.hadoop.hdfs.server.namenode.ha.ConfiguredFailoverProxyProvider</value>
</property>

<!-- 配置隔离机制，即同一时刻只能有一台服务器对外响应 -->
<!-- 配置隔离机制方法，多个机制用换行分割，即每个机制暂用一行-->
<property>
	<name>dfs.ha.fencing.methods</name>
	<value>
	sshfence
	shell(/bin/true)
	</value>
</property>

<!-- 配置sshfence隔离机制超时时间30s -->
<property>
	<name>dfs.ha.fencing.ssh.connect-timeout</name>
	<value>30000</value>
</property>

<!-- 使用隔离机制时需要 ssh 秘钥登录-->
<property>
	<name>dfs.ha.fencing.ssh.private-key-files</name>
	<value>/home/hadoop/.ssh/id_rsa</value>
</property>
```

```shell
# 修改workers文件
vim workers

# 添加如下内容（datanode的列表，使用start-dfs.sh等命令会遍历此文件）
hadoop101
hadoop102
hadoop103
hadoop104
hadoop105
```
#### 3.2.2 yarn集群配置文件


```shell
# 修改yarn-site.xml，添加如下内容
vim yarn-site.xml
```

```xml
<!--在<configuration>标签里新增如下内容-->

<!-- 指定 MR 走 shuffle -->
<property>
	<name>yarn.nodemanager.aux-services</name>
	<value>mapreduce_shuffle</value>
</property>

<!-- 开启RM高可用 -->
<property>
	<name>yarn.resourcemanager.ha.enabled</name>
	<value>true</value>
</property>

<!-- 指定RM的cluster id -->
<property>
	<name>yarn.resourcemanager.cluster-id</name>
	<value>cluster-yarn</value>
</property>

<!--指定 resourcemanager 的逻辑列表-->
<property>
	<name>yarn.resourcemanager.ha.rm-ids</name>
	<value>rm1,rm2</value>
</property>

<!-- ========== rm1 的配置 ========== -->
<!-- 指定 rm1 的主机名 -->
<property>
	<name>yarn.resourcemanager.hostname.rm1</name>
	<value>hadoop103</value>
</property>
<!-- 指定 rm1 的 web 端地址 -->
<property>
	<name>yarn.resourcemanager.webapp.address.rm1</name>
	<value>hadoop103:8088</value>
</property>
<!-- 指定 rm1 的内部通信地址 -->
<property>
	<name>yarn.resourcemanager.address.rm1</name>
	<value>hadoop103:8032</value>
</property>
<!-- 指定 AM 向 rm1 申请资源的地址 -->
<property>
	<name>yarn.resourcemanager.scheduler.address.rm1</name>
	<value>hadoop103:8030</value>
</property>
<!-- 指定供 NM 连接的地址 -->
<property>
	<name>yarn.resourcemanager.resource-tracker.address.rm1</name>
	<value>hadoop103:8031</value>
</property>

<!-- ========== rm2 的配置 ========== -->
<!-- 指定 rm1 的主机名 -->
<property>
	<name>yarn.resourcemanager.hostname.rm2</name>
	<value>hadoop104</value>
</property>
<!-- 指定 rm1 的 web 端地址 -->
<property>
	<name>yarn.resourcemanager.webapp.address.rm2</name>
	<value>hadoop104:8088</value>
</property>
<!-- 指定 rm1 的内部通信地址 -->
<property>
	<name>yarn.resourcemanager.address.rm2</name>
	<value>hadoop104:8032</value>
</property>
<!-- 指定 AM 向 rm1 申请资源的地址 -->
<property>
	<name>yarn.resourcemanager.scheduler.address.rm2</name>
	<value>hadoop104:8030</value>
</property>
<!-- 指定供 NM 连接的地址 -->
<property>
	<name>yarn.resourcemanager.resource-tracker.address.rm2</name>
	<value>hadoop104:8031</value>
</property>

<!-- 指定zk集群地址 -->
<property>
	<name>yarn.resourcemanager.zk-address</name>
	<value>hadoop103:2181,hadoop104:2181,hadoop105:2181</value>
</property>

<!-- 启用自动恢复 -->
<property>
	<name>yarn.resourcemanager.recovery.enabled</name>
	<value>true</value>
</property>

<!-- 指定 resourcemanager 的状态信息存储在 zookeeper 集群 -->
<property>
	<name>yarn.resourcemanager.store.class</name>
	<value>org.apache.hadoop.yarn.server.resourcemanager.recovery.ZKRMStateStore</value>
</property>

<!-- 环境变量的继承 -->
<property>
	<name>yarn.nodemanager.env-whitelist</name>
	<value>JAVA_HOME,HADOOP_COMMON_HOME,HADOOP_HDFS_HOME,HADOOP_CONF_DIR,CLASSPATH_PREPEND_DISTCACHE,HADOOP_YARN_HOME,HADOOP_MAPRED_HOME</value>
</property>

<!-- 日志聚集功能好处：可以方便的查看到程序运行详情，方便开发调试。-->
<!-- 开启日志聚集功能 -->
<property>
	<name>yarn.log-aggregation-enable</name>
	<value>true</value>
</property>
<!-- 设置日志聚集服务器地址 -->
<property>
	<name>yarn.log.server.url</name>
	<value>http://hadoop101:19888/jobhistory/logs</value>
</property>
<!-- 设置日志保留时间为 7 天-->
<property>
	<name>yarn.log-aggregation.retain-seconds</name>
	<value>604800</value>
</property>

```

```shell
# 修改workers文件（如果跟datanode列表不一样，分发后去resouceManager的机器修改）
vim workers

# 添加如下内容（nodeManager的列表(一般跟datanode的列表一样)，使用start-yarn.sh等命令会遍历此文件）
hadoop101
hadoop102
hadoop103
hadoop104
hadoop105
```
#### 3.3.3 mapreduce程序配置文件

```shell
# 修改mapred-site.xml，添加如下内容
vim mapred-site.xml
```

```xml
<!-- 指定 MapReduce 程序运行在 Yarn 上 -->
<property>
	<name>mapreduce.framework.name</name>
	<value>yarn</value>
</property>

<!--为了查看程序job的历史运行情况，需要配置一下历史服务器-->
<!-- 历史服务器端地址 -->
<property>
	<name>mapreduce.jobhistory.address</name>
	<value>hadoop101:10020</value>
</property>
<!-- 历史服务器 web 端地址 -->
<property>
	<name>mapreduce.jobhistory.webapp.address</name>
	<value>hadoop101:19888</value>
</property>
```
### 3.3 分发脚本

```shell
cd /opt/module

# 1. 拷贝hadoop。再hadoop101执行
# rsync -av ha-hadoop-3.3.6 hadoop102:$PWD 有修改的才更新，效率更快
scp -r ha-hadoop-3.3.6 hadoop102:$PWD
scp -r ha-hadoop-3.3.6 hadoop103:$PWD
scp -r ha-hadoop-3.3.6 hadoop104:$PWD
scp -r ha-hadoop-3.3.6 hadoop105:$PWD

# 2. 拷贝zookeeper。再hadoop103执行
scp -r ha-zookeeper-3.8.2 hadoop104:$PWD
scp -r ha-zookeeper-3.8.2 hadoop105:$PWD

cd /opt/data
scp -r ha-zookeeper-3.8.2 hadoop104:$PWD
scp -r ha-zookeeper-3.8.2 hadoop105:$PWD

# 再hadoop104和hadoop105上修改myid文件的内容，分别是2和3
cd /opt/data/ha-zookeeper-3.8.2
vim myid

# 3.环境变量
sudo vim /etc/profile.d/my_env.sh

# 确保hadoop101-102的内容如下：
#JAVA_HOME
export JAVA_HOME=/opt/module/jdk1.8.0_144
export PATH=$PATH:$JAVA_HOME/bin

#HADOOP_HOME
export HADOOP_HOME=/opt/module/ha-hadoop-3.3.6
export PATH=$PATH:$HADOOP_HOME/bin
export PATH=$PATH:$HADOOP_HOME/sbin

# 确保hadoop103-105的内容如下：
#JAVA_HOME
export JAVA_HOME=/opt/module/jdk1.8.0_144
export PATH=$PATH:$JAVA_HOME/bin

#HADOOP_HOME
export HADOOP_HOME=/opt/module/ha-hadoop-3.3.6
export PATH=$PATH:$HADOOP_HOME/bin
export PATH=$PATH:$HADOOP_HOME/sbin

# 添加如下内容
#ZOOKEEPER_HOME
export ZOOKEEPER_HOME=/opt/module/ha-zookeeper-3.8.2
export PATH=$PATH:$ZOOKEEPER_HOME/bin
```

```shell
# 在hadoop101-105执行环境变量生效语句
source /etc/profile

# 再次执行 (如果显示路径不对，重启虚拟机)
hadoop version
```
### 3.4 格式化操作

（1）启动zookeeper（分别在hadoop103、hadoop104、hadoop105上执行）

```shell
# 启动服务 zkServer.sh stop
zkServer.sh start

# 可以看到 QuorumPeerMain
jps

# 查看状态，一个leader,两个flower 才算启动成功
zkServer.sh status

```

（2）启动journalnode（分别在hadoop103、hadoop104、hadoop105上执行）

```shell
# hadoop-daemon.sh stop journalnode
hadoop-daemon.sh start journalnode

# 可以看到 JournalNode
jps
```

（3）格式化namenode（在hadoop101上执行）

```shell
# 在hadoop101上执行
hdfs namenode -format

hdfs --daemon start namenode

# 分别在hadooop102和hadoop105上执行。同步hadoop101的namenode元数据信息（/opt/data/ha-hadoop-3.3.6/dfs/name）
hdfs namenode -bootstrapStandby
```

（4）格式化ZKFC（在hadoop101上执行）

```shell
hdfs zkfc -formatZK
```

（5）关闭服务

```shell
# 在hadoop101上执行
hdfs --daemon stop namenode

# 分别在hadoop103、hadoop104、hadoop105上执行
hadoop-daemon.sh stop journalnode

# 分别在hadoop103、hadoop104、hadoop105上执行
zkServer.sh stop
```
## 四、启动集群

### 4.1 启动hdfs集群

（1）启动zookeeper（分别在hadoop103、hadoop104、hadoop105上执行）

```shell
# 启动服务 zkServer.sh stop
zkServer.sh start

# 可以看到 QuorumPeerMain
jps

# 查看状态，一个leader,两个flower 才算启动成功
zkServer.sh status
```

（2）启动hdfs集群（在hadoop101、hadoop102或hadoop105上执行）

```shell
start-dfs.sh

# 关闭，把启动命令的start改为stop
```

从hadoop101-105执行jps信息如下：[[hdfs高可用jps信息]]

（3）查看hdfs哪台机器是active

```shell
# 登陆zookeeper客户端
zkCli.sh

# 查询active的机器
get -s /hadoop-ha/mycluster/ActiveStandbyElectorLock
```

```shell
# 查看是否 Active
hdfs haadmin -getServiceState nn1
hdfs haadmin -getServiceState nn2
hdfs haadmin -getServiceState nn3
```

（4）web端查看

```txt
http://hadoop101:9870
http://hadoop102:9870
http://hadoop103:9870
```

### 4.2 测试hdfs集群是否高可用

（1）kill掉active的namenode

```shell
# 查询到hadoop105是active的namenode。在hadoop105上删掉namenode进程
kill -9 3404
```

查看web端：

![[Pasted image 20230927163410.png|400]]

![[Pasted image 20230927163430.png|400]]


（2）也可以把直接把active的namenode的机器直接关机

把hadoop102直接关机

查看web端：

![[Pasted image 20230927164457.png|400]]

（3）启动集群。hadoop102开机，在hadoop101、hadoop102或hadoop105执行

```shell
start-dfs.sh
```

### 4.3 启动yarn集群

（1）启动zookeeper（分别在hadoop103、hadoop104、hadoop105上执行）

```shell
# 启动服务 zkServer.sh stop
zkServer.sh start

# 可以看到 QuorumPeerMain
jps

# 查看状态，一个leader,两个flower 才算启动成功
zkServer.sh status
```

（2）启动yarn集群（在hadoop103或hadoop104上执行）

```shell
# 一键启动yarn（在hadoop103或hadoop104上执行。会根据workers文件依次启动nodeManagers）
start-yarn.sh

# 关闭，把启动命令的start改为stop
```

（3）查看yarn哪台机器是active

```shell
# 登陆zookeeper客户端
zkCli.sh

# 查询active的机器
get -s /yarn-leader-election/cluster-yarn/ActiveStandbyElectorLock
```

```shell
# 查看是否 Active
yarn rmadmin -getServiceState rm1
yarn rmadmin -getServiceState rm2
```

（4）web端查看

```txt
http://hadoop103:8088
http://hadoop104:8088
```

### 4.4 测试yarn集群高可用

kill掉active的ResourceManager

```shell
# 查询到hadoop104是active的ResourceManager。在hadoop104上删掉ResourceManager进程
kill -9 16858
```

查看web端：

![[Pasted image 20230927174026.png|400]]

### 4.5 启动历史服务器

主要是通过yarn的网页查看某个job的日志（Tracking URL：History）。

```shell
# 在 hadoop101 启动历史服务器 mapred --daemon stop historyserver
mapred --daemon start historyserver

# 关闭，把启动命令的start改为stop
```

web端查看：

```txt
http://hadoop101:19888
```
## 五、测试集群

参见[[Hadoop3.x集群安装#五、测试集群]]

## 六、报错信息分析

### 6.1 hdfs启动报错

```txt
org.apache.hadoop.ipc.Client: Retrying connectto server: hadoop103/192.168.6.103:8485. Already tried 9 time(s); retry
policy is RetryUpToMaximumCountWithFixedSleep(maxRetries=10,sleepTime=1000 MILLISECONDS)
2020-08-17 10:11:49,678 WARN org.apache.hadoop.hdfs.server.namenode.FSEditLog: Unable to determine input streams from QJM to [192.168.6.102:8485, 192.168.6.103:8485,192.168.6.104:8485]. Skipping.
org.apache.hadoop.hdfs.qjournal.client.QuorumException: Got too many exceptions to achieve quorum size 2/3. 3 exceptions thrown:
192.168.6.103:8485: Call From hadoop102/192.168.6.102 to hadoop103:8485 failed on connection exception: java.net.ConnectException: 拒绝连接; For more details see: http://wiki.apache.org/hadoop/ConnectionRefused
```

查看报错日志，可==分析出报错原因是因为 NameNode 连接不上 JournalNode==，而利用 jps 命令查看到三台 JN 都已经正常启动，为什么 namenode 还是无法正常连接到 JournalNode 呢？这是因为 start-dfs.sh 群起脚本默认的启动顺序是先启动 namenode，再启动datanode，然后再启动 JournalNode，并且默认的 rpc 连接参数是重试次数为 10，每次重试的间隔是 1s，也就是说启动完 namenode以后的 10s 中内，JournalNode 还启动不起来，namenode 就会报错了。

解决方案：遇到上述问题后，可以稍等片刻，等 JournalNode 成功启动后，手动启动下三台namenode：

```shell
hdfs --daemon start namenode
```

也可以在 core-site.xml 里面适当调大上面的两个参数：

```xml
<!-- NN 连接 JN 重试次数，默认是 10 次 -->
<property>
	<name>ipc.client.connect.max.retries</name>
	<value>20</value>
</property>

<!-- 重试时间间隔，默认 1s -->
<property>
	<name>ipc.client.connect.retry.interval</name>
	<value>5000</value>
</property>
```

### 6.1 workcount报错

```shell
hadoop jar /opt/module/ha-hadoop-3.3.6/share/hadoop/mapreduce/hadoop-mapreduce-examples-3.3.6.jar wordcount /input /output
```

```txt
Exit code: 1

[2023-09-27 18:00:30.594]Container exited with a non-zero exit code 1. Error file: prelaunch.err.
Last 4096 bytes of prelaunch.err :
Last 4096 bytes of stderr :
log4j:WARN No appenders could be found for logger (org.apache.hadoop.mapreduce.v2.app.MRAppMaster).
log4j:WARN Please initialize the log4j system properly.
log4j:WARN See http://logging.apache.org/log4j/1.2/faq.html#noconfig for more info.


[2023-09-27 18:00:30.595]Container exited with a non-zero exit code 1. Error file: prelaunch.err.
Last 4096 bytes of prelaunch.err :
Last 4096 bytes of stderr :
log4j:WARN No appenders could be found for logger (org.apache.hadoop.mapreduce.v2.app.MRAppMaster).
log4j:WARN Please initialize the log4j system properly.
log4j:WARN See http://logging.apache.org/log4j/1.2/faq.html#noconfig for more info.


For more detailed output, check the application tracking page: http://hadoop103:8088/cluster/app/application_1695808497717_0003 Then click on links to logs of each attempt.
. Failing the application.
```

解决方法：需要添加如下参数

```xml
<!-- 指定 rm1 的 web 端地址 -->
<property>
	<name>yarn.resourcemanager.webapp.address.rm1</name>
	<value>hadoop103:8088</value>
</property>

<!-- 指定 AM 向 rm1 申请资源的地址 -->
<property>
	<name>yarn.resourcemanager.scheduler.address.rm1</name>
	<value>hadoop103:8030</value>
</property>

<!-- 环境变量的继承 -->
<property>
	<name>yarn.nodemanager.env-whitelist</name>
	<value>JAVA_HOME,HADOOP_COMMON_HOME,HADOOP_HDFS_HOME,HADOOP_CONF_DIR,CLASSPATH_PREPEND_DISTCACHE,HADOOP_YARN_HOME,HADOOP_MAPRED_HOME</value>
</property>
```