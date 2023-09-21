---
author: wufm
title: Hadoop生产调优手册
rating: 6
time: 2023-09-16 周六
tags:
  - Hadoop
  - HDFS
  - MapReduce
  - Yarn
  - Hadoop生成调优手册
代码:
---
## 一、HDFS—核心参数

### 1.1 NameNode内存生产配置

#### 1.1.1 NameNode内存计算

每个文件块大概占用 **150byte**，一台服务器 128G 内存为例，能存储多少文件块呢？

```txt
128 * 1024 * 1024 * 1024 / 150Byte ≈ 9.1 亿
G      MB         KB     Byte
```

[硬件要求 |6.x |Cloudera 文档](https://docs.cloudera.com/documentation/enterprise/6/release-notes/topics/rg_hardware_requirements.html#concept_fzz_dq4_gbb)

![[NameNode和DataNode的内存要求.excalidraw|600]]
#### 1.1.2 查看NameNode和DataNode占用内存

Hadoop2.x 系列，NameNode 内存默认 2000m；Hadoop3.x 系列，Hadoop 的内存是动态分配的。

```shell
# 1.查看进程
jps

# 结果
1921 NameNode
2690 JobHistoryServer
2067 DataNode
2483 NodeManager
2794 Jps

# 2.查看NameNode和DataNode占用内存
jmap -heap 1921
jmap -heap 2067

# 结果都一样
Heap Configuration:
   MaxHeapSize              = 989855744 (944.0MB)
```

查看发现 hadoop101 上的 NameNode 和 DataNode 占用内存都是自动分配的，且相等。不是很合理。
#### 1.1.3 内存配置

2. 配置 NameNode 内存，新增一行数据

```shell
cd /opt/module/hadoop-3.3.6/etc/hadoop
vim hadoop-env.sh
# namenode内存设置，以下二选一
# 新增一行数据。在（# export HADOOP_HEAPSIZE_MIN=）下添加
HADOOP_NAMENODE_OPTS=-Xmx102400m
export HDFS_NAMENODE_OPTS="-Dhadoop.security.logger=INFO,RFAS -Xmx1024m"

# datanode内存设置
export HDFS_DATANODE_OPTS="-Dhadoop.security.logger=ERROR,RFAS-Xmx1024m"
```

### 1.2 NameNode心跳并发配置

![[NameNode 心跳并发配置.excalidraw]]
企业经验：dfs.namenode.handler.count=20 × 𝑙𝑜𝑔<sub>e</sub><sup>𝐶𝑙𝑢𝑠𝑡𝑒𝑟 𝑆𝑖𝑧𝑒</sup> ，比如集群规模（DataNode 台
数）为 3 台时，此参数设置为 21。可通过简单的 python 代码计算该值，代码如下。

```shell
sudo yum install -y python
python
```

```python
import math
print int(20*math.log(3))
quit()
```

配置NameNode 工作线程池，新增如下参数

```shell
cd /opt/module/hadoop-3.3.6/etc/hadoop
vim hdfs-site.xml
```

```xml
<!--NameNode 有一个工作线程池，用来处理不同 DataNode 的并发心跳以及客户端并发的元数据操作。-->
<!--对于大集群或者有大量客户端的集群来说，通常需要增大该参数。默认值是 10。-->
<property>
<name>dfs.namenode.handler.count</name>
<value>21</value>
</property>
```
### 1.3 开启回收站配置

开启回收站功能，可以将删除的文件在不超时的情况下，恢复原数据，起到防止误删除、备份等作用。

![[回收站工作机制.excalidraw|350]]
#### 1.3.1 启动回收站

修改core-site.xml，配置垃圾回收时间为 60 分钟。添加如下参数：

```shell
cd /opt/module/hadoop-3.3.6/etc/hadoop
vim core-site.xml
```

```xml
<!--垃圾回收时间为 60 分钟-->
<!--默认值为0，表示禁用回收站-->
<property>
<name>fs.trash.interval</name>
<value>60</value>
</property>

<!--10分钟检测下回收站-->
<!--如果该值为 0，则该值设置和 fs.trash.interval 的参数值相等-->
<property>
<name>fs.trash.checkpoint.interval</name>
<value>10</value>
</property>
```

#### 1.3.2 数据进入回收站

回收站目录在 HDFS 集群中的路径：/user/hadoop/.Trash/...

1. ==注意：通过网页上直接删除的文件也不会走回收站==

2. 通过程序删除的文件不会经过回收站，需要调用 moveToTrash()才进入回收站

```java
Trash trash = New Trash(conf);
trash.moveToTrash(path);
```

3. 只有在命令行利用 ==hadoop fs -rm ==命令删除的文件才会走回收站。

```shell
hadoop fs -rm -r /output
```
#### 1.3.3 恢复回收站数据

```shell
hadoop fs -mv /user/hadoop/.Trash/Current/output /ouput
```
## 二、HDFS—集群压测

在企业中非常关心每天从 Java 后台拉取过来的数据，需要多久能上传到集群？消费者关心多久能从 HDFS 上拉取需要的数据？

为了搞清楚 HDFS 的读写性能，生产环境上非常需要对集群进行压测。HDFS 的读写性能主要受==网络和磁盘==影响比较大。为了方便测试，将 hadoop101、hadoop102、hadoop103 虚拟机网络都设置为 100mbps。100Mbps 单位是 bit；10M/s 单位是 byte ; 1byte=8bit，**100Mbps/8=12.5M/s**。

测试网速：来到 hadoop102 的/opt/module 目录，创建一个目录

```shell
python -m SimpleHTTPServer
```

![[Drawing 2023-09-18 11.03.34.excalidraw|350]]

![[Pasted image 20230918111610.png|600]]
### 2.1 测试HDFS写性能

![[写测试底层原理.excalidraw]]

测试内容：向 HDFS 集群写 10 个 128M 的文件

```shell
# nrFiles n 为生成 mapTask 的数量，生产环境一般可通过 hadoop103:8088 查看 CPU核数，设置为（CPU 核数 - 1）
hadoop jar /opt/module/hadoop-3.3.6/share/hadoop/mapreduce/hadoop-mapreduce-client-jobclient-3.3.6-tests.jar TestDFSIO -write -nrFiles 10 -fileSize 128MB
```

![[Pasted image 20230918114821.png]]

- Number of files：生成mapTask数量，一般是集群CPU核数-1，我们测试虚拟机就按照实际的物理内存-1分配即可
- Total MBytes processed：单个map处理的文件大小
- ==Throughput mb/sec==：单个mapTask的吞吐量（处理的总文件大小/每一个maptask写数据的时间累加）
- ==Average IO rate mb/sec==：平均maptask的吞吐量（每个maptask处理文件大小/每一个maptask写数据的时间，全部相加除以 task 数量）
IO rate std deviation：方差、反映各个 mapTask 处理的差值，越小越均衡

==注意：如果测试过程中，出现异常==。可以在 yarn-site.xml 中设置虚拟内存检测为 false。分发配置并重启 Yarn 集群

```shell
cd /opt/module/hadoop-3.3.6/etc/hadoop
vim yarn-site.xml
```

```xml
<!--是否启动一个线程检查每个任务正使用的虚拟内存量，如果任务超出分配值，则
直接将其杀掉，默认是 true -->
<property>
<name>yarn.nodemanager.vmem-check-enabled</name>
<value>false</value>
</property>
```

**测试结果分析**

由于副本 1 就在本地，所以该副本不参与测试（如果客户端不在集群节点，那就三个副本都参与计算）
![[HDFS副本.excalidraw|400]]
- 一共参与测试的文件：10个文件 * 2个副本 = 20个
- 压测后的速度：1.57
- ==实测速度==：1.57M/S * 20个文件 约= 31M/s
- ==三台服务器的宽带==：12.5 * 3 约= 30M/s

所有的网络资源都已经用满

**如果实测速度远远小于网络，并且实测速度不能满足工作需求，可以考虑采用固态硬盘或者增加磁盘个数**
### 2.2 测试HDFS读性能

测试内容：读取 HDFS 集群 10 个 128M 的文件

```shell
hadoop jar /opt/module/hadoop-3.3.6/share/hadoop/mapreduce/hadoop-mapreduce-client-jobclient-3.3.6-tests.jar TestDFSIO -read -nrFiles 10 -fileSize 128MB
```

![[Pasted image 20230918115058.png]]

删除测试生成数据

```shell
hadoop jar /opt/module/hadoop-3.3.6/share/hadoop/mapreduce/hadoop-mapreduce-client-jobclient-3.3.6-tests.jar TestDFSIO -clean
```

**测试结果分析**：为什么读取文件速度大于网络带宽？由于目前只有三台服务器，且有三个副本，数据读取就近原则，==相当于都是读取的本地磁盘数据，没有走网络==。
## 三、HDFS—多目录

### 3.1 NameNode多目录配置

nameNode的本地目录可以配置成多个，==且每个目录存放内容相同==，增加可靠性

![[namenode多目录配置.excalidraw|300]]

**具体配置如下：**

在 hdfs-site.xml 文件中添加如下内容

```shell
# hadoop101
cd /opt/module/hadoop-3.3.6/etc/hadoop
vim hdfs-site.xml
```

```xml
<property>
<name>dfs.namenode.name.dir</name>
<value>file://${hadoop.tmp.dir}/dfs/name1,file://${hadoop.tmp.dir}/dfs/name2</value>
</property>
```

停止集群，删除三台节点的 data 和 logs 中所有数据。

```shell
# 停止集群
myhadoop.sh stop

# 删除三台节点的 data 和 logs 中所有数据。
rm -rf /opt/data/hadoop-3.3.6/
rm -rf /opt/module/hadoop-3.3.6/logs/
```

格式化集群并启动。

```shell
# 在hadoop101 格式化集群
hdfs namenode -format

# 启动集群
myhadoop.sh start
```

查看结果

```shell
# hadoop101,检查 name1 和 name2 里面的内容，发现一模一样。
cd /opt/data/hadoop-3.3.6/dfs
ll 
```

![[Pasted image 20230918123347.png|400]]
### 3.2 DataNode多目录配置

DataNode 可以配置成多个目录，==每个目录存储的数据不一样==（数据不是副本）

![[datanode多目录配置.excalidraw|450]]
**具体配置如下**：

把hadoop101的datanode目录配置两个，配置只需要修改hadoop101即可

在 hdfs-site.xml 文件中添加如下内容

```shell
# hadoop101
cd /opt/module/hadoop-3.3.6/etc/hadoop
vim hdfs-site.xml
```

```xml
<property>
<name>dfs.datanode.data.dir</name>
<value>file://${hadoop.tmp.dir}/dfs/data1,file://${hadoop.tmp.dir}/dfs/data2</value>
</property>
```

关闭hadoop101的datanode服务，并启动

```shell
hdfs --daemon stop datanode
hdfs --daemon start datanode
```

查看目录

```shell
cd /opt/data/hadoop-3.3.6/dfs
ll
```

向集群上传一个文件，再次观察两个文件夹里面的内容发现不一致（一个有数一个没有）
### 3.3 集群数据均衡之磁盘间数据均衡

生产环境，由于硬盘空间不足，往往需要增加一块硬盘。刚加载的硬盘没有数据时，可以执行磁盘数据均衡命令。（Hadoop3.x 新特性）

![[集群数据均衡之磁盘间数据均衡.excalidraw|400]]

```shell
# 生成均衡计划（我们只有一块磁盘，不会生成计划）
hdfs diskbalancer -plan hadoop101

# 执行均衡计划
hdfs diskbalancer -execute hadoop101.plan.json

# 查看当前均衡任务的执行情况
hdfs diskbalancer -query hadoop101

# 取消均衡任务
hdfs diskbalancer -cancel hadoop101.plan.json
```

## 四、HDFS—集群扩容及缩容

### 4.1 添加白名单

白名单：==表示在白名单的主机 IP 地址可以，用来存储数据。==

企业中：**配置白名单，可以尽量防止黑客恶意访问攻击。**

3. 在 web 浏览器上查看 datanodes

企业中：配置白名单，可以尽量防止黑客恶意访问攻击。

![[白名单.excalidraw|450]]
配置白名单步骤如下：

1. 创建白名单和黑名单文件

```shell
# 在hadoop101
cd /opt/module/hadoop-3.3.6/etc/hadoop
# 创建白名单
vim whitelist

# 添加如下内容
hadoop101
hadoop102

# 创建黑名单（保持空的就可以）
touch blacklist
```

在 hdfs-site.xml 配置文件中增加 dfs.hosts 配置参数

```shell
vim hdfs-site.xml
```

```xml
<!-- 白名单 -->
<property>
<name>dfs.hosts</name>
<value>/opt/module/hadoop-3.3.6/etc/hadoop/whitelist</value>
</property>

<!-- 黑名单 -->
<property>
<name>dfs.hosts.exclude</name>
<value>/opt/module/hadoop-3.3.6/etc/hadoop/blacklist</value>
</property>
```

2. 第一次添加白名单必须重启集群，不是第一次，只需要刷新 NameNode 节点即可

```shell
myhadoop.sh stop
myhadoop.sh start
```

3. 在 web 浏览器上查看 datanodes

![[Pasted image 20230918200319.png|500]]


5. 二次修改白名单，增加 hadoop103

```
vim whitelist

# 新增
hadoop103
```

6. 刷新 NameNode

```shell
hdfs dfsadmin -refreshNodes
```

7.  在 web 浏览器上查看 datanodes

![[Pasted image 20230918220025.png|500]]


### 4.2 服役新服务器

随着公司业务的增长，数据量越来越大，原有的数据节点的容量已经不能满足存储数据的需求，需要在原有集群基础上动态添加新的数据节点。

1. 环境准备
- 在 hadoop102 主机上再克隆一台 hadoop104主机
- 修改 IP 地址和主机名称

```shell
# 修改ip为104
vim /etc/sysconfig/network-scripts/ifcfg-ens33

# 修改为hadoop104
vim /etc/hostname

# 重启
reboot
```

- 删除 hadoop104上Hadoop 的历史数据，data 和 log 数据

```shell
rm -rf /opt/data/hadoop-3.3.6/
rm -rf /opt/module/hadoop-3.3.6/logs/
```

- 配置域名映射

```shell
# 在hadoop101-104都要配置
sudo vim /etc/hosts
# 添加如下内容并保存：
192.168.204.101	hadoop101
192.168.204.102	hadoop102
192.168.204.103	hadoop103
192.168.204.104	hadoop104
```

- 配置免密登陆

```shell
cd /home/hadoop
ls -al
rm -rf .ssh/
# 先后在hadoop101、hadoop102和hadoop103执行以下操作

# 在hadoop104上生成公钥和私钥
# 1. 输入后敲（三个回车），就会生成两个文件 id_rsa（私钥）、id_rsa.pub（公钥）
ssh-keygen -t rsa

# 2.将公钥拷贝到要免密登录的目标机器上
ssh-copy-id hadoop104
ssh-copy-id hadoop101
ssh-copy-id hadoop102
ssh-copy-id hadoop103

# 在hadoop101-103,分别执行
ssh-copy-id hadoop104

# 3.可以进入相应目录，查看生成的信息
cd /home/hadoop/.ssh
ll
```

2. 服役新节点具体步骤

- 添加白名单

```shell
# 在hadoop101
cd /opt/module/hadoop-3.3.6/etc/hadoop
# 创建白名单
vim whitelist

# 添加如下内容
hadoop104

# 刷新namenode
hdfs dfsadmin -refreshNodes
```

- 启动新节点（直接启动 DataNode，即可关联到集群）

```shell
# 在hadoop104执行
hdfs --daemon start datanode
yarn --daemon start nodemanager
```

![[Pasted image 20230919100101.png|600*500]]



- 在 hadoop104上上传文件

```shell
hadoop fs -put wc.jar /
```

![[Pasted image 20230919100314.png|500]]

==思考：如果数据不均衡（hadoop104 数据少，其他节点数据多），怎么处理？==
### 4.3 服务器间数据均衡

在企业开发中，如果经常在 hadoop102 和 hadoop104 上提交任务，且副本数为 2，由于数据本地性原则，就会导致 hadoop102 和 hadoop104 数据过多，hadoop103 存储的数据量小。

另一种情况，就是新服役的服务器数据量比较少，需要执行集群均衡命令。

注意：==由于 HDFS 需要启动单独的 Rebalance Server 来执行 Rebalance 操作，所以尽量不要在 NameNode 上执行 start-balancer.sh，而是找一台比较空闲的机器。==

```shell
# 1.开始数据均衡：
# 对于参数 10，代表的是集群中各个节点的磁盘空间利用率相差不超过 10%，可根据实际情况进行调整。
start-balancer.sh -threshold 10

# 2.停止数据均衡命令
stop-balancer.sh
```

### 4.4 黑名单退役服务器

黑名单：==表示在黑名单的主机 IP 地址不可以，用来存储数据==。

企业中：**配置黑名单，用来退役服务器**

黑名单：hadoop104

```shell
# 在hadoop101
cd /opt/module/hadoop-3.3.6/etc/hadoop
# 创建白名单
vim blacklist

# 添加如下内容
hadoop104
```

```shell
vim hdfs-site.xml
```

```xml
<!-- 黑名单 -->
<property>
<name>dfs.hosts.exclude</name>
<value>/opt/module/hadoop-3.3.6/etc/hadoop/blacklist</value>
</property>
```

第一次添加黑名单必须重启集群，不是第一次，只需要刷新 NameNode 节点即可

```shell
hdfs dfsadmin -refreshNodes
```

检查 Web 浏览器，退役节点的状态为 decommission in progress（退役中），说明数据节点正在复制块到其他节点
![[Pasted image 20230919102126.png]]

==等待退役节点状态为 decommissioned（所有块已经复制完成），停止该节点及节点资源管理器==。注意：如果副本数是 3，服役的节点小于等于 3，是不能退役成功的，需要修改副本数后才能退役

![[Pasted image 20230919102308.png]]

正式退役：

```shell
# 在hadoop104执行
hdfs --daemon stop datanode
yarn --daemon stop nodemanager
```

如果数据不均衡，可以用命令实现集群的再平衡。[[#4.3 服务器间数据均衡]]


## 五、HDFS—存储优化

==注：演示纠删码和异构存储需要一共 5 台虚拟机。尽量拿另外一套集群。提前准备 5 台服务器的集群。==

服役新服务器参考[[#4.2 服役新服务器]]
### 5.1 纠删码

#### 5.1.1 纠删码原理

HDFS默认情况，一个文件有3个副本，这样提高了数据的可靠性，但也带来了2倍的冗余开销。Hadoop3.x引入了纠删码，采用计算的方式，可以节省约50%左右的存储空间。

![[纠删码原理.excalidraw]]
```shell
# 纠删码操作相关的命令
hdfs ec
```

![[Pasted image 20230919105917.png|400]]

```shell
# 查看当前支持的纠删码策略
hdfs ec -listPolicies
```

![[Pasted image 20230919110043.png]]

```txt
RS-10-4-1024k：使用RS编码，每10个数据单元（cell），生成4个校验单元，共 14个单元，也就是说：这 14 个单元中，只要有任意的10个单元存在（不管是数据单元还是校验单元，只要总数=10），就可以得到原始数据。每个单元的大小是 1024k=1024*1024=1048576。

RS-3-2-1024k：使用RS编码，每3个数据单元，生成2个校验单元，共5个单元，也就是说：这5个单元中，只要有任意的3个单元存在（不管是数据单元还是校验单元，只要总数=3），就可以得到原始数据。

RS-6-3-1024k：使用RS编码，每6个数据单元，生成3个校验单元，共9个单元，也就是说：这9个单元中，只要有任意的6个单元存在（不管是数据单元还是校验单元，只要总数=6），就可以得到原始数据。

RS-LEGACY-6-3-1024k：策略和上面的RS-6-3-1024k一样，只是编码的算法用的是rs-legacy。

XOR-2-1-1024k：使用XOR编码（速度比 RS 编码快），每2个数据单元，生成1个校验单元，共3个单元，也就是说：这3个单元中，只要有任意的2个单元存在（不管是数据单元还是校验单元，只要总数= 2），就可以得到原始数据。
```
#### 5.1.2 纠删码案例实操

纠删码策略是给具体一个路径设置。所有往此路径下存储的文件，都会执行此策略。

默认只开启对 RS-6-3-1024k 策略的支持，如要使用别的策略需要提前启用。

1. 需求：将/input 目录设置为 RS-3-2-1024k 策略
2. 准备工作：

```shell
# 删除所有的数据（删不成功，页面删除）
hadoop fs -rm -r /
```

datanode目录只有一个，方便测试。参见[[datanode多目录配置.excalidraw]]

3. 具体步骤：

```shell
# 1.开启对 RS-3-2-1024k 策略的支持
hdfs ec -enablePolicy -policy RS-3-2-1024k

# 2.在HDFS创建目录，并设置RS-3-2-1024k策略
hdfs dfs -mkdir /input
hdfs ec -setPolicy -path /input -policy RS-3-2-1024k

# 3.上传文件，并查看文件编码后的存储情况
# 注：你所上传的文件需要大于 2M 才能看出效果。（低于 2M，只有一个数据单元和两个校验单元）
# jdk-8u144-linux-x64.tar.gz 177M （五个单元，每个单元大小177/3=59m）
hdfs dfs -put /opt/software/jdk-8u144-linux-x64.tar.gz /input
hdfs dfs -put /home/hadoop/weiguo.txt /input

# 4.查看存储路径的数据单元和校验单元，并作破坏实验
# 在hadoop104和hadoop105删除数据
cd /opt/data/hadoop-3.3.6/dfs/data/current/BP-1611234272-192.168.204.101-1695011506281/current/finalized/subdir0/subdir0
rm -rf blk_*
# 5.网页或命令下载文件
hdfs dfs -get /input/jdk-8u144-linux-x64.tar.gz /home/hadoop
```

![[Pasted image 20230919113758.png|450]]

### 5.2 异构存储（冷热数据分离）

==异构存储主要解决，不同的数据，存储在不同类型的硬盘中，达到最佳性能的问题。==

![[异构存储（冷热数据分离）.excalidraw]]
#### 5.2.1 存储类型和存储策略

1. 关于存储类型

```txt
RAM_DISK：内存镜像文件系统
SSD：固定硬盘
DISK：普通磁盘。在HDFS中，如果没有主动声明数据目录存储类型，默认是DISK
ARCHIVE：归档存储。没有特指哪种存储介质，主要指的是计算能力比较弱而存储密度比较高生完存储介质，用来解决数据量的容量扩增的问题，一般用于归档
```

2. 关于存储策略

说明：**从Lazy_Persist到Cold，分别代表了设备的访问速度从快到慢**

| 策略ID | 策略名称     | 副本分布            | 说明                                               |
| ------ | ------------ | ------------------- | -------------------------------------------------- |
| 15     | Lazy_Persist | RAM_DISK:1,DISK:n-1 | 一个副本保存在内存RAM_DISK中，其余副本保存在磁盘中 |
| 12     | All_SDD      | SSD:n               | 所有副本都保存在SSD中                              |
| 10     | One_SSD      | SSD:1,DISK:n-1      | 一个副本保存在SSD，其余副本保存在磁盘中            |
| 7      | Hot(default) | DISK:n              | 所有副本保存在磁盘中，这也是默认的存储策略         |
| 5      | Warm         | DSIK:1,ARCHIVE:n-1  | 一个副本保存在磁盘上，其余副本保存在归档存储上     |
| 2      | Cold         | ARCHIVE:n           | 所有副本都保存在归档存储上                         |

#### 5.2.2 异构存储 Shell 操作

```shell
# 1.查看当前有哪些存储策略可以用
hdfs storagepolicies -listPolicies

# 2.为指定路径（数据存储目录）设置指定的存储策略
hdfs storagepolicies -setStoragePolicy -path 路径 -policy 策略

# 3.获取指定路径（数据存储目录或文件）的存储策略
hdfs storagepolicies -getStoragePolicy -path 路径

# 4.取消存储策略；执行改命令之后该目录或者文件，以其上级的目录为准，如果是根目录，那么就是 HOT
hdfs storagepolicies -unsetStoragePolicy -path 路径

# 5.查看文件块的分布
hdfs fsck 路径 -files -blocks -locations

# 6.查看集群节点
hadoop dfsadmin -report
```

#### 5.2.3 异构存储测试
##### 5.2.3.1 测试环境准备

**1. 测试环境描述**

服务器规模：5 台

集群配置：副本数为 2，创建好带有存储类型的目录（提前创建）

集群规划：

| 节点      | 存储类型分配  |
| --------- | ------------- |
| hadoop101 | RAM_DISK,SSD  |
| hadoop102 | SSD,DISK      |
| hadoop103 | DISK,RAM_DISK |
| hadoop104 | ARCHIVE       |
| hadoop105 | ARCHIVE              |

**2. 配置文件信息**

1）为 hadoop101 节点的 hdfs-site.xml 添加如下信息

```shell
cd /opt/module/hadoop-3.3.6/etc/hadoop/
vim hdfs-site.xml
```

```xml
<!-- 副本数-->
<property>
<name>dfs.replication</name>
<value>2</value>
</property>

<!-- 开启存储策略-->
<property>
<name>dfs.storage.policy.enabled</name>
<value>true</value>
</property>

<!-- datanode数据存储路径-->
<property>
<name>dfs.datanode.data.dir</name>
<value>[SSD]file:///opt/data/hadoop-3.3.6/dfs/hdfsdata/ssd,[RAM_DISK]file:///opt/data/hadoop-3.3.6/dfs/hdfsdata/ram_disk</value>
</property>
```

2）为 hadoop102-105跟1）一样。只需要修改最后一个参数的value值（根据集群规范）。

**3. 数据准备**

1）启动集群

```shell
# 删除原来的数据和logs
rm -rf /opt/data/hadoop-3.3.6/
rm -rf /opt/module/hadoop-3.3.6/logs/

# 在hadoop101执行
hdfs namenode -format

# 启动集群
myhadoop.sh start
# 新加入的机器可以单独启动（或者重新写myhadoop.sh脚本）
# 在hadoop104-105执行
hdfs --daemon start datanode
yarn --daemon start nodemanager
```

2）并在 HDFS 上创建文件目录，并将文件资料上传

```shell
hadoop fs -mkdir /hdfsdata
hdfs dfs -put weiguo.txt /hdfsdata
```

##### 5.2.3.2 存储策略案例

###### 5.2.3.2.1 HOT 存储策略案例

HOT存储策略：所有的副本存储在磁盘DISK。

==未设置存储策略，所有文件块都存储在 DISK 下==。所以，默认存储策略为 HOT。

```shell
# 最开始我们未设置存储策略的情况下，我们获取该目录的存储策略
hdfs storagepolicies -getStoragePolicy -path /hdfsdata

# 我们查看上传的文件块分布
hdfs fsck /hdfsdata -files -blocks -locations

# 结果
[DatanodeInfoWithStorage[192.168.204.102:9866,DS-6242d660-99f2-4ebc-af89-a647735f6646,DISK], DatanodeInfoWithStorage[192.168.204.103:9866,DS-a696fd6a-8c20-439c-81bd-2c75a29b4f77,DISK]]
```
###### 5.2.3.2.2 WARM 存储策略测试

WARN存储策略：一个副本保存在磁盘DISK，其他副本存储在磁盘归档存储上ARCHIVE。

```shell
# 1.为数据降温
hdfs storagepolicies -setStoragePolicy -path /hdfsdata -policy WARM

# 2.再次查看文件块分布，我们可以看到文件块依然放在原处。
hdfs fsck /hdfsdata -files -blocks -locations

# 3.我们需要让他 HDFS 按照存储策略自行移动文件块
hdfs mover /hdfsdata

# 4.再次查看文件块分布（文件块一半在 DISK，一半在 ARCHIVE，符合我们设置的 WARM 策略）
hdfs fsck /hdfsdata -files -blocks -locations

# 结果
[DatanodeInfoWithStorage[192.168.204.105:9866,DS-dbf2f8f7-830e-4dd1-8a24-6f665ab3d2ac,ARCHIVE], DatanodeInfoWithStorage[192.168.204.103:9866,DS-a696fd6a-8c20-439c-81bd-2c75a29b4f77,DISK]]
```
###### 5.2.3.2.3 COLD 存储策略测试

COLD存储策略：所有副本存储在磁盘归档存储上ARCHIVE。

```shell
# 1.继续降温
hdfs storagepolicies -setStoragePolicy -path /hdfsdata -policy COLD

# 2.我们需要让他 HDFS 按照存储策略自行移动文件块
hdfs mover /hdfsdata

# 3.检查文件块的分布（所有文件块都在 ARCHIVE，符合 COLD 存储策略。）
hdfs fsck /hdfsdata -files -blocks -locations

# 结果
[DatanodeInfoWithStorage[192.168.204.105:9866,DS-dbf2f8f7-830e-4dd1-8a24-6f665ab3d2ac,ARCHIVE], DatanodeInfoWithStorage[192.168.204.104:9866,DS-fef1171f-1072-412e-802b-273d09e535eb,ARCHIVE]]
```
###### 5.2.3.2.3 ONE_SSD 存储策略测试

ONE_SSD存储策略：一个副本保存在SSD，其余副本保存在磁盘DISK中。

```shell
# 1.存储策略更改为 One_SSD
hdfs storagepolicies -setStoragePolicy -path /hdfsdata -policy One_SSD

# 2.我们需要让他 HDFS 按照存储策略自行移动文件块
hdfs mover /hdfsdata

# 3.检查文件块的分布（文件块分布为一半在 SSD，一半在 DISK，符合 One_SSD 存储策略。）
hdfs fsck /hdfsdata -files -blocks -locations

# 结果
[DatanodeInfoWithStorage[192.168.204.103:9866,DS-a696fd6a-8c20-439c-81bd-2c75a29b4f77,DISK], DatanodeInfoWithStorage[192.168.204.102:9866,DS-9a5bb273-d764-4434-aa71-b17901f2f5bd,SSD]]
```
###### 5.2.3.2.4 ALL_SSD 存储策略测试

ALL_SSD存储策略：所有副本保存在SSD。

```shell
# 1.存储策略更改为 All_SSD
hdfs storagepolicies -setStoragePolicy -path /hdfsdata -policy All_SSD

# 2.我们需要让他 HDFS 按照存储策略自行移动文件块
hdfs mover /hdfsdata

# 3.检查文件块的分布（所有的文件块都存储在 SSD，符合 All_SSD 存储策略。）
hdfs fsck /hdfsdata -files -blocks -locations

# 结果
[DatanodeInfoWithStorage[192.168.204.101:9866,DS-5ad2aa69-c9ce-47d2-b260-0df7b99c5c1a,SSD], DatanodeInfoWithStorage[192.168.204.102:9866,DS-9a5bb273-d764-4434-aa71-b17901f2f5bd,SSD]]
```
###### 5.2.3.2.5 LAZY_PERSIST 存储策略测试

LAZY_PERSIST存储策略：一个副本保存在内存RAM_DISK，其余副本保存在磁盘DISK。

```shell
# 1.存储策略更改为 lazy_persist
hdfs storagepolicies -setStoragePolicy -path /hdfsdata -policy lazy_persist

# 2.我们需要让他 HDFS 按照存储策略自行移动文件块
hdfs mover /hdfsdata

# 3.检查文件块的分布
hdfs fsck /hdfsdata -files -blocks -locations

# 结果（跟预期不一样）
[DatanodeInfoWithStorage[192.168.204.103:9866,DS-a696fd6a-8c20-439c-81bd-2c75a29b4f77,DISK], DatanodeInfoWithStorage[192.168.204.102:9866,DS-6242d660-99f2-4ebc-af89-a647735f6646,DISK]]
```

这里我们发现所有的文件块都是存储在 DISK，按照理论一个副本存储在 RAM_DISK，其他副本存储在 DISK 中，这是因为，我们还需要配置“dfs.datanode.max.locked.memory”，“dfs.block.size”参数。

那么出现存储策略为 LAZY_PERSIST 时，文件块副本都存储在 DISK 上的原因有如下两点：

```txt
（1）当客户端所在的 DataNode 节点没有 RAM_DISK 时，则会写入客户端所在的DataNode节点的 DISK 磁盘，其余副本会写入其他节点的 DISK 磁盘。

（2）当客户端所在的 DataNode 有 RAM_DISK，但“dfs.datanode.max.locked.memory”参数值未设置或者设置过小（小于“dfs.block.size”参数值）时，则会写入客户端所在的DataNode 节点的 DISK 磁盘，其余副本会写入其他节点的 DISK 磁盘。
```

```xml
<!-- 用于在数据节点上的内存中缓存块副本的内存量（以字节为单位）。默认情况下，此参数设置为0，这将禁用	内存中缓存。内存值过小会导致内存中的总的可存储的数据块变少，但如果超过 DataNode 能承受的最大内存大小的话，部分内存块会被直接移出 。byte 类型,64k -->
<property>
	<name>dfs.datanode.max.locked.memory</name>
	<value>65536</value>
</property>
```

但是由于虚拟机的“max locked memory”为 64KB，==这个策略演示不了==。所以，如果参数配置过大，还会报出错误：

```txt
ERROR org.apache.hadoop.hdfs.server.datanode.DataNode: Exception insecureMain
java.lang.RuntimeException: Cannot start datanode because the configured max locked memory size (dfs.datanode.max.locked.memory) of 209715200 bytes is more than the datanode's available RLIMIT_MEMLOCK ulimit of 65536 bytes.
```

我们可以通过该命令查询此参数的内存

```shell
ulimit -a

# 结果
max locked memory (kbytes, -l) 64
```
## 六、HDFS—故障排除

==注意：采用三台服务器即可，恢复到 Yarn 开始的服务器快照。==
### 6.1 NameNode故障处理

![[NameNode故障.excalidraw|450]]
**1. 需求**：==NameNode 进程挂了并且存储的数据也丢失了，如何恢复 NameNode==

**2. 故障模拟**

```shell
# 1.停止namenode进程（jps查看进程）
kill -9 8472

# 2.删除 NameNode 存储的数据
rm -rf /opt/data/hadoop-3.3.6/dfs/name/*
```

**3. 问题解决**

```shell
# 1.拷贝 SecondaryNameNode 中数据到原 NameNode 存储数据目录
scp -r hadoop@hadoop103:/opt/data/hadoop-3.3.6/dfs/namesecondary/* /opt/data/hadoop-3.3.6/dfs/name/

# 2.重新启动 NameNode
hdfs --daemon start namenode
```

此方法只是部分恢复
![[Drawing 2023-09-19 18.54.38.excalidraw]]
### 6.2 集群安全模式&磁盘修复

安全模式：==文件系统只接受读数据请求，而不接受删除、修改等变更请求==

进入安全模式场景：
1. NameNode 在加载镜像文件和编辑日志期间处于安全模式；
2. NameNode 再接收 DataNode 注册时，处于安全模式

退出安全模式条件：
- dfs.namenode.safemode.min.datanodes:最小可用 datanode 数量，默认 0
- dfs.namenode.safemode.threshold-pct:副本数达到最小要求的 block 占系统总 block 数的百分比，默认 0.999f。（只允许丢一个块）
- dfs.namenode.safemode.extension:稳定时间，默认值 30000 毫秒，即 30 秒

基本语法：

```shell
# 查看安全模式状态
hdfs dfsadmin -safemode get

# 进入安全模式状态
hdfs dfsadmin -safemode enter

# 离开安全模式状态
hdfs dfsadmin -safemode leave

# 等待安全模式状态
hdfs dfsadmin -safemode wait
```

#### 6.1.1 案例 1：启动集群进入安全模式

重新启动集群

```shell
myhadoop.sh stop
myhadoop.sh start
```

集群启动后，立即来到集群上删除数据，提示集群处于安全模式

![[Pasted image 20230919192000.png]]

#### 6.1.2 案例 2：磁盘修复

需求：数据块损坏，进入安全模式，如何处理

```shell
# 分别进入hadoop101-103执行以下命令。统一删除某2个块信息
cd /opt/data/hadoop-3.3.6/dfs/data/current/BP-1375785971-192.168.204.101-1693365248220/current/finalized/subdir0/subdir0

rm -rf blk_1073742046 blk_1073742046_1222.meta
rm -rf blk_1073742047 blk_1073742047_1223.meta
```

重新启动集群

```shell
myhadoop.sh stop
myhadoop.sh start
```

说明：安全模式已经打开，块的数量没有达到要求。

![[Pasted image 20230919192959.png]]

离开安全模式

```shell
# 查看安全模式状态
hdfs dfsadmin -safemode get
# 离开安全模式状态
hdfs dfsadmin -safemode leave
```

![[Pasted image 20230919211858.png|600]]

删除元数据

![[Pasted image 20230919212126.png]]

集群已经正常
![[Pasted image 20230919213202.png|500]]

#### 6.1.3 案例 3：模拟等待安全模式

```shell
# 1.查看当前模式
hdfs dfsadmin -safemode get

# 2.先进入安全模式
hdfs dfsadmin -safemode enter

# 3.创建脚本,添加下面内容
vim safemode.sh

#!/bin/bash
# 等待安全模式状态
hdfs dfsadmin -safemode wait
hdfs dfs -put /home/hadoop/test/test1.txt /

# 4.执行脚本
sh safemode.sh

# 5.可以先观察web端是否已经上传了文件。再打开一个窗口，执行
# 离开安全模式
hdfs dfsadmin -safemode leave
```

HDFS 集群上已经有上传的数据了

![[Pasted image 20230919214038.png]]
### 6.3 慢磁盘监控

“慢磁盘”指的时写入数据非常慢的一类磁盘。其实慢性磁盘并不少见，当机器运行时间长了，上面跑的任务多了，磁盘的读写性能自然会退化，严重时就会出现写入数据延时的问题。如何发现慢磁盘？

正常在 HDFS 上创建一个目录，只需要不到 1s 的时间。==如果你发现创建目录超过 1 分钟及以上，而且这个现象并不是每次都有。只是偶尔慢了一下，就很有可能存在慢磁盘。==

**可以采用如下方法找出是哪块磁盘慢：**

==1. 通过心跳未联系时间。==
一般出现慢磁盘现象，会影响到 DataNode 与 NameNode 之间的心跳。正常情况心跳时间间隔是 3s。超过 3s 说明有异常。

![[Pasted image 20230920092652.png]]

==2. fio 命令，测试磁盘的读写性能==

- 机械硬盘（HDD）：读取速度通常为100MB/s至200MB/s，写入速度通常为50MB/s至150MB/s。
- 固态硬盘（SSD）：读取速度通常为300MB/s至550MB/s，写入速度通常为200MB/s至350MB/s。
- M.2硬盘（NVMe）：读取速度通常高达3500MB/s，写入速度通常高达3000MB/s。

```shell
sudo yum install -y fio
```

（1）顺序读测试

```shell
# filename：测试文件名称；direct=1：测试过程绕过系统自带的buffer，使测试结果更真实。
# iodepth：异步队列深度，默认为1；thread：创建的是POSIX 线程
# rw：读写方式 read（顺序读）write（顺序写） randwrite（随机写测试） randrw（混合随机读写）
# ioengine：指定io引擎使用xx方式.官方有几十种，比如有sync，psync，一般这两个用的多一点
# bs：读写的块大小；size：测试的负载的数量；numjobs：本次测试的线程数；runtime：测试时间**秒
# group_reporting：显示结果，汇总每个进程的信息；name：一个任务的名字
sudo fio -filename=/home/hadoop/test.log -direct=1 -iodepth 1 -thread -rw=read -ioengine=psync -bs=16k -size=2G -numjobs=10 -runtime=60 -group_reporting -name=test_r

# 结果
Run status group 0 (all jobs):
   READ: bw=249MiB/s (261MB/s), 249MiB/s-249MiB/s (261MB/s-261MB/s), io=14.6GiB (15.6GB), run=60001-60001msec
```

结果显示，磁盘的总体顺序读速度为249MiB/s。

（2）顺序写测试

```shell
sudo fio -filename=/home/hadoop/test.log -direct=1 -iodepth 1 -thread -rw=write -ioengine=psync -bs=16k -size=2G -numjobs=10 -runtime=60 -group_reporting -name=test_w

# 结果
Run status group 0 (all jobs):
  WRITE: bw=248MiB/s (260MB/s), 248MiB/s-248MiB/s (260MB/s-260MB/s), io=14.5GiB (15.6GB), run=60001-60001msec
```

结果显示，磁盘的总体顺序写速度为 248MiB/s。

（3）随机写测试

```shell
sudo fio -filename=/home/hadoop/test.log -direct=1 -iodepth 1 -thread -rw=randwrite -ioengine=psync -bs=16k -size=2G -numjobs=10 -runtime=60 -group_reporting -name=test_w

# 结果
Run status group 0 (all jobs):
  WRITE: bw=229MiB/s (240MB/s), 229MiB/s-229MiB/s (240MB/s-240MB/s), io=13.4GiB (14.4GB), run=60001-60001msec
```

结果显示，磁盘的总体随机写速度为229MiB/s。

（4）混合随机读写

```shell
sudo fio -filename=/home/hadoop/test.log -direct=1 -iodepth 1 -thread -rw=randrw -ioengine=psync -bs=16k -size=2G -numjobs=10 -runtime=60 -group_reporting -name=test_w

# 结果
Run status group 0 (all jobs):
   READ: bw=120MiB/s (126MB/s), 120MiB/s-120MiB/s (126MB/s-126MB/s), io=7207MiB (7557MB), run=60001-60001msec
  WRITE: bw=120MiB/s (126MB/s), 120MiB/s-120MiB/s (126MB/s-126MB/s), io=7205MiB (7555MB), run=60001-60001msec
```

结果显示，磁盘的总体混合随机读写，读速度为120MiB/s，写速度 120MiB/s。
### 6.4 小文件归档

#### 6.4.1 HDFS存储小文件弊端

100个1k文件块和100个128m的文件块，占用NameNode内存大小是一样的。

每个文件均按块存储，**每个块的元数据存储在 NameNode 的内存中**（每个文件块大概占用 **150byte**），因此 HDFS 存储小文件会非常低效。因为大量的小文件会耗尽 NameNode 中的大部分内存。但注意，存储小文件所需要的磁盘容量和数据块的大小无关。例如，一个 1MB 的文件设置为 128MB 的块存储，实际使用的是 1MB 的磁盘空间，而不是 128MB。

#### 6.4.2 解决存储小文件办法之一

![[hdfs小文件归档.excalidraw|500]]
HDFS归档文件或HAR文件，是一个更高效的文件归档工具，它将文件存入HDFS块，再减少NameNode内存使用的同时，允许对文件进行透明访问。具体来说，HDFS存档文件对内还是一个一个独立文件，对NameNode而言却是一个整体，减少了NameNode的内存。

```shell
# 1.数据准备
hadoop fs -mkdir -p /input
hadoop fs -put test1.txt /input
hadoop fs -put test2.txt /input
hadoop fs -put test3.txt /input

# 2.归档文件（yarn要开启）
# 把/input 目录里面的所有文件归档成一个叫 input.har 的归档文件，并把归档后文件存储到/output 路径下。
hadoop archive -archiveName input.har -p /input /output

# 查看归档
hadoop fs -ls /output/input.har
hadoop fs -ls har:///output/input.har

# 解压归档
hadoop fs -cp har:///output/input.har/* /
```

## 七、HDFS—集群迁移

### 7.1 Apache和Apache集群间数据拷贝

#### 7.1.1 scp 实现两个远程主机之间的文件复制

```shell
# 在hadoop101创建一个文件
vim hello.txt

# 添加以下内同
hello world

# 1.推 push
# 把hadoop101的hello.txt文件推到hadoop102,用户一样可以省略（目录相同scp -r hello.txt  hadoop102:$PWD）
scp -r hello.txt hadoop@hadoop102:/home/hadoop/hello.txt

# 拉 pull
# 在hadoop103上把hadoop101的文件拉过来
scp -r hadoop@hadoop101:/home/hadoop/hello.txt hello.txt

# 是通过本地主机中转实现两个远程主机的文件复制；如果在两个远程主机之间 ssh 没有配置的情况下可以使用该方式。
# 删除hadoop103的文件，在hadoop102上，把hadoop101的文件拉到hadoop103中
scp -r hadoop@hadoop101:/home/hadoop/hello.txt hadoop@hadoop103:/home/hadoop
```

#### 7.1.1 采用 distcp 命令实现两个Hadoop集群之间的递归数据复制

不演示

```shell
hadoop distcp hdfs://hadoop102:8020/user/atguigu/hello.txt hdfs://hadoop105:8020/user/atguigu/hello.txt
```
### 7.2 Apache和CDH集群间数据拷贝

## 八、MapReduce 生产经验

### 8.1 MapReduce跑的慢的原因

1. 计算机性能
	1. cpu、内存、磁盘、网络
2. I/O操作优化
	1. 数据倾斜
	2. Map运行时间过长，导致reduce等待过久
	3. 小文件过多
### 8.2 MapReduce常用调优参数

![[mapreduce框架内部核心工作机制.excalidraw|2500]]

==mapper阶段：==
1. 自定义分区，减少数据倾斜，参见[[MapReduce学习#2.4.3 Partitioner分区]]
2. 减少溢写的次数
	- mapreduce.task.io.sort.mb（环形缓冲区大小，默认是100m，可以调高到200m）
	- mapreduce.map.sort.spil.percent（环形缓冲区溢出的阈值，默认是80%，可以调高到90%）
3. 增加每次merge合并次数
	- mapreduce.task.io.sort.factor（默认是一次处理10个溢写文件，可以调高到20）
4. 在不影响业务结果的前提下可以采用Combiner，参见[[MapReduce学习#2.4.5 Combiner合并]]
5. 为了减少磁盘IO，可以采用snappy或者LZO压缩。参见[[MapReduce学习#6.4 压缩实操案例]]
6. mapreduce.map.memory.mb 默认MapTask内存上限1024MB。
	- 可以根据128m数据对应1G内存原则提高该内存。
7. mapreduce.map.java.opts：控制MapTask堆内存大小。（如果内存不够，报：java.lang.OutOfMemoryError）
8. mapreduce.map.cpu.vcores 默认MapTask的CPU核数1。计算密集型任务可以增加CPU核数
9. 异常重试
	- mapreduce.map.maxattempts每个Map Task最大重试次数，一旦重试次数超过该值，则认为Map Task运行失败，默认值：4。根据机器性能适当提高。

==reduce阶段：==
1. mapreduce.reduce.shuffle.parallelcopies每个Reduce去Map中拉取数据的并行数，默认值是5。可以提高到10。
2. mapreduce.reduce.shuffle.input.buffer.percent 
	- Buffer大小占Reduce可用内存的比例，默认值0.7。可以提高到0.8
3. mapreduce.reduce.shuffle.merge.percent 
	- Buffer中的数据达到多少比例开始写入磁盘，默认值0.66。可以提高到0.75
4. mapreduce.reduce.memory.mb 默认ReduceTask内存上限1024MB，
	- 根据128m数据对应1G内存原则，适当提高内存到4-6G
5. mapreduce.reduce.java.opts：控制ReduceTask堆内存大小（如果内存不够，报java.lang.OutOfMemoryError）
6. mapreduce.reduce.cpu.vcores默认ReduceTask的CPU核数1个。可以提高到2-4个
7. 异常重试
	- mapreduce.reduce.maxattempts每个Reduce Task最大重试次数，一旦重试次数超过该值，则认为Map Task运行失败，默认值：4。
8. mapreduce.job.reduce.slowstart.completedmaps 当MapTask完成的比例达到该值后才会为ReduceTask申请资源。默认是0.05。
9. mapreduce.task.timeout如果一个Task在一定时间内没有任何进入，即不会读取新的数据，也没有输出数据，则认为该Task处于Block状态，可能是卡住了，也许永远会卡住，为了防止因为用户程序永远Block住不退出，则强制设置了一个该超时时间（单位毫秒），默认是600000（10分钟）。如果你的程序对每条输入数据的处理时间过长，建议将该参数调大。
10. **如果可以不用Reduce，尽可能不用**
### 8.3 MapReduce数据倾斜问题

1. 数据倾斜现象：
	- 数据频率倾斜——某一个区域的数据量要远远大于其他区域。（某个map执行很慢）
	- 数据大小倾斜——部分记录的大小远远大于平均值。（某个reduce执行很慢）

2. 减少数据倾斜的方法（针对数据大小倾斜）
	1. 首先检查是否空值过多造成的数据倾斜
	2. 能在 map 阶段提前处理，最好先在 Map 阶段处理。如：Combiner、MapJoin。参见[[MapReduce学习#5.1 编程案例——join算法]]中思路三
	3. 设置多个 reduce 个数。参见[[MapReduce学习#5.2 编程案例——数据倾斜场景]]
## 九、Hadoop-Yarn生产经验

### 9.1 常用的调优参数

参见[[Yarn学习#四、Yarn 生产环境核心参数]]
### 9.2 容量调度器使用

[[Yarn学习#5.2 容量调度器多队列提交案例]]
### 9.3 公平调度器使用

[[Yarn学习#5.3 公平调度器案例]]
## 十、Hadoop综合调优

### 10.1 Hadoop小文件优化方法

#### 10.1.1 Hadoop小文件过多弊端

1. 会**大量占用NameNode 的内存空间**。HDFS 上每个文件都要在 NameNode 上创建对应的元数据，这个元数据的大小约为150byte，这样当小文件比较多的时候，就会产生很多的元数据文件。
2. 元数据文件过多，使得**寻址索引速度变慢**。
3. 在进行 MR 计算时，**会生成过多切片，需要启动过多的 MapTask**。每个MapTask 处理的数据量小，导致 MapTask 的处理时间比启动时间还小，白白消耗资源。

#### 10.1.2 Hadoop 小文件解决方案

1. 在数据采集的时候，就将小文件或小批数据合成大文件再上传 HDFS（数据源头）
2. Hadoop Archive（存储方向）。参见[[#6.4 小文件归档]]。是一个高效的将小文件放入 HDFS 块中的文件存档工具，能够将多个小文件打包成一个 HAR 文件，从而达到减少 NameNode 的内存使用。
3. CombineTextInputFormat（计算方向）。参见[[MapReduce学习#2.4.5 Combiner合并]]。CombineTextInputFormat 用于将多个小文件在切片过程中生成一个单独的切片或者少量的切片。
4. 开启 uber 模式，实现 JVM 重用（计算方向）

默认情况下，**每个 Task 任务都需要启动一个 JVM 来运行**，==如果 Task 任务计算的数据量很小，我们可以让同一个 Job 的多个 Task 运行在一个 JVM 中==，不必为每个 Task 都开启一个 JVM。

==未开启 uber 模式==

```shell
# 未开启 uber 模式，在/input 路径上上传多个小文件并执行 wordcount 程序
hadoop jar /opt/module/hadoop-3.3.6/share/hadoop/mapreduce/hadoop-mapreduce-examples-3.3.6.jar wordcount /input /output2

# 观察控制台（uber 模式未开启）（map数量+reduce数量+am=3+1+1=5）
Job job_1695173049399_0006 running in uber mode : false
...
Launched map tasks=3
Launched reduce tasks=1
# 网页观察（开启容器5个）
```

![[mapreduce开启容器数量.excalidraw]]
==开启 uber 模式==

（1）开启 uber 模式，在 mapred-site.xml 中添加如下配置

```shell
cd /opt/module/hadoop-3.3.6/etc/hadoop/
vim mapred-site.xml
```

```xml
<!-- 开启 uber 模式，默认关闭 -->
<property>
<name>mapreduce.job.ubertask.enable</name>
<value>true</value>
</property>

<!-- uber 模式中最大的 mapTask 数量，可向下修改 -->
<property>
<name>mapreduce.job.ubertask.maxmaps</name>
<value>9</value>
</property>

<!-- uber 模式中最大的 reduce 数量，可向下修改 -->
<property>
<name>mapreduce.job.ubertask.maxreduces</name>
<value>1</value>
</property>

<!-- uber 模式中最大的输入数据量，默认使用 dfs.blocksize 的值，可向下修改 -->
<property>
<name>mapreduce.job.ubertask.maxbytes</name>
<value></value>
</property>
```

（2）分发配置

```shell
xsync mapred-site.xml 
```

（3）再次执行 wordcount 程序

```shell
# 未开启 uber 模式，在/input 路径上上传多个小文件并执行 wordcount 程序
hadoop jar /opt/module/hadoop-3.3.6/share/hadoop/mapreduce/hadoop-mapreduce-examples-3.3.6.jar wordcount /input /output3

# 观察控制台（uber 模式开启）（map数量+reduce数量+am=3+1+1=5）
Job job_1695173049399_0007 running in uber mode : true
...
Launched map tasks=3
Launched reduce tasks=1
# 网页观察（开启容器1个）
```

![[mapreduce容器数量21.excalidraw]]
### 10.2 测试MapReduce计算性能

使用 Sort 程序评测 MapReduce

==注：一个虚拟机不超过 150G 磁盘尽量不要执行这段代码==

```shell
# 1.使用 RandomWriter 来产生随机数，每个节点运行 10 个 Map 任务，每个 Map 产生大约 1G 大小的二进制随机数
hadoop jar /opt/module/hadoop-3.3.6/share/hadoop/mapreduce/hadoop-mapreduce-examples-3.3.6.jar randomwriter random-data

# 2.执行 Sort 程序
hadoop jar /opt/module/hadoop-3.3.6/share/hadoop/mapreduce/hadoop-mapreduce-examples-3.3.6.jar sort random-data sorted-data

# 3.验证数据是否真正排好序了
hadoop jar /opt/module/hadoop-3.3.6/share/hadoop/mapreduce/hadoop-mapreduce-client-jobclient-3.3.6-tests.jar testmapredsort -sortInput random-data -sortOutput sorted-data
```
### 10.3 企业开发场景实例

#### 10.3.1 需求

（1）需求：从 1G 数据中，统计每个单词出现次数。服务器 3 台，每台配置 4G 内存，4 核 CPU，4 线程。

（2）需求分析：

1G / 128m = 8 个 MapTask；1 个 ReduceTask；1 个 mrAppMaster

平均每个节点运行 10 个 / 3 台 ≈ 3 个任务（4 3 3）

#### 10.3.2 HDFS 参数调优

1. 内存设置。参见 [[#1.1.3 内存配置]]
2. namenode心跳并发配置。参见[[#1.2 NameNode心跳并发配置]]
3. 启动回收站。参见[[#1.3.1 启动回收站]]

#### 10.3.3 MapReduce 参数调优

参见[[#8.2 MapReduce常用调优参数]] 整理如下（以下参数为默认值，根据实际调整）：

```shell
cd /opt/module/hadoop-3.3.6/etc/hadoop/
vim mapred-site.xml
```

==mapper阶段参数==

```xml
<!-- 环形缓冲区大小，默认 100m -->
<property>
<name>mapreduce.task.io.sort.mb</name>
<value>100</value>
</property>

<!-- 环形缓冲区溢写阈值，默认 0.8 -->
<property>
<name>mapreduce.map.sort.spill.percent</name>
<value>0.80</value>
</property>

<!-- merge 合并次数，默认 10 个（一次处理10个文件） -->
<property>
<name>mapreduce.task.io.sort.factor</name>
<value>10</value>
</property>

<!-- maptask 内存，默认 1g； maptask 堆内存大小默认和该值大小一致mapreduce.map.java.opts -->
<property>
<name>mapreduce.map.memory.mb</name>
<value>-1</value>
</property>

<!-- matask 的 CPU 核数，默认 1 个 -->
<property>
<name>mapreduce.map.cpu.vcores</name>
<value>1</value>
</property>

<!-- matask 异常重试次数，默认 4 次 -->
<property>
<name>mapreduce.map.maxattempts</name>
<value>4</value>
</property>
```

==reduce阶段参数==

```xml
<!-- 每个 Reduce 去 Map 中拉取数据的并行数。默认值是 5 -->
<property>
<name>mapreduce.reduce.shuffle.parallelcopies</name>
<value>5</value>
</property>

<!-- Buffer 大小占 Reduce 可用内存的比例，默认值 0.7 -->
<property>
<name>mapreduce.reduce.shuffle.input.buffer.percent</name>
<value>0.70</value>
</property>

<!-- Buffer 中的数据达到多少比例开始写入磁盘，默认值 0.66。 -->
<property>
<name>mapreduce.reduce.shuffle.merge.percent</name>
<value>0.66</value>
</property>

<!-- reducetask 内存，默认 1g；reducetask 堆内存大小默认和该值大小一致mapreduce.reduce.java.opts -->
<property>
<name>mapreduce.reduce.memory.mb</name>
<value>-1</value>
</property>

<!-- reducetask 的 CPU 核数，默认 1 个 -->
<property>
<name>mapreduce.reduce.cpu.vcores</name>
<value>1</value>
</property>

<!-- reducetask 失败重试次数，默认 4 次 -->
<property>
<name>mapreduce.reduce.maxattempts</name>
<value>4</value>
</property>

<!-- 当 MapTask 完成的比例达到该值后才会为 ReduceTask 申请资源。默认是 0.05-->
<property>
<name>mapreduce.job.reduce.slowstart.completedmaps</name>
<value>0.05</value>
</property>

<!-- 如果程序在规定的默认 10 分钟内没有读到数据，将强制超时退出 -->
<property>
<name>mapreduce.task.timeout</name>
<value>600000</value>
</property>
```

#### 10.3.4 Yarn 参数调优

参见[[Yarn学习#四、Yarn 生产环境核心参数]]