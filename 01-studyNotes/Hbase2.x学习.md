---
author: wufm
title: Hbase2.x学习
rating: 7
time: 2023-10-12 周四
tags:
  - Hbase
  - 数据库
  - nosql
  - 分布式数据库系统
代码: bigdata项目的下的hbase，bigdata项目的下的hbase-phoenix
---
## 一、Hbase简介

### 1.1 概念特性

官网网址：[Apache HBase – Apache HBase™ Home](https://hbase.apache.org/)

HBASE是一个基于hdfs文件系统的分布式nosql==数据库==系统——可以提供数据的实时随机读写。

HBASE与mysql、oralce、db2、sqlserver等关系型数据库不同，==它是一个NoSQL数据库（非关系型数据库）==

![[各种数据库之间的差别比较.excalidraw|700]]

Hbase的表模型与关系型数据库的表模型不同：
1. Hbase的表没有固定的字段定义；
2. Hbase的表中==每行存储的都是一些key-value对==
3. Hbase的表中有列族的划分，用户可以指定将哪些kv插入哪个列族
4.  Hbase的表在物理存储上，是按照列族来分割的，不同列族的数据一定存储在不同的文件中
5.  **Hbase的表中的每一行都固定有一个行键，而且每一行的行键在表中不能重复**
7. Hbase中的数据，包含行键，包含key，包含value，==都是byte[ ]类型，hbase不负责为用户维护数据类型==
8.  HBASE对事务的支持很差

HBASE相比于其他nosql数据库(mongodb、redis、cassendra、hazelcast)的特点：
- Hbase的表数据存储在HDFS文件系统中

从而，hbase具备如下特性：==存储容量可以线性扩展； 数据存储的安全性可靠性极高==！
## 二、Hbase安装

见[[Hbase2.x集群安装]]
## 三、Hbase表模型

hbase的表模型跟mysql之类的关系型数据库的表模型差别巨大

hbase的表模型中有：==行的概念；但没有字段的概念==

==行中存的都是key-value对==，每行中的key-value对中的key可以是各种各样，每行中的key-value对的数量也可以是各种各样
### 3.1 Hbase表模型的要点

1. 一个表，有表名
2. 一个表可以分为多个**列族**（不同列族的数据会存储在不同文件中）
3. 表中的每一行有一个“行键rowkey”，而且行键在表中不能重复
4. 表中的每一对kv数据称作一个cell
5. hbase可以对数据存储多个历史版本（历史版本数量可配置）
6. 整张表由于数据量过大，会被横向切分成若干个region（用rowkey范围标识），不同region的数据也存储在不同文件中
7. hbase会对插入的数据按字典顺序存储：==首先会按行键排序，同一行按列族排序，再按key排序==

![[Hbase表模型.excalidraw]]

```txt
Table                     (HBase table)(表)
	Region                (Regions for the table)（区）
		Store             (Store per ColumnFamily for each Region for the table)（列族：存储区的列族）
			MemStore      (MemStore for each Store for each Region for the table)
			StoreFile     (StoreFiles for each Store for each Region for the table)
				Block     (Blocks within a StoreFile within a Store for each Region for the table)
```

可结合[[#4.1 Hbase客户端命令行操作（shell）]]来查看

操作完[[#4.3.1 建表]]后查看
![[Pasted image 20231013095619.png]]

操作完[[#4.3.2 插入/修改数据]]后查看。明明插入数据，为什么查看不到数据呢？因为数据量少的时候会存储在内存MemStore。


![[Pasted image 20231013095839.png]]

可重启hbase集群查看，可以看到列族中有数据了。因为hbase会记录修改操作在日志中，启动会遍历日志文件，把数据写入响应的表中。（也可以选择刷新数据（把内存数据刷到磁盘） flush t_user_info）

```shell
stop-hbase.sh 
start-hbase.sh 
```

![[Pasted image 20231013101052.png]]
### 3.2 Hbase的表中能存储什么数据类型

hbase中只支持byte[]

此处的byte[] 包括了： rowkey,key,value,列族名,表名

### 3.3 Hbase表的物理存储结构

```txt
/<Table>             (Tables in the cluster)(表)
	/<Region>         (Regions for the table)(区)
		/<ColumnFamily>      (ColumnFamilies for the Region for the table)(列族)
			/<StoreFi1e>     (StoreFiles for the ColumnFamily for the Regions for the table)(文件)
```

![[Pasted image 20231013095619.png]]
## 四、Hbase客户端操作

==实际开发中使用 shell 的机会不多，所有丰富的使用方法到 API 中介绍。==
### 4.1 Hbase客户端命令行操作（shell）

```shell
# 查看所有的命令，输错了也会显示
help

# 查看具体某个命令
help 'create'
```
#### 4.3.1 建表

```shell
# create 表名,列族名,列族名...
create 't_user_info','base_info','extra_info'

# 查看表信息
desc "t_user_info"

# 建立一个命名空间（相当于数据库），并再该库建立一张student表
create_namespace 'my_test'
create 'my_test:student','cf1','cf2'
```
#### 4.3.2 插入/修改数据

```shell
# put 表,行键,列族:列,值
put 't_user_info','001','base_info:username','zhangsan'
put 't_user_info','001','base_info:age','18'
put 't_user_info','001','base_info:sex','female'
put 't_user_info','001','extra_info:career','it'
put 't_user_info','002','extra_info:career','actoress'
put 't_user_info','002','base_info:username','liuyifei'

put 'my_test:student','001','cf1:name','hainiu01'
put 'my_test:student','001','cf1:age',20
put 'my_test:student','001','cf2:gender','male' 
put 'my_test:student','002','cf1:province','beijing' 
put 'my_test:student','002','cf1:city','beijing' 
put 'my_test:student','002','cf2:gender','male'
put 'my_test:student','002','cf1:age',30
put 'my_test:student','003','cf2:gender','male'
put 'my_test:student','003','cf1:age',40
```
#### 4.3.3 查询数据

##### 4.3.1 scan 扫描

```shell
scan 't_user_info'
```

![[Pasted image 20231013102530.png]]

```shell
# 展示两行
scan 'my_test:student', {LIMIT => 2}

# 指定展示某列
scan 'my_test:student',{COLUMNS=>'cf1:age'}
```

过滤查询：

```shell
# 过滤器查询。查询值等于20的数据
scan 'my_test:student', FILTER=>"ValueFilter(=,'binary:20')"

# 指定列等值查询。查询cf1列族age列不等于20的数据
scan 'my_test:student',{COLUMNS=>'cf1:age',FILTER=>"ValueFilter(!=,'binary:20')"}

# 范围查询
scan 'my_test:student', { STARTROW => '001', STOPROW => '003'}
```

![[Pasted image 20231019155121.png]]

![[Pasted image 20231019155108.png]]

![[Pasted image 20231019155450.png]]

##### 4.3.2 get 单行数据

```shell
get 't_user_info','001'

# 获取某个行键某个列族
# get 't_user_info','001','base_info'
```

![[Pasted image 20231013102729.png]]

多版本查询

```shell
#修改设置版本，查询时，加上版本就可以查出来版本 
alter 'my_test:student',{ NAME =>'cf1', VERSIONS => 2 }

put 'my_test:student','id10', 'cf1:name','name10a'
put 'my_test:student','id10', 'cf1:name','name10aa'
put 'my_test:student','id10', 'cf1:name','name10aaa'

#此时，可以查询出2个版本的数据 
get 'my_test:student', 'id10', { COLUMN =>'cf1:name', VERSIONS => 2}
```

![[Pasted image 20231019161249.png]]
##### 4.3.3 查询行数

```shell
# 查询表的行数 # 语法：count <table>, {INTERVAL => intervalNum, CACHE => cacheNum} 
# INTERVAL设置多少行显示一次及对应的rowkey，默认1000； 
# CACHE每次去取的缓存区大小，默认是10，调整该参数可提高查询速度 
count 'my_test:student' 
# 直接查询完毕返回值 
count 'my_test:student', {INTERVAL => 2,CACHE=>50} # 间隔两秒返回一次结果值
```

大表统计的时候不能使用hbase自带的count命令，这样hbase压力太大

```shell
# 我们可以通过外置的mr进行计算统计大小 
hbase org.apache.hadoop.hbase.mapreduce.RowCounter 'my_test:student'
```

![[Pasted image 20231020104443.png]]
#### 4.3.4 删除

（1）删除一个kv数据

```shell
delete 't_user_info','001','base_info:username'
```

（2）删除一行数据

```shell
deleteall 't_user_info','001'
```

（3）删除整张表

```shell
# 禁用表
disable 't_user_info'
# 再删除表
drop 't_user_info'
# 查看所有表
list
```

### 4.2 Hbase客户端API操作（java）

hbase的DDL操作：com.bigdata.hbase.HbaseClientDDLMain

hbase的DML操作：com.bigdata.hbase.HbaseClientDMLMain

![[Pasted image 20231019180620.png|600]]
## 五、Hbase运行原理

### 5.1 组件结构图


![[Pasted image 20231019213541.png|650]]
#### 5.1.1 Zookeeper集群所起作用

- 存放整个HBase集群的元数据以及集群的状态信息。
- 实现HMaster主从节点的failover。

```txt
HMaster通过监听ZooKeeper中的Ephemeral节点(默认：/hbase/rs/*)来监控HRegionServer的加入和宕机。

在第一个HMaster连接到ZooKeeper时会创建Ephemeral节点(默认：/hbasae/master)来表示Active的HMaster，其后加进来的HMaster则监听该Ephemeral节点

如果当前Active的HMaster宕机，则该节点消失，因而其他HMaster得到通知，而将自身转换成Active的HMaster，在变为Active的HMaster之前，它会在/hbase/masters/下创建自己的Ephemeral节点
```

以下均打开zookeeper客户端查询：
##### 5.1.1.1 实现HMaster主从节点的failover

```shell
# 1.查看有哪些HRegionServer
ls /hbase/rs
# 结果
[hadoop101,16020,1697162984446, hadoop102,16020,1697162984507, hadoop103,16020,1697162984571]

# 2.查看active的HMaster
get /hbase/master

# 结果
�master:16000�S�Z)=�PBUF

        hadoop101�}�鯶�1�}

# 3.查看stanby的HMaster
ls /hbase/backup-masters

# 结果
[hadoop102,16000,1697162986498, hadoop103,16000,1697181100334]

# 4.删掉hadoop101的hmaster kill -9
# 再次查看
get /hbase/master

# 结果
�master:16000|���>}ӼPBUF

        hadoop103�}�ҁ��1�}

# 5.查看stanby的HMaster
ls /hbase/backup-masters
[hadoop102,16000,1697162986498]
```
##### 5.1.1.2 保存meta表所在位置

![[Pasted image 20231013153724.png|400]]

![[Pasted image 20231013153739.png|400]]

```shell
# 查看元数据所在服务器
get /hbase/meta-region-server

# 结果
�master:16000�`�1ו<FPBUF

        hadoop103�}�����1
```

与上图结果一致
#### 5.1.2 master职责

hmaster一般都是两台机器，使用zookeeper进行管理和协调

- 管理表操作（Admin职能：ddl操作），如：create、alter、drop；
- 管理HRegionServer的负载均衡，调整region分布；
- region split后，负责新region重分布；
- 在HRegionServer停机后，负责失效的HRegionServer上region的迁移；
#### 5.1.3 region server职责

- ==管理自己所负责的region数据的读写==
-  读写HDFS，管理Table中的数据
- Client直接通过HRegionServer读写数据（先去zk查找meta表所在的region server，再去meta表查找自己所要的数据RowKey所在的region server，最后去regeion server上要自己的数据）（meta表所在位置见[[#5.2.2 保存系统的元数据表]]）
#### 5.1.4 Region

一个表会按照rowkey的范围进行行级别的分割，分割出来的一个部分就叫做region，它是表的一部分数据，可以分散到不同的regionserver中进行管理，是一个表的最小负载均衡的单位  

==每个region都会记录自己的startkey和endkey的范围==
#### 5.1.5 Store

每一个列族对应一个Store，一个Region里包含一个或者多个Store。
#### 5.1.6 Hlog

hbase WAL(write ahead log)，在用户发起写请求时先向Hlog写一份，然后再将数据向memstore中写，Hlog数据是写磁盘，为了避免HRegionServer故障时memstore数据丢失，Hlog滚动更新，新数据会加入会对应冲抵掉较早的Hlog数据。
#### 5.1.7 Memstore

hbase写缓存，在用户发起写请求时先写入hlog，然后再写入memstore中，当memstore写入达到flush阈值时，将memstore中的数据写到hdfs上(hfile)，==每个列族对应一个memstore==，即一个HStore/Store中只有一个memstore。
#### 5.1.8 storefile

当memstore写数据达到设定的阈值之后，会将数据溢写到hdfs，即storefile，==内部存储hfile==。storefile会进行合并，当storefile经过多次合并后变得已经达到指定规则的分裂阈值，则再进行region分裂。
### 5.2 hbase的写数据流程

![[Hbase写数据流程.excalidraw|700]]

注意说明：

（1）去zk查找meta表所在的region server，见[[#5.1.1.2 保存meta表所在位置]]]

（3）去meta表查找自己所要的数据RowKey所在的region server

```shell
scan 'hbase:meta'
```

![[Pasted image 20231019223200.png|775]]

（3）将操作写入WAL日志

```shell
# 查看日志文件内容
hbase wal /hbase/WALs/hadoop102,16020,1697692552239/hadoop102%2C16020%2C1697692552239.1697721373655 -j
```

![[Pasted image 20231019220634.png|400]]


### 5.3 hbase的读数据流程

![[Hbase读数据流程.excalidraw|700]]


（1）StoreFile内容

![[Pasted image 20231020101126.png]]

==还可以通过startkey endkey跳过HFIle，通过bloom过滤器实现跳过，如果真的存在就可以直接通过索引文件==

```shell
# flush表的数据，清空memstore ,写入hdfs
flush t_user_info
```

```shell
# 查看hfile存储了什么信息
# -m 打印元数据信息 -b 打印块信息 -p 打印数据内容 -f 后面接文件
hbase hfile -m -b -p -f /hbase/data/default/t_user_info/5296508fedf620fc82f4203fa4e60fc1/base_info/998d24772578461a9d4a405eb3ca08a2
```


一般是包冲突：

```txt
2023-10-20T09:39:50,679 WARN  [main] util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
2023-10-20T09:39:52,037 INFO  [main] metrics.MetricRegistries: Loaded MetricRegistries class org.apache.hadoop.hbase.metrics.impl.MetricRegistriesImpl
Exception in thread "main" java.lang.NoSuchMethodError: com.codahale.metrics.MetricRegistry.histogram(Ljava/lang/String;Lcom/codahale/metrics/MetricRegistry$MetricSupplier;)Lcom/codahale/metrics/Histogram;
```

```shell
# 把其中一个包改下名字
cd /opt/module/hbase-2.5.5/lib
mv metrics-core-3.0.1.jar metrics-core-3.0.1.jar.bak
```

数据内容：

![[Pasted image 20231020104716.png]]

布隆过滤器：

![[Pasted image 20231020104801.png|275]]

开始结束中将行键：

![[Pasted image 20231020104843.png|400]]

![[Pasted image 20231020104901.png|400]]

布隆过滤器：

一个使用hash表作为计算规则的过滤器，也就是在写入数据到hbase的时候要先将数据写出到memstore中，在memstore写满了以后就会将数据以storeFile形式写出到磁盘上，这个时候也会生成一份对应数据的hash表文件，以metablock的形式存储到起来，它的功能非常实用，==比如我们在查询数据的时候就可以首先将这个数据进行hash处理，然后和hash表进行比对，如果不存在可以直接避免扫描这个storeFile文件==。在巨大的数据面前可以进行高效的数据

blockCache：

对应表数据的regionserver级别的缓存组件，主要使用规则就是在查询数据的时候也会将查询结果缓存到regionserver对应的blockCache组件中，下次查询的时候可以直接使用上次查询的结果，blockCache中存储的数据内存包含索引文件，布隆过滤器的值和数据的key，其他的value数据，会以64Kb为大小进行存储，如果数据过期了先清理value的数据，而索引等数据和元数据信息不会清理出去


![[Pasted image 20231020102153.png|875]]

### 5.4 hbase性能保证

#### 5.4.1 写入数据的性能

首先我们使用memstore进行缓存，不会将数据直接放入到hdfs中，插入完毕数据可以直接返回成功，可以根据zookeeper直接定位到meta表，根据meta表可以直接找到数据所对应的regionserver，表的管理部分，每个表拆分为不同的region区域，每个区域会交给不同的regionserver进行管理，分布式管理均衡压力，并且每个列族都会有一个单独的组件store进行管理

首先存数据放入到memstore中，这个数据在内存中仅仅是追加形式的，不需要任何计算

#### 5.4.2 写入数据的性能

与写出数据相比，寻址也是比较方便的，而且还可以在客户端选择性存储

memstore在写入数据到达阈值以后，将数据存储到hdfs中，这个文件叫做hfile，这个文件在存储的时候，需要做三件事情，==按照rowkey进行排序，这样可以生成索引文件LSM Tree 【日志合并树】，生成相应的bloom过滤器的文件存储==

读取数据的时候memstore>blockCache>Hfile,如果真的找到HFile还可以通过startkey endkey跳过HFIle，通过bloom过滤器实现跳过，如果真的存在就可以直接通过索引文件

blockCache regionserver级别的缓存，读取进来的时候会加载所有的HFile的元数据信息，以及查询到的block【64KB】数据，每次查询都可以直接从缓存中读取数据，免去多次查询，缓存会失效，每次查询并不是直接使用blockCache中的数据，拿到blockCache的元数据和Hfile中的数据进行比对，失效策略是LRU机制
## 六、region server 内部机制

### 6.1  memstore的刷写

参考官网（MemStore Flush）：[Apache HBase ™ Reference Guide](https://hbase.apache.org/book.html#quickstart)

MemStore刷新可以在下面列出的任何条件下触发。最小刷新单位是每个区域，而不是单个MemStore级别。

（1）当某个region的所有MemStore的大小总和超过了 hbase.hregion.memstore.flush.size 的大小，默认128MB，此时当前的region中所有的MemStore会Flush到HDFS中。

（2）当一个region server所有MemStore的大小超过了 hbase.regionserver.global.memstore.upperLimit 的大小，默认40％的内存使用量（占堆内存）。此时当前HRegionServer中所有HRegion中的MemStore都会Flush到HDFS中，Flush顺序是MemStore大小的倒序（一个HRegion中所有MemStore总和），直到总体的MemStore使用量低于 hbase.regionserver.global.memstore.lowerLimit，默认38%的内存使用量。

（3）当一个region server 的wal log数量超过 base.regionserver.max.logs，当前region server中所有Region中的MemStore都会Flush到HDFS中，Flush刷写按时间顺序，最早的MemStore先Flush，直到WAL的数量少于 base.regionserver.max.logs下（该属性名已经废弃，现无需手动设置，最大值为 32）。

（4）到达自动刷写的时间，也会触发memstore flush。自动刷新的时间间隔由该属性进行配置hbase.regionserver.optionalcacheflushinterval（默认1小时，-1 代表不自动刷写）。
### 6.2 storeFile的合并

由于 memstore 每次刷写都会生成一个新的 HFile，文件过多读取不方便，所以会进行文件的合并，清理掉过期和删除的数据，会进行 StoreFile Compaction。

- Minor Compaction：将临近的若干个较小的 HFile 合并成一个较大的 HFile，并==清理掉部分==过期和删除的数据
- Major Compaction：将一个 Store 下的所有的 HFile 合并成一个大 HFile，并且会==清理掉所有==过期和删除的数据，由参数 hbase.hregion.majorcompaction控制，默认 7 天。

![[Pasted image 20231020115757.png|400]]

注：Compaction的触发时机Major Compaction时间会持续比较长，整个过程会消耗大量系统资源，对上层业务有比较大的影响。因此线上业务都会将==关闭自动触发Major Compaction功能，改为手动在业务低峰期触发==。

HBase中可以触发compaction的因素有很多，最常见的因素有这么三种：Memstore Flush、后台线程周期性检查、手动触发。

**1）Memstore Flush：**

每当 RegionServer发生一次Memstore flush操作之后也会进行检查是否需要进行Compaction操作。

**2）周期性检查：**

通过CompactionChecker线程来定时检查是否需要执行compaction（RegionServer启动时在initializeThreads()中初始化），每隔10000毫秒（可配置）检查一次。

**一般系统触发都是minor Compact**，合并主要有以下几个参数进行配置

```txt
hbase.store.compaction.ratio默认值1.2f，大于最小值但是小于1.2倍数的大小的数据也参加合并 
hbase.hstore.compaction.min 默认值2，每次合并最少两个hfile 
hbase.hstore.compaction.max 默认值10，每次合并最多10个hfile 
hbase.hstore.compaction.min.size 默认 128M 小于这个值的file肯定会参加合并 
hbase.hstore.compaction.max.size 默认 Long.MAX_VALUE 大于这个值的肯定不会参加合并
```

**3）手动触发：**

​ 自动major compaction影响读写性能，因此会选择低峰期手动触发，执行命令 major_compact '表名'

​ 执行完alter操作之后希望立刻生效，执行手动触发major compaction；

```shell
create 't_test', 'info'
put 't_test','001', 'info:name','zhangsan'

# 时间2023-10-20T12:44:07.233 对应 1697777047233
scan 't_test'

# 数据刷新
flush 't_test'

put 't_test','001', 'info:name','lisi'

# 时间timestamp=2023-10-20T12:45:05.672
scan 't_test'

# 数据刷新
flush 't_test'

# 数据查询
get 't_test', '001', {TIMESTAMP => 1697777047233}
```

![[Pasted image 20231020125017.png]]

![[Pasted image 20231020125045.png|700]]

```shell
# 执行major合并, 由于n1是历史版本，所以n1被合并没了， 只留下n2(最新版本数据) 
major_compact 't_test'

# 数据查询
get 't_test', '001', {TIMESTAMP => 1697777047233}
```

![[Pasted image 20231020125214.png|400]]

![[Pasted image 20231020125304.png]]

### 6.3 region split机制

Region 切分分为两种，创建表格时候的预分区即自定义分区，同时系统默认还会启动一个切分规则，避免单个 Region 中的数据量太大。
#### 6.3.1 系统拆分

​ HRegionServer拆分region的步骤是，先将该region下线，然后拆分，将其子region加入到hbase:meta表中，再将他们加入到原本的HRegionServer中，最后汇报Master。

可以通过设置RegionSplitPolicy的实现类来指定拆分策略，RegionSplitPolicy类的实现类有：

```java
ConstantSizeRegionSplitPolicy 
	IncreasingToUpperBoundRegionSplitPolicy
		DelimitedKeyPrefixRegionSplitPolicy 
		KeyPrefixRegionSplitPolicy
DisabledRegionSplitPolicy // 不拆分
```

其中：

**ConstantSizeRegionSplitPolicy：(一刀切)【0.94前】**

​ 当一个region中最大store大小大于设置阈值（hbase.hregion.max.filesize 默认10G）就会触发切分，每10s检查一次region大小，hbase.server.thread.wakefrequency=10000。

弊端：设置阈值大些，对大表友好，但对小表并不友好，可能小表不会分裂； 如果阈值小些，对小表友好，但对大表并不友好，可能会大量分裂；

**IncreasingToUpperBoundRegionSplitPolicy【0.94-2.0】：**

默认使用的拆分策略，Region的前几次拆分的阈值不是固定的数值，是需要进行计算得到，当同一table在同一regionserver上的region数量在[0,100)之间时按照如下的计算公式算，否则按照ConstantSizeRegionSplitPolicy策略计算：

Min (R^3 "hbase.hregion.memstore.flush.size"2, "hbase.hregion.max.filesize")

- R为同一个table中在同一个regionserver中region的个数
- hbase.hregion.memstore.flush.size默认为128M
- hbase.hregion.max.filesize默认为10G

第一次分裂： `1^3 * 128 * 2` = 256M

第二次分裂：`2^3 * 128 * 2` = 2G

第三次分裂： `3^3 * 128 * 2` = 6.75G

**SteppingSplitPolicy【2.x版本】：**

​ 这种策略和IncreasingToUpperBoundRegionSplitPolicy策略很相似，但更简单，第一个Region容量的上限为256M，之后都是10G，这个策略考虑到IncreasingToUpperBoundRegionSplitPolicy会多拆分几个Region(256M -> 2G -> 6.75G -> 10G)，所以进行了简化。
##### 6.3.1.2 预分区（自定义分区）

为了解决数据的倾斜问题，或者数据在刚开始插入的数据都在一个region中，使得一个region中的压力太大，我们可以预先设定一个表数据的分区范围，让数据更加均匀的分布在不同的分区中，或者我们在做数据分类的时候可以按照不同的类别将数据放入到不同的region中，扫描数据的时候会比较容易，防止跨多个分区进行操作查询

==预分region需要考虑两个因素，即region个数与region大小。==

- **region个数**

官方推荐region个数计算公式：

```txt
(RS Xmx * hbase.regionserver.global.memstore.size) / (hbase.hregion.memstore.flush.size * column familys)

RS Xmx：regionserver堆栈内存大小，官方推荐每台regionserver内存大小设置20-24G，不推荐设置更大，因为更大的堆栈内存GC效率较低。
hbase.regionserver.global.memstore.size：为整个regionserver中memstore总大小占用总内存的比例，一般默认为0.4
hbase.hregion.memstore.flush.size：为memstoreflush阈值，一般默认128，可以自己设置
column familys：为列族数

例：(20G*0.4)/(128M*2)=32

官方推荐每个regionserver上region个数在20-200之间。
```

- **region大小**

单个region官方推荐大小为5-10GB，可以通过hbase.hregion.max.filesize设置，当超过该值后会触发split，与region split策略相关。

1）手动设定预分区

```shell
create 'staff1','info', SPLITS => ['1000','2000','3000','4000']
```

2）生成 16 进制序列预分区

```shell
create 'staff2','info',{NUMREGIONS => 15, SPLITALGO =>'HexStringSplit'}
```

3）按照文件中设置的规则预分区

比如我们做人口普查，需要将不同省份的数据放入到不同的region中 河北省,山西省,吉林省,辽宁省,黑龙江省,陕西省,甘肃省,青海省,山东省,福建省,浙江省,台湾省,河南省,湖北省,湖南省,江西省,江苏省,安徽省,广东省,海南省,四川省,贵州省,云南省 

==首先我们按照这些省份的字典顺序将字母排序（可用集合sort排序）==

创建 splits.txt 文件内容如下：

```shell
cd /home/hadoop
vim splits.txt
# 添加如下内容
```

```txt
云南省
台湾省
吉林省
四川省
安徽省
山东省
山西省
广东省
江苏省
江西省
河北省
河南省
浙江省
海南省
湖北省
湖南省
甘肃省
福建省
贵州省
辽宁省
陕西省
青海省
黑龙江省
```

```shell
# 然后将这些数据放入到一个文件中 /home/hadoop/split.txt 
create 'advance_split_region', 'cf', {SPLITS_FILE => '/home/hadoop/splits.txt'}
```

![[Pasted image 20231020133433.png|900]]

4）使用 JavaAPI 创建预分区

```java
// 7.创建对应的切分

byte[][] splits = new byte[3][];
splits[0] = Bytes.toBytes("aaa");
splits[1] = Bytes.toBytes("bbb");
splits[2] = Bytes.toBytes("ccc");

// 8.创建表
admin.createTable(builder.build(),splits);
```
### 6.4 hbase的压缩

建表时指定压缩格式，开启压缩后可以非常有效的缓解hbase数据膨胀问题。

（1）建表时指定压缩格式

```shell
create 'flow',{NAME => 'cf',VERSIONS => 3,COMPRESSION => 'SNAPPY'}
```

（1）建表时没有指定压缩格式

可以通过以下命令查看压缩前后的大小

```shell
hdfs dfs -du -s -h /hbase/data/default/user_info
```


```shell
# 如果表的数据量很大，region很多，disable过程会比较缓慢，需要等待较长时间。过程可以通过查看hbase master log日志监控。
disable 'user_info'

# NAME即列族。HBase修改压缩格式，需要一个列族一个列族的修改。名字一定要与你自己列族的名字一致，否则就会创建一个新的列族并且压缩格式是snappy的。
alter 'user_info', {NAME => 'base_info', COMPRESSION => 'snappy'}
alter 'user_info', {NAME => 'extra_info', COMPRESSION => 'snappy'}

# 重新enable表
enable 'user_info'

# enable表后，HBase表的压缩格式并没有生效，还需要执行一个命令，major_compact
# Major compact除了做文件Merge操作，还会将其中的delete项删除。
major_compact 'user_info'
```

![[Pasted image 20231020212736.png|400]]

执行 `alter 'user_info', {NAME => 'base_info', COMPRESSION => 'snappy'}` 报如下错误：

```txt
ERROR: org.apache.hadoop.hbase.DoNotRetryIOException: Compression algorithm 'snappy' previously failed test. Set hbase.table.sanity.checks to false at conf or table descriptor if you want to bypass sanity checks
```

```shell
cd /opt/module/hbase-2.5.5/conf
# 指定snappy包
vim  hbase-site.xml
# 新增如下内容
```

```xml
<property>
    <name>hbase.io.compress.snappy.codec</name>
    <value>org.apache.hadoop.hbase.io.compress.xerial.SnappyCodec</value>
</property>
```

```shell
# 分发脚本
xsync hbase-site.xml
# 重启hbase
```

## 七、整合Phoenix

### 7.1 Phoenix概述

官网：[Overview | Apache Phoenix](https://phoenix.apache.org/)

Phoenix 是 HBase 的开源 SQL 皮肤。可以使用标准 JDBC API 代替 HBase 客户端 API来创建表，插入数据和查询 HBase 数据。

官方给的解释为：==在 Client 和 HBase 之间放一个 Phoenix 中间层不会减慢速度==，==因为用户编写的数据处理代码和 Phoenix 编写的没有区别==（更不用说你写的垃圾的多），不仅如此Phoenix 对于用户输入的 SQL 同样会有大量的优化手段（就像 hive 自带 sql 优化器一样）。
### 7.2 Phoenix安装

参见[[Phoenix2.x安装]]

### 7.3 Phoenix客户端命令行操作（Shell）

官网：[Grammar | Apache Phoenix](https://phoenix.apache.org/language/index.html)

6.2.2.1 table
#### 7.3.1 表的操作

##### 7.3.1.1 创建表 

在phoenix中创建测试表，==必须指定主键，主键对应hbase的rowkey==。

==phoenix严格区分大小写，所有小写在phoenix中都会被翻译为大写==，若要小写，使用双引号，如"us_population"。

注：Phoenix 中建表，会在 HBase 中创建一张对应的表。为了减少数据对磁盘空间的占用，Phoenix 默认会对 HBase 中的列名做编码处理。具体规则可参考官网链接：[Storage Formats | Apache Phoenix](https://phoenix.apache.org/columnencoding.html)  ，若不想对列名编码，可在建表语句末尾加上 COLUMN_ENCODED_BYTES = 0;

```sql
-- 表名不带双引号，默认转成大写 
create table phtest1(
pk varchar not null primary key
, col1 varchar
, col2 varchar
, col3 varchar ); 

-- 表名带双引号，不转大写 
create table "phtest2"( 
pk varchar not null primary key
, col1 varchar
, col2 varchar
, col3 varchar ); 

-- 查看表列表 
!tables

-- 查看表结构 
!desc PHTEST1
!describe "phtest2"
```

![[Pasted image 20231023094405.png]]

在hbase shell中查询(==phoenix严格区分大小写，所有小写在phoenix中都会被翻译为大写==)

![[Pasted image 20231023094436.png|1025]]
##### 7.3.1.2 插入/查询数据

```sql
-- upsert插入时如果主键已经存在则更新，如果不存在则插入。 
-- 数据插入与hbase shell插入数据性质一致，如果插入相同主键的值，则保持最新的一条数据。 
-- 直接 values 插入或更新 
upsert into PHTEST1 values ('x0001','1','2','3'); 
upsert into PHTEST1 values ('x0001','1','22','3'); 
upsert into PHTEST1 values ('x0002','1','2','3'); 
-- 指定字段插入更新 
upsert into PHTEST1 (pk,col1) values ('x0003','1'); 

-- 查询 
select * from PHTEST1; 
select col1 from PHTEST1; 
select t.col1 from PHTEST1 t;
```

![[Pasted image 20231023094906.png]]

![[Pasted image 20231023095038.png]]
##### 7.3.1.3 删除

插入多行，删除其中某一行

```sql
-- 删除一行 
delete from PHTEST1 where col2='2'; 

-- 查询验证 
select * from PHTEST1;
```

![[Pasted image 20231023095512.png]]
##### 7.3.1.4 查询导入

使用select查询结果集批量更新表 

```sql

upsert into "phtest2" values ('x0001','newvalue','newvalue','newvalue'); 
upsert into "phtest2" values ('x0002','newvalue','newvalue','newvalue'); 
upsert into "phtest2" values ('x0003','3','4','5'); 
upsert into "phtest2" values ('x0004','4','5','6'); 
upsert into "phtest2" values ('x0005','newvalue','newvalue','newvalue'); 
upsert into "phtest2" values ('x0006','newvalue','newvalue','newvalue'); 

-- 执行批量更新, 将"phtest2"表的数据覆盖到PHTEST1表 
upsert into PHTEST1 select * from "phtest2";

-- 查询验证 
select * from PHTEST1;
```

![[Pasted image 20231023095931.png]]
##### 7.3.1.5 删除表

```sql
drop table "phtest2";
```
##### 7.3.1.6 统计查询

```sql
select count(1) from PHTEST1; 
select count(distinct col1) from PHTEST1; 
select sum(num) from (select col1, count(*) as num from PHTEST1 group by col1) t1; 
select col1, count(*) as num from PHTEST1 group by col1 order by num desc;
```
##### 7.3.1.7 数据导入

（1）执行sql文件

```shell
# 在客户端外,执行SQL文件 
# 对标hive的-f test.sql ${hiveconf:batch_date} 

# 创建sql文件
cd /home/hadoop
vim test.sql

# 添加如下内容
select * from PHTEST1

sqlline.py hadoop101,hadoop102,hadoop103:2181 /home/hadoop/test.sql
```

![[Pasted image 20231023101052.png]]

（2）数据导入

注意：
1. phoenix数据导入只支持后缀为.csv的文件
2. 指定的表必须是大写，小写就报错

```sql
-- 创建表 
create table user(id varchar primary key,name varchar,age varchar); 

```

```shell
# 创建csv文件 /root/user.csv 
cd /home/hadoop
vim user.csv 

# 添加如下内容
1,zhangsan,20
2,lisi,30 

psql.py -t USER hadoop101,hadoop102,hadoop103:2181 /home/hadoop/user.csv 
```

```sql
select * from USER;
```

![[Pasted image 20231023101445.png]]

##### 7.3.1.8 在phoenix建表时指定列族

```sql
-- 用 列族名.字段名 
create table "cftest" ( 
pk varchar not null primary key
, cf1.col1 varchar
, cf2.col2 varchar); 

-- 查询时可以不用列族 
select col1 from "cftest";
```

![[Pasted image 20231023101750.png]]
##### 7.3.1.9 在phoenix建表时指定压缩格式

```sql
-- 在后面可指定压缩格式 
create table "comptest" ( 
pk varchar not null primary key
, cf1.col1 varchar
, cf2.col2 varchar) 
compression='snappy';
```

![[Pasted image 20231023102124.png]]

##### 7.3.1.10 在phoenix建表时预分region

```sql
-- 用 split on ('x0001','x0002','x0003','x0004','x0005') 来进行预分region 
-- 其中 on 里面的 是 splitkey 
create table "split_region_test" ( 
pk varchar not null primary key
, cf1.col1 varchar
, cf2.col2 varchar) 
compression='snappy' 
split on ('x0001','x0002','x0003','x0004','x0005');
```

![[Pasted image 20231023102415.png]]
##### 7.3.1.11 phoenix与hbase表关联

(1）在hbase中创建带有命名空间的表，并添加数据

```sql

```


##### 7.3.1.12 phoenix建表时指定组合rowkey

```sql
-- 通过 CONSTRAINT pk primary key ( prefix,id ) 设定联合主键，作为rowkey 
-- 当prefix和id作为联合主键， 只在hbase的rowkey中存在， column里没有 
-- 建表语句 
create table "mytest"."combinationkey_table1" ( 
prefix varchar not null
, id varchar not null
, col1 varchar
, col2 varchar 
CONSTRAINT pk primary key ( prefix,id ) 
) column_encoded_bytes=0, compression='snappy' split on ('1','2','|'); 

-- 插入数据 
upsert into "mytest"."combinationkey_table1" (prefix,id,col1,col2) values ('1','001','user1','20'); 
upsert into "mytest"."combinationkey_table1" (prefix,id,col1,col2) values ('1','002','user2','21'); 

-- 查看表结构 
!describe "mytest"."combinationkey_table1"

select * from "mytest"."combinationkey_table1";
```

![[Pasted image 20231023112238.png]]

##### 7.3.1.13 phoenix实现动态列

```sql
-- 创建表 
create table "mytest"."dynamic_table1"( 
pk varchar not null primary key
, col1 varchar
, col2 varchar 
)column_encoded_bytes=0; 

-- 插入数据 
upsert into "mytest"."dynamic_table1" (pk,col1,col2) values ('x0001','user1','20'); 
upsert into "mytest"."dynamic_table1" (pk,col1,col2) values ('x0002','user1','21'); 
upsert into "mytest"."dynamic_table1" (pk,col1,col2) values ('x0003','user1','22'); 
upsert into "mytest"."dynamic_table1" (pk,col1,col2) values ('x0004','user1','23'); 

-- 动态插入列
upsert into "mytest"."dynamic_table1" (pk,col1,col2,col3 varchar,col4 varchar) values ('x0005','user1','23','beijing','hainiu'); 
upsert into "mytest"."dynamic_table1" (pk,col1,col2,col4 varchar,col5 varchar) values ('x0006','user2','32','huawei','30K'); 
upsert into "mytest"."dynamic_table1" (pk,col1,col2,col3 varchar,col4 varchar,col5 varchar) values ('x0007','user3','33','shanghai','ali','22K'); 
upsert into "mytest"."dynamic_table1" (pk,col1,col2,col3 varchar,col4 varchar,col5 varchar,col6 varchar) values ('x0008','user4','35','shanghai','baidu','12K','false'); 

-- phoenix中查询动态列 
select * from "mytest"."dynamic_table1"(col3 varchar,col4 varchar); 
select * from "mytest"."dynamic_table1"(col3 varchar,col4 varchar,col5 varchar) ; 
select * from "mytest"."dynamic_table1"(col3 varchar,col4 varchar,col5 varchar,col6 varchar) ;
```

#### 7.3.2 表的映射

默认情况下，HBase 中已存在的表，通过 Phoenix 是不可见的。如果要在 Phoenix 中操作 HBase 中已存在的表，可以在 Phoenix 中进行表的映射。映射方式有两种：视图映射和表映射

（1）视图映射

==Phoenix 创建的视图是只读的，所以只能用来做查询==，无法通过视图对数据进行修改等操作。在 phoenix 中创建关联 test 表的视图

在hbase建立表：

```shell
create 'test','info1','info2'
```

创建视图：

```sql
create view "test"(
id varchar primary key
, "info1"."name" varchar
, "info2"."address" varchar
);
```

删除视图：

```sql
drop view "test";
```

（2）表映射

在 Pheonix 创建表去映射 HBase 中已经存在的表，是可以修改删除 HBase 中已经存在的数据的。而且，删除 Phoenix 中的表，那么 HBase 中被映射的表也会被删除。

==注：进行表映射时，不能使用列名编码，需将 column_encoded_bytes 设为 0==

```shell
create 'mytest:relatetable_1',{NAME => 'cf1',COMPRESSION => 'snappy'},{NAME => 'cf2',COMPRESSION => 'snappy'} 

# 添加数据 
put 'mytest:relatetable_1','x0001','cf1:name','user1' 
put 'mytest:relatetable_1','x0002','cf1:name','user2' 
put 'mytest:relatetable_1','x0001','cf1:age','20' 
put 'mytest:relatetable_1','x0002','cf1:age','21' 
put 'mytest:relatetable_1','x0001','cf2:address','beijing' 
put 'mytest:relatetable_1','x0002','cf2:address','shanghai'
```

```sql
-- 在phoenix创建'hainiu:relatetable'的关联表 
-- 也可以在phoenix中创建schema，对应hbase的namespace create schema if not exists "mytest";
-- 其中： column_encoded_bytes=0 是把字段名转成字符串，而不是原来的byte数组 
create table "mytest"."relatetable_1"( 
id varchar not null primary key
, "cf1"."name" varchar
, "cf1"."age" varchar
, "cf2"."address" varchar 
) column_encoded_bytes=0;


select * from "mytest"."relatetable_1";
```

![[Pasted image 20231023105137.png]]

==数字类型说明==

HBase 中的数字，底层存储为补码，而 Phoenix 中的数字，底层存储为在补码的基础上，将符号位反转。故当在 Phoenix 中建表去映射 HBase 中已存在的表，当 HBase 中有数字类型的字段时，会出现解析错误的现象。

```shell
# Hbase 演示：
create 'test_number','info'
put 'test_number','1001','info:number',Bytes.toBytes(1000)
scan 'test_number',{COLUMNS => 'info:number:toLong'}
```

```sql
-- phoenix 演示：
create view "test_number"(
id varchar primary key
,"info"."number" bigint
);

select * from "test_number";
```

![[Pasted image 20231023122109.png]]

解决上述问题的方案有以下两种：

（1）Phoenix 种提供了 unsigned_int，unsigned_long 等无符号类型，其对数字的编码解码方式和 HBase 是相同的，如果无需考虑负数，那在 Phoenix 中建表时采用无符号类型是最合适的选择。

```sql
drop view "test_number";
create view "test_number"(
id varchar primary key
,"info"."number" unsigned_long);

select * from "test_number";
```

![[Pasted image 20231023122322.png]]

（2）如需考虑负数的情况，则可通过 Phoenix 自定义函数，将数字类型的最高位，即符号位反转即可，自定义函数可参考如下链接：[User-defined functions(UDFs) | Apache Phoenix](https://phoenix.apache.org/udf.html)。

### 7.4 Phoenix二级索引

（1）建表

```sql
drop table if exists students;
create table students( 
pk varchar not null primary key
, name varchar 
, age bigint 
, addr varchar 
)column_encoded_bytes=0;
```

（2）数据准备

```shell
cd /home/hadoop

vim students.sh

# 添加如下内容
#! /bin/bash 
for((i=1;i<=20000;i++)) 
	do 
		echo "upsert into student values ('x${i}','zhangsan${i}',${i},'北京${i}');" >> students.sql 
	done

# 执行脚本
sh students.sh

# 执行SQL文件导入表
sqlline.py hadoop101,hadoop102,hadoop103:2181 students.sql
```

（3）索引开启前查询

```sql
-- 查看执行计划，发现全表扫描 
explain select count(1) from students where name = 'zhangsan2000'; 
```

通过执行计划可以发现，查询为FULL SCAN。

![[Pasted image 20231023133943.png]]

#### 7.4.1 全局索引

Global Index 是默认的索引格式，创建全局索引时，会在 HBase 中建立一张新表。也就是说索引数据和数据表是存放在不同的表中的，==因此全局索引适用于多读少写的业务场景==。写数据的时候会消耗大量开销，因为索引表也要更新，而索引表是分布在不同的数据节点上的，跨节点的数据传输带来了较大的性能消耗

在读数据的时候 Phoenix 会选择索引表来降低查询消耗的时间。

```sql
-- 基于 COL1字段 创建索引， 当创建完后，索引里存的是已经排序好的COL1数据 
-- local index 适用于写操作频繁的场景。索引数据和数据表的数据是存放在相同的服务器中的，避免了在写操作的时候往不同服务器的索引表中写索引带来的额外开销 
create index my_index1 on students(name); 

-- 查看执行计划，发现不全表扫描 
explain select count(1) from students where name = 'zhangsan2000'; 

-- 删除索引 
drop index my_index1 on students;

```

![[Pasted image 20231023125950.png|1050]]

​ 通过执行计划可以发现，查询为RANGE SCAN（有二级索引之后会变成范围扫描）。

![[Pasted image 20231023133857.png]]

#### 7.4.2 包含索引

创建携带其他字段的全局索引（本质还是全局索引）。

```sql
-- 先删除之前的索引
drop index my_index1 on students;

-- 创建索引
create index my_index1 on students(name) include (age);

-- 查看执行计划
explain select count(*) from students where name = 'zhangsan2000' and age=20000; 
```

![[Pasted image 20231023134426.png]]

![[Pasted image 20231023134744.png]]
#### 7.4.3 本地索引

==Local Index 适用于写操作频繁的场景==。索引数据和数据表的数据是存放在同一张表中（且是同一个 Region），避免了在写操作
的时候往不同服务器的索引表中写索引带来的额外开销

本地索引会将所有的信息存在一个影子列族中，虽然读取的时候也是范围扫描，但是没有全局索引快，优点在于不用写多个表了

```sql
-- 删除之前的索引
drop index my_index1 on students;

create local index my_index1 ON students (name,age);

explain select * from students where name = 'zhangsan2000' and age=20000; 
```

![[Pasted image 20231023135634.png]]
### 7.5 Phoenix客户端API操作（java）

com.bigdata.phoenix.PhoenixClient

## 八、与Hive的集成

如果大量的数据已经存放在 HBase 上面，需要对已经存在的数据进行数据分析处理，那么 Phoenix 并不适合做特别复杂的 SQL 处理，此时可以使用 hive 映射 HBase 的表格，之后写 HQL 进行分析处理

```shell
cd /opt/module/hive-3.1.3/conf
vim hive-site.xml 

# 添加如下参数
```

```xml
<property>
	<name>hive.zookeeper.quorum</name>
	<value>hadoop101,hadoop102,hadoop103</value>
</property>
<property>
	<name>hive.zookeeper.client.port</name>
	<value>2181</value>
</property>
```

### 8.1 创建hive的内部表

建立 Hive 表，关联 HBase 表，插入数据到 Hive 表的同时能够影响 HBase 表。

分步实现（提示：完成之后，可以分别进入 Hive 和 HBase 查看，都生成了对应的表。）：

```sql
create table student_hive(
id int
,name string
,age int
)STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler' 
WITH SERDEPROPERTIES ("hbase.columns.mapping" = ":key,info:name,info:age") 
TBLPROPERTIES ("hbase.table.name" = "student_hbase");
```

![[Pasted image 20231023175604.png]]

![[Pasted image 20231023175541.png|1050]]
### 8.2  创建hive的外部表

有的时候在hbase中已经存在一个表并且其中存在数据，我们需要使用hive进行分析，那么我们就需要创建一个外部表进行映射
删除表，因为hive对应的是外部表所以hbase的表不会被删除掉

首先在hbase中创建表：

```shell
create 'student_hbase_2','info' 
# 增加数据 
put 'student_hbase_2','1','info:name','zhangsan' 
put 'student_hbase_2','1','info:age','20' 
put 'student_hbase_2','2','info:name','lisi' 
put 'student_hbase_2','2','info:age','30'
```

创建外部表进行映射

```sql
create external table student_hive_2(
id int
,name string
,age int
) STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler' 
WITH SERDEPROPERTIES ("hbase.columns.mapping" = ":key,info:name,info:age") 
TBLPROPERTIES ("hbase.table.name" = "student_hbase_2");
```

![[Pasted image 20231023180131.png]]
## 九、批量数据如何快速导入Hbase

在大数据的场景计算中，有时候我们会遇见将大量数据一次性导入到hbase的情况，但是这个时候hbase是不能够容纳的：
1. 首先会进入到memstore中如果大量插入数据会==造成memstore的内存压力急剧增大==
2. hbase在大量插入数据的时候首先这个region会急剧增加，后续region会按照拆分策略进行region拆分，当前region下线，插入程序会直接卡死造成hbase宕机等严重问题，

为了解决这个问题，hbase给用户提供了一种新的插入数据的方式bulkload方式，这个方式中会跳过hbase本身的过程，首先在使用hbase的提供的mapreduce程序按照插入数据的格式和hbase的表格式生成hfile文件，然后我们==将hfile文件一次性插入到hbase对应的hdfs的文件夹中==，这种方式是最快捷并且对于hbase的压力最小的方式

```shell
# 首先在本地创建文件a.txt 输入以下内容 
cd /home/hadoop
vim people.txt

# 添加如下内容
1,zhangsan,20 
2,lisi,30 
3,wangwu,40 
5 zhaosi,50 

# 然后将数据上传到hdfs中 
hdfs dfs -put people.txt / 
# 在hbase中创建表 
create 'people','info' 

# 然后将id当成是rowkey，info:name存放名称 info:age存放年龄
```

```shell
# -Dimporttsv.separator ：指定分隔符
# -Dimporttsv.columns ：指定列映射  HBASE_ROW_KEY强制要求写 cf:pk指定rowkey字段 其他字段与hive表中对应
# -Dimporttsv.skip.bad.lines：是否跳过无效行
# -Dimporttsv.bulk.output：hfile输出路径
# hbase表名 用于生成hfile文件的输入目录
hbase org.apache.hadoop.hbase.mapreduce.ImportTsv \
-Dimporttsv.separator="," \
-Dimporttsv.columns=HBASE_ROW_KEY,info:name,info:age \
-Dimporttsv.skip.bad.lines=false \
default:people hdfs://hadoop101:8020/people.txt
```

![[Pasted image 20231023182624.png]]
## 十、Hbase的性能调优

### 10.1 RowKey 设计

一条数据的唯一标识就是 rowkey，那么这条数据存储于哪个分区，取决于 rowkey 处于哪个一个预分区的区间内，设计 rowkey的主要目的 ，就是让数据均匀的分布于所有的 region中，在一定程度上防止数据倾斜。接下来我们就谈一谈 rowkey 常用的设计方案。

1）生成随机数、hash、散列值

2）时间戳反转

3）字符串拼接

### 10.2 hbase的参数优化

Lars Hofhansl（拉斯·霍夫汉斯）大神推荐 Region 设置 20G，刷写大小设置 128M，其它默认

1）Zookeeper 会话超时时间

```txt
属性：zookeeper.session.timeout
解释：默认值为 90000 毫秒（90s）。当某个 RegionServer 挂掉，90s 之后 Master 才能察觉到。可适当减小此值，尽可能快地检测 regionserver 故障，可调整至 20-30s。看你能有都能忍耐超时，同时可以调整重试时间和重试次数

hbase.client.pause（默认值 100ms）
hbase.client.retries.number（默认 15 次）
```

2）设定线程数量

```txt
属性：hbase.regionserver.handler.count
解释：默认值为 30，用于指定 RPC 监听的数量，可以根据客户端的请求数进行调整，读写请求较多时，增加此值。
```

3）手动控制 Major Compaction

```txt
属性：hbase.hregion.majorcompaction
解释：默认值：604800000 秒（7 天）， Major Compaction 的周期，若关闭自动 MajorCompaction，可将其设为 0。如果关闭一定记得自己手动合并，因为大合并非常有意义
```

4）优化 HStore 文件大小

```txt
属性：hbase.hregion.max.filesize
解释：默认值 10737418240（10GB），如果需要运行 HBase 的 MR 任务，可以减小此值，
因为一个 region 对应一个 map 任务，如果单个 region 过大，会导致 map 任务执行时间过长。该值的意思就是，如果 HFile 的大小达到这个数值，则这个 region 会被切分为两个 Hfile。
```

5）优化 HBase 客户端缓存

```txt
属性：hbase.client.write.buffer
解释：默认值 2097152bytes（2M）用于指定 HBase 客户端缓存，增大该值可以减少 RPC调用次数，但是会消耗更多内存，反之则反之。一般我们需要设定一定的缓存大小，以达到减少 RPC 次数的目的。
```

6）提交读取缓存大小

```txt
属性：hfile.block.cache.size
解释：BlockCache 占用 RegionServer 堆内存的比例，默认 0.2，读请求比较多的情况下，可适当调大
```

7）设定内存大小

```txt
属性：hbase.regionserver.global.memstore.size
解释：MemStore 占用 RegionServer 堆内存的比例，默认 0.4，写请求较多的情况下，可适当调大
```
### 10.3 JVM 调优

JVM 调优的思路有两部分：一是内存设置，二是垃圾回收器设置。

垃圾回收的修改是使用并发垃圾回收，默认 PO+PS 是并行垃圾回收，会有大量的暂停。理由是 HBsae 大量使用内存用于存储数据，容易遭遇数据洪峰造成 OOM，同时写缓存的数据是不能垃圾回收的，==主要回收的就是读缓存==，而读缓存垃圾回收不影响性能，所以最终设置的效果可以总结为：防患于未然，早洗早轻松。

1）设置使用 CMS 收集器：

```shell
-XX:+UseConcMarkSweepGC
```

2）保持新生代尽量小，同时尽早开启 GC，例如：

```shell
# 在内存占用到 70%的时候开启 GC
-XX:CMSInitiatingOccupancyFraction=70

# 指定使用 70%，不让 JVM 动态调整
-XX:+UseCMSInitiatingOccupancyOnly

# 新生代内存设置为 512m
-Xmn512m

# 并行执行新生代垃圾回收
-XX:+UseParNewGC

# 设 置 scanner 扫 描 结 果 占 用 内 存 大 小 ， 在 hbase-site.xml 中，设置
hbase.client.scanner.max.result.size(默认值为 2M)为 eden 空间的 1/8（大概在 64M）

# 设置多个与 max.result.size * handler.count 相乘的结果小于 Survivor
Space(新生代经过垃圾回收之后存活的对象)
```

