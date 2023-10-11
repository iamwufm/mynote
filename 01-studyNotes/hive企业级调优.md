---
author: wufm
title: hive企业级调优
rating: 6
time: 2023-10-10 周二
tags:
  - Hive
  - hive调优
代码:
---

## 一、计算资源配置

本教程的计算环境为Hive on MR。计算资源的调整主要包括Yarn和MR。
### 1.1 Yarn资源配置

需要调整的Yarn参数均与CPU、内存等资源有关，核心配置参数如下

参见[[Hadoop生产调优手册#10.3.4 Yarn参数调优]]
### 1.2 MapReduce资源配置

MapReduce资源配置主要包括Map Task的内存和CPU核数，以及Reduce Task的内存和CPU核数。核心配置参数如下：

参见[[Hadoop生产调优手册#10.3.3 MapReduce参数调优]]

在hive中，可直接使用如下方式为每个SQL语句单独进行配置

```sql
-- 单个Map Task申请的container容器内存大小
set mapreduce.map.memory.mb=2048;
-- 单个Reduce Task申请的container容器内存大小
set mapreduce.reduce.memory.mb=2048;
```

默认值为1024。以上值不能超出yarn.scheduler.maximum-allocation-mb和yarn.scheduler.minimum-allocation-mb

```sql
-- 单个Map Task申请的container容器cpu核数
set mapreduce.map.cpu.vcores;
-- 单个Reduce Task申请的container容器cpu核数
set mapreduce.reduce.cpu.vcores;
```

默认值为1。该值一般无需调整。

## 二、资源配置和测试用表

### 2.1 资源配置

未开虚拟机，电脑可用内存剩10G多一点。

三台虚拟机，每台配置4G内存，集群也会用掉一些内存，每台nodemanager最多配置可用内存3G。按照mapreduce容器默认配置，最多同时可以启动3+3+1=7个任务（am容器需要1.5G内存）。

```shell
cd /opt/module/hadoop-3.3.6/etc/hadoop
vim yarn-site.xml
```

```xml
<!-- NodeManager 使用内存-->
<property>
	<name>yarn.nodemanager.resource.memory-mb</name>
	<value>3072</value>
</property>
```

```shell
# 分发给其他机器
xsync yarn-site.xml
```
### 2.2 订单表(600w条数据)

```sql
-- 建表
drop table if exists order_detail;
create table order_detail(
id    string   comment '订单id',
user_id   string   comment '用户id',
product_id  string   comment '商品id',
province_id  string   comment '省份id',
create_time  string   comment '下单时间',
product_num  int    comment '商品件数',
total_amount decimal(16,2) comment '下单金额'
)
row format delimited fields terminated by ',';

load data local inpath '/home/hadoop/hivedata/order_detail.dat' overwrite into table order_detail;

-- 查询数据
select * from order_detail limit 5;
```

| id       | user_id   | product_id | province_id | create_time         | product_num | total_amount |
| -------- | --------- | ---------- | ----------- | ------------------- | ----------- | ------------ |
| 10000000 | 180233413 | 1902463    | 1           | 2020-06-14 16:05:56 | 10          | 837.92       |
| 10000001 | 127273788 | 1078945    | 1           | 2020-06-14 04:13:40 | 5           | 532.89       |
| 10000002 | 183739423 | 1634802    | 1           | 2020-06-14 13:55:20 | 1           | 520.72       |
| 10000003 | 146341900 | 1971115    | 1           | 2020-06-14 09:23:47 | 2           | 371.84       |
| 10000004 | 183831802 | 1592533    | 1           | 2020-06-14 06:07:50 | 7           | 759.35       |
### 2.3 支付表(240w条数据)

```sql
-- 建表
drop table if exists payment_detail;
create table payment_detail(
id              string comment '支付id',
order_detail_id string comment '订单明细id',
user_id         string comment '用户id',
payment_time    string comment '支付时间',
total_amount    decimal(16, 2) comment '支付金额'
)
row format delimited fields terminated by ',';

--导入数据
load data local inpath '/home/hadoop/hivedata/payment_detail.dat' overwrite into table payment_detail;

-- 查询数据
select * from payment_detail limit 5;
```

| id       | order_detail_id | user_id   | payment_time        | total_amount | dt         |
| -------- | --------------- | --------- | ------------------- | ------------ | ---------- |
| 10000000 | 16874642        | 113382690 | 2020-06-14 09:15:40 | 985.38       | 2020-06-14 |
| 10000001 | 17403042        | 131508758 | 2020-06-14 13:55:44 | 391.72       | 2020-06-14 |
| 10000002 | 19198884        | 133018075 | 2020-06-14 08:46:23 | 657.1        | 2020-06-14 |
| 10000003 | 12853558        | 173966667 | 2020-06-14 04:35:01 | 707.12       | 2020-06-14 |
| 10000004 | 12066072        | 138996167 | 2020-06-14 05:56:00 | 228.91       | 2020-06-14 |
### 2.4 商品信息表(100w条数据)

```sql
--建表
drop table if exists product_info;
create table product_info(
id           string comment '商品id',
product_name string comment '商品名称',
price        decimal(16, 2) comment '价格',
category_id  string comment '分类id'
)
row format delimited fields terminated by '\t';

--导入数据
load data local inpath '/home/hadoop/hivedata/product_info.txt' overwrite into table product_info;

-- 查询数据
select * from product_info limit 5;
```

```txt
+------------------+----------------------------+---------------------+---------------------------+
| product_info.id  | product_info.product_name  | product_info.price  | product_info.category_id  |
+------------------+----------------------------+---------------------+---------------------------+
| 1000000          | XMkwvr                     | 8682.00             | 383                       |
| 1000001          | CuisW                      | 4517.00             | 219                       |
| 1000002          | TBtbp                      | 9357.00             | 208                       |
| 1000003          | FDWvNH                     | 4916.00             | 649                       |
| 1000004          | poQvdu                     | 9305.00             | 659                       |
+------------------+----------------------------+---------------------+---------------------------+
```
### 2.5 省份信息表(34条数据)

```sql
-- 建表
drop table if exists province_info;
create table province_info(
id            string comment '省份id',
province_name string comment '省份名称'
)
row format delimited fields terminated by '\t';

--导入数据
load data local inpath '/home/hadoop/hivedata/province_info.txt' overwrite into table province_info;

-- 查询数据
select * from province_info limit 5;
```

```txt
+-------------------+------------------------------+
| province_info.id  | province_info.province_name  |
+-------------------+------------------------------+
| 1                 | 北京                           |
| 2                 | 天津                           |
| 3                 | 山西                           |
| 4                 | 内蒙古                          |
| 5                 | 河北                           |
+-------------------+------------------------------+
```

### 2.6 表大小


```sql
-- 可使用如下语句获取表/分区的大小信息,totalSize（单位为Byte）
desc formatted table_name partition(partition_col='partition');

-- 查看有多少块
hdfs fsck hdfs文件路径 -blocks -locations

desc formatted order_detail;
desc formatted payment_detail;
desc formatted product_info;
desc formatted province_info;

hdfs fsck /user/hive/warehouse/my_test.db/order_detail -blocks -locations
```

```txt
# 默认块大小是128M
表名大小       大小
order_detail  347942702（约332M）3个块
payment_detail  140414783（约134M）2个块
product_info  25285707（约24M）1个块
province_info 369(约0.36K) 1个块
```
## 三、 explain查看执行计划（重点）

见[[Hive学习#七、explain查看执行计划（重点）]]

## 四、hql语法优化—分组聚合优化

Hive 中未经优化的分组聚合，是通过一个 MapReduce Job 实现的。Map 端负责读取数据，并按照分组字段分区，通过 Shuffle，将数据发往 Reduce 端，各组数据在 Reduce 端完成最终的聚合运算。

Hive 对分组聚合的优化主要围绕着减少 Shuffle 数据量进行，具体做法是 map-side 聚合。

所谓 map-side 聚合，就是在 Map 端维护一个 hash table，利用其完成部分的聚合，然后将部分聚合的结果，按照分组字段分区，发送至 Reduce 端，完成最终的聚合。map-side 聚合能有效减少 Shuffle 的数据量，提高分组聚合运算的效率

map-side 聚合相关的参数如下（都是默认参数）：

```sql
-- 启用 map-side 聚合（默认开启） 
set hive.map.aggr=true; 
-- 用于检测源表数据是否适合进行 map-side 聚合。检测的方法如下： 
-- 先对若干条数据进行 map-side 聚合，若聚合后的条数和聚合前的条数比值小于该值，则认为该表适合进行 map-side 聚合； 
-- 否则，认为该表数据不适合进行 map-side 聚合，后续数据便不再进行 map-side 聚合。 
set hive.map.aggr.hash.min.reduction=0.5; 
-- 用于检测源表是否适合 map-side 聚合的条数 
set hive.groupby.mapaggr.checkinterval=100000; 
-- map-side 聚合所用的 hash table 占用 map task 堆内存的最大比例，若超出该值则会对 hash table 进行一次 flush。 
set hive.map.aggr.hash.force.flush.memory.threshold=0.9;
```

### 4.1 分组聚合优化前

```sql
--关闭启用map-side聚合（默认开启） 
set hive.map.aggr=false; 

-- 执行计划
explain
select province_id,count(*) from order_detail group by province_id; 

-- 执行  Time taken: 30.79 seconds
select province_id,count(*) from order_detail group by province_id; 
```

```txt
....
INFO  : MapReduce Jobs Launched: 
INFO  : Stage-Stage-1: Map: 2  Reduce: 2   Cumulative CPU: 36.86 sec   HDFS Read: 347974013 HDFS Write: 786 SUCCESS
INFO  : Total MapReduce CPU Time Spent: 36 seconds 860 msec
INFO  : Completed executing command(queryId=hadoop_20231010160250_3b52ab3a-8a16-4fd4-b860-60c7e2aa3ccb); Time taken: 30.79 seconds
```

==order_detail的有3块，为什么map task数量只有两个（Map: 2 ）？== 

因为==输入数据接口的实现类是小文件合并的CombineHiveInputFormat==（见[[MapReduce学习#4.1.1 读数据：InputFormat(输入数据接口)]]），切片最大值设置是256M，order_detail（约332M）三个块，大小分别是128M，128M和76M，分成2个切片（128+ 128，76 或 128+76，128）。我们可以把切片大小设置为块大小

```sql
-- 输入数据接口实现类。Hive默认已经做了输入合并
set hive.input.format;

+----------------------------------------------------+
|                        set                         |
+----------------------------------------------------+
| hive.input.format=org.apache.hadoop.hive.ql.io.CombineHiveInputFormat |
+----------------------------------------------------+

-- CombineFileInputFormat类重要的三个参数
-- 切片大小最大值，默认256M
-- 相当于 CombineFileInputFormat.setMaxInputSplitSize()方法进行设置
set mapreduce.input.fileinputformat.split.maxsize;

-- 同一节点的数据块形成切片时，切片大小的最小值
--  相当于 CombineFileInputFormat.setMinSplitSizeNode()
set mapreduce.input.fileinputformat.split.minsize.per.node;

-- 同一机架的数据块形成切片时，切片大小的最小值
-- 相当于 CombineFileInputFormat.setMinSplitSizeRack()
set mapreduce.input.fileinputformat.split.minsize.per.rack;
```

切片大小设置为块大小

```sql
-- 切片大小设置为块大小
set mapreduce.input.fileinputformat.split.maxsize=128000000;
```

再次执行，maptask就是3个了。

```txt
...
INFO  : MapReduce Jobs Launched: 
INFO  : Stage-Stage-1: Map: 3  Reduce: 2   Cumulative CPU: 77.23 sec   HDFS Read: 347988123 HDFS Write: 786 SUCCESS
INFO  : Total MapReduce CPU Time Spent: 1 minutes 17 seconds 230 msec
INFO  : Completed executing command(queryId=hadoop_20231010160021_81571da1-6831-4042-82aa-54abfa298ded); Time taken: 33.016 seconds
INFO  : OK
INFO  : Concurrency mode is disabled, not creating a lock manager
```
### 4.2 分组聚合优化后

```sql
--关闭启用map-side聚合（默认开启） 
set hive.map.aggr=true; 

-- 执行计划
explain
select province_id,count(*) from order_detail group by province_id; 

-- 执行  Time taken: 24.583 second
select province_id,count(*) from order_detail group by province_id; 

-- 让 Hive 强制使用 Map 分组聚合
-- set hive.map.aggr.hash.min.reduction=1;
```

```txt
...
INFO  : Ended Job = job_1696921920082_0008
INFO  : MapReduce Jobs Launched: 
INFO  : Stage-Stage-1: Map: 3  Reduce: 2   Cumulative CPU: 66.07 sec   HDFS Read: 347995341 HDFS Write: 786 SUCCESS
INFO  : Total MapReduce CPU Time Spent: 1 minutes 6 seconds 70 msec
INFO  : Completed executing command(queryId=hadoop_20231010155807_101ff046-6dce-4ab8-92e3-982a9d93994d); Time taken: 27.295 seconds
INFO  : OK
INFO  : Concurrency mode is disabled, not creating a lock manager
```

注意，看 MapTask 日志的时候有可能会出现 Map output records 大于分组字段的基数的情况。

这和参数 `hive.map.aggr.hash.force.flush.memory.threshold` 有关。map-side 聚合所用的 hash table 占用 map task 堆内存的最大比例，若超出该值则会对 hash table 进行一次 flush。比如说，flush 之前聚合过 user_id=100000000 的数据了，flush 完后，又统计到 prodcut_id=1101 的数据。可以把 map 内存调大点，flush 的次数就少点了。
### 4.2 分组聚合优化前后对比

执行时间对比：

优化前：Time taken: 33.016 seconds

优化后：Time taken: 27.295 seconds

执行计划对比：

![[1104875-20230729235339299-1700592333.png|350]]

map端输入和输出对比：

![[Pasted image 20231010153712.png|400]]

![[Pasted image 20231010153937.png|400]]

## 五、hql语法优化—join优化

### 5.1 join算法概述

hive拥有多种join算法，包括common join，map join，bucket map join，sort merge buckt map join等，下面对每种join算法做简要说明：
#### 5.1.1 common join

Common Join是Hive中最稳定的join算法，其通过一个MapReduce Job完成一个join操作。Map端负责读取join操作所需表的数据，并按照关联字段进行分区，通过Shuffle，将其发送到Reduce端，相同key的数据在Reduce端完成最终的Join操作。

![[Pasted image 20231009184350.png|400]]

需要注意的是，sql语句中的join操作和执行计划中的Common Join任务并非一对一的关系，==一个sql语句中的相邻的且关联字段相同的多个join操作可以合并为一个Common Join任务。==

```sql
select a.val, b.val, c.val 
from a 
join b on (a.key = b.key1) 
join c on (c.key = b.key1)
```

上述sql语句中两个join操作的关联字段均为b表的key1字段，则该语句中的两个join操作可由一个Common Join任务实现，也就是可通过一个Map Reduce任务实现。

```sql
select a.val, b.val, c.val 
from a 
join b on (a.key = b.key1) 
join c on (c.key = b.key2)
```

上述sql语句中的两个join操作关联字段各不相同，则该语句的两个join操作需要各自通过一个Common Join任务实现，也就是通过两个Map Reduce任务实现。
#### 5.1.2 map join

Map Join算法可以通过两个只有map阶段的Job完成一个join操作。==其适用场景为大表join小表==。若某join操作满足要求，则第一个Job会==读取小表数据，将其制作为hash table，并上传至Hadoop分布式缓存==（本质上是上传至HDFS）。第二个Job会先从分布式缓存中读取小表数据，并缓存在Map Task的内存中，然后扫描大表数据，这样在map端即可完成关联操作。如下图所示：

![[Pasted image 20231009190550.png|400]]

#### 5.1.3 bucket map join

Bucket Map Join是对Map Join算法的改进，其打破了Map Join只适用于大表join小表的限制，可用于==大表join大表==的场景。

Bucket Map Join的核心思想是：若能保证==参与join的表均为分桶表，且关联字段为分桶字段，且其中一张表的分桶数量是另外一张表分桶数量的整数倍==，就能保证参与join的两张表的分桶之间具有明确的关联关系，所以就可以在两表的分桶间进行Map Join操作了。这样一来，第二个Job的Map端就无需再缓存小表的全表数据了，而只需缓存其所需的分桶即可。其原理如图所示：

![[Pasted image 20231009190851.png|575]]

#### 5.1.4 sort merge bucket map join

Sort Merge Bucket Map Join（简称SMB Map Join）基于Bucket Map Join。SMB Map Join要求，==参与join的表均为分桶表，且需保证分桶内的数据是有序的，且分桶字段、排序字段和关联字段为相同字段，且其中一张表的分桶数量是另外一张表分桶数量的整数倍。==

SMB Map Join同Bucket Join一样，同样是利用两表各分桶之间的关联关系，在分桶之间进行join操作，不同的是，分桶之间的join操作的实现原理。Bucket Map Join，两个分桶之间的join实现原理为Hash Join算法；而SMB Map Join，两个分桶之间的join实现原理为Sort Merge Join算法。

Hash Join和Sort Merge Join均为关系型数据库中常见的Join实现算法。Hash Join的原理相对简单，就是对参与join的一张表构建hash table，然后扫描另外一张表，然后进行逐行匹配。Sort Merge Join需要在两张按照关联字段排好序的表中进行，其原理如图所示：

![[Pasted image 20231009191342.png|550]]

Hive 中的 SMB Map Join 就是对两个分桶的数据按照上述思路进行 join 操作的。可以看出 SMB Map Join 与 Bucket Map Join 相比，在进行 join 操作时，Map 端是无须对整个 Bucket 构建 hash table，也无须在 Map 端缓存整个 Bucket 数据的，==每个 Mapper 只需要按顺序逐个 key 读取两个分桶的数据进行 join 即可==。
### 5.2 map join优化

map join有两种触发方式，一种是用户在SQL语句中增加hint提示，另外一种是Hive优化器根据参与join表的数据量大小，自动触发。

用户可通过如下方式，指定通过map join算法，并且ta将作为map join中的小表。这种方式已经过时，不推荐使用。

```sql
select /*+ mapjoin(ta) */
ta.id,
tb.id
from table_a ta
join table_b tb
on ta.id=tb.id;
```

自动触发：

```sql
--启动Map Join自动转换，默认开启
set hive.auto.convert.join=true;

-- 有条件转Map Join时的小表之和阈值
-- 一个Common Join Operator转为Map Join Operator的判断条件，若该Common Join相关的表中，存在N-1张表的已知大小总和<=该值，则生成一个Map Join Task，此时可能存在多种N-1张表的组合均满足该条件，则Hive会为每种满足条件的组合均生成一个Map Join Task，同时还会保留原有的Common Join Task作为BackUp Task，实际运行时优先执行Map Join Task，若不能执行成功，则启动BackUp Task。
-- 小表的最大文件大小，默认是25M
set hive.mapjoin.smalltable.filesize=25000000;

-- 开启'无条件转Map Join'
--是否将多个map join转成一个map join,默认开启
set hive.auto.convert.join.noconditionaltask=true;

-- 无条件转Map Join时的小表之和阈值
-- 多个mapjoin转换为1个时，所有小表的文件大小总和的最大值
-- 默认是10M
set hive.auto.convert.join.noconditionaltask.size=10000000;
```

==测试sql：==

```sql
select od.user_id,od.product_id,od.province_id,product.product_name,province.province_name
from order_detail od
join product_info product on od.product_id = product.id
join province_info province on od.province_id = province.id
limit 10;
```
#### 5.2.1 map join 优化前

```sql
-- 关闭mapjoin
set hive.auto.convert.join=false;

-- 执行计划
explain
select od.user_id,od.product_id,od.province_id,product.product_name,province.province_name
from order_detail od
join product_info product on od.product_id = product.id
join province_info province on od.province_id = province.id
limit 10;
```

上述SQL语句共有三张表进行两次join操作，且两次==join操作的关联字段不同==。故优化前的执行计划应该包含两个Common Join operator，也就是由两个MapReduce任务实现执行计划如下图所示：

![[1104875-20230812114247859-418740039.png|450]]


```sql
select od.user_id,od.product_id,od.province_id,product.product_name,province.province_name
from order_detail od
join product_info product on od.product_id = product.id
join province_info province on od.province_id = province.id
limit 10;
```

```txt
...
INFO  : MapReduce Jobs Launched: 
INFO  : Stage-Stage-1: Map: 3  Reduce: 2   Cumulative CPU: 96.35 sec   HDFS Read: 373267542 HDFS Write: 267056511 SUCCESS
INFO  : Stage-Stage-2: Map: 2  Reduce: 2   Cumulative CPU: 59.38 sec   HDFS Read: 267079099 HDFS Write: 1298 SUCCESS
INFO  : Total MapReduce CPU Time Spent: 2 minutes 35 seconds 730 msec
INFO  : Completed executing command(queryId=hadoop_20231010161739_6b87fe71-42ad-4213-be3e-8bbe62d35fc7); Time taken: 104.238 seconds
```
#### 5.2.2 map join 优化后

==注：map join 比较耗内存，在资源不足的情况下，优化后比优化前执行的时间更长，也是有可能的。==

三张表中，product_info（约24M）和province_info(约0.36K)数据量较小，可考虑将其作为小表，进行Map Join优化。

==方案一：将两个 Common Join operator 均可转为 Map Join operator，并保留 Common Join 作为后备计划，保证计算任务的稳定==

```sql
-- 启用Map Join自动转换。
set hive.auto.convert.join=true;
-- 有条件转Map Join时的小表之和阈值
-- 小表的最大文件大小
-- 调整hive.mapjoin.smalltable.filesize参数，使其大于等于product_info。默认值25000000
set hive.mapjoin.smalltable.filesize=25285707;

-- 开启'无条件转Map Join'
-- 是否将多个map join转成一个map join,默认开启
set hive.auto.convert.join.noconditionaltask=false;

-- 执行计划
explain
select od.user_id,od.product_id,od.province_id,product.product_name,province.province_name
from order_detail od
join product_info product on od.product_id = product.id
join province_info province on od.province_id = province.id
limit 10;
```

调整完的执行计划如下图：

![[1104875-20230729235858549-354775374.png]]

```sql
select od.user_id,od.product_id,od.province_id,product.product_name,province.province_name
from order_detail od
join product_info product on od.product_id = product.id
join province_info province on od.province_id = province.id
limit 10;
```

执行计划一共有5个job（mapreduce程序）。最后选择执行计划Stage-8和Stage-5执行。

```txt
...
INFO  : MapReduce Jobs Launched: 
INFO  : Stage-Stage-8: Map: 2   Cumulative CPU: 603.8 sec   HDFS Read: 347972595 HDFS Write: 267056511 SUCCESS
INFO  : Stage-Stage-5: Map: 1   Cumulative CPU: 4.29 sec   HDFS Read: 406401 HDFS Write: 608 SUCCESS
INFO  : Total MapReduce CPU Time Spent: 10 minutes 8 seconds 90 msec
INFO  : Completed executing command(queryId=hadoop_20231010162247_56291ad5-5c79-4952-a8d9-6d4d45aa12cf); Time taken: 90.899 seconds
```

==方案二：将两个 Map Join operator 任务可合并为同一个。这个方案计算效率最高，但需要的内存也是最多的。==

```sql
-- 启用Map Join自动转换。
set hive.auto.convert.join=true;

-- 开启'无条件转Map Join'
-- 是否将多个map join转成一个map join,默认开启
set hive.auto.convert.join.noconditionaltask=true;

-- 无条件转Map Join时的小表之和阈值
-- 多个mapjoin转换为1个时，所有小表的文件大小总和的最大值
-- 调整size使其 >= (product_info.size + province_info.size) 默认是10000000
set hive.auto.convert.join.noconditionaltask.size=25286076;

-- 执行计划
explain
select od.user_id,od.product_id,od.province_id,product.product_name,province.province_name
from order_detail od
join product_info product on od.product_id = product.id
join province_info province on od.province_id = province.id
limit 10;
```

![[1104875-20230812114438761-833595408.png]]

```sql
select od.user_id,od.product_id,od.province_id,product.product_name,province.province_name
from order_detail od
join product_info product on od.product_id = product.id
join province_info province on od.province_id = province.id
limit 10;
```

执行计划一共有1个job（mapreduce程序）。执行计划Stage-5。执行时间长是因为内存不足。

```txt
...
INFO  : MapReduce Jobs Launched: 
INFO  : Stage-Stage-5: Map: 2   Cumulative CPU: 46.58 sec   HDFS Read: 988527 HDFS Write: 1206 SUCCESS
INFO  : Total MapReduce CPU Time Spent: 46 seconds 580 msec
INFO  : Completed executing command(queryId=hadoop_20231010162818_067d02ef-df74-4eb4-b6a7-959969e8d288); Time taken: 31.98 seconds
```

==方案三：将两个 Common Join operator 转为 Map Join operator，但不会将两个 Map Join 的任务合并。该方案计算效率比方案二低，但需要的内存也更少==

```sql
-- 启用Map Join自动转换。
set hive.auto.convert.join=true;

-- 开启'无条件转Map Join'
-- 是否将多个map join转成一个map join,默认开启
set hive.auto.convert.join.noconditionaltask=true;

-- 无条件转Map Join时的小表之和阈值
-- 多个mapjoin转换为1个时，所有小表的文件大小总和的最大值
-- 调整size使其 = product_info 默认是10000000
set hive.auto.convert.join.noconditionaltask.size=25285707;

-- 执行计划
explain
select od.user_id,od.product_id,od.province_id,product.product_name,province.province_name
from order_detail od
join product_info product on od.product_id = product.id
join province_info province on od.province_id = province.id
limit 10;

```

![[1104875-20230812114634903-1092580031.png|400]]

```sql
select od.user_id,od.product_id,od.province_id,product.product_name,province.province_name
from order_detail od
join product_info product on od.product_id = product.id
join province_info province on od.province_id = province.id
limit 10;
```

执行跟方案一一样，执行两个map join。只是方案一有备份任务BackUp Task，如果执行失败，就执行备份任务。

```txt
...
INFO  : MapReduce Jobs Launched: 
INFO  : Stage-Stage-6: Map: 2   Cumulative CPU: 345.89 sec   HDFS Read: 347969943 HDFS Write: 267056511 SUCCESS
INFO  : Stage-Stage-5: Map: 1   Cumulative CPU: 4.25 sec   HDFS Read: 405661 HDFS Write: 608 SUCCESS
INFO  : Total MapReduce CPU Time Spent: 5 minutes 50 seconds 140 msec
INFO  : Completed executing command(queryId=hadoop_20231010163116_5bb0774a-e648-43f7-a49d-5785a7d73671); Time taken: 81.408 seconds
```
#### 5.2.2 map join 优化前后对比

执行时间对比：

| 优化前                                 | 104.238 seconds |
| -------------------------------------- | --------------- |
| 方案一：两个map join，还有其他备份方案 | 90.899 seconds  |
| 方案二：两个map join合并成一个map join | 31.98 seconds   |
| 方案三：两个map join                   | 81.408 seconds                |

执行计划对比：如上面截图所示
### 5.3 bucket map join 优化

Bucket Map Join 不支持自动转换，发须通过用户在 SQL 语句中提供如下 Hint 提示并配置如下相关参数，方可使用。

```sql
-- 相关参数
--关闭cbo优化，cbo会导致hint信息被忽略
set hive.cbo.enable=false;
--map join hint默认会被忽略(因为已经过时)，需将如下参数设置为false
set hive.ignore.mapjoin.hint=false;
--启用bucket map join优化功能
set hive.optimize.bucketmapjoin=true;

select /*+ mapjoin(ta) */
ta.id,
tb.id
from table_a ta
join table_b tb on ta.id=tb.id;
```

测试sql：

```sql
select t1.user_id,t1.product_id,t2.payment_time,t1.total_amount
from order_detail t1
join payment_detail t2
on t1.id = t2.order_detail_id
limit 10;
```
#### 5.3.1 bucket map join 优化前

```sql
-- 执行计划
explain
select t1.user_id,t1.product_id,t2.payment_time,t1.total_amount
from order_detail t1
join payment_detail t2
on t1.id = t2.order_detail_id
limit 10;
```

上述SQL语句共有两张表一次join操作，故优化前的执行计划应包含一个Common Join任务，通过一个MapReduce Job实现。执行计划如下图所示：
![[1104875-20230812114733512-1001564612.png|400]]

```sql
-- 执行
select t1.user_id,t1.product_id,t2.payment_time,t1.total_amount
from order_detail t1
join payment_detail t2
on t1.id = t2.order_detail_id
limit 10;
```

```txt
...
INFO  : MapReduce Jobs Launched: 
INFO  : Stage-Stage-1: Map: 3  Reduce: 2   Cumulative CPU: 81.59 sec   HDFS Read: 488404076 HDFS Write: 1312 SUCCESS
INFO  : Total MapReduce CPU Time Spent: 1 minutes 21 seconds 590 msec
INFO  : Completed executing command(queryId=hadoop_20231010165646_1e16d87b-1037-483f-968b-83df62c31441); Time taken: 42.83 seconds
```
#### 5.3.2 bucket map join 优化后

两张表都相对较大，若采用普通的Map Join算法，则Map端需要较多的内存来缓存数据，当然可以选择为Map段分配更多的内存，来保证任务运行成功。但是，Map端的内存不可能无上限的分配，所以当参与Join的表数据量均过大时，就可以考虑采用Bucket Map Join算法。下面演示如何使用Bucket Map Join。

首先需要依据源表创建两个分桶表，order_detail建议分10个bucket，payment_detail建议分5个bucket,注意。==参与join的表均为分桶表，且关联字段为分桶字段，且其中一张表的分桶数量是另外一张表分桶数量的整数倍==

（1）建表导入数据：

```sql
-- 建表
drop table if exists order_detail_bucketed;
create table order_detail_bucketed(
id    string   comment '订单id',
user_id   string   comment '用户id',
product_id  string   comment '商品id',
province_id  string   comment '省份id',
create_time  string   comment '下单时间',
product_num  int    comment '商品件数',
total_amount decimal(16,2) comment '下单金额'
)
clustered by (id) into 10 buckets
row format delimited fields terminated by ',';

drop table if exists payment_detail_bucketed;
create table payment_detail_bucketed(
id              string comment '支付id',
order_detail_id string comment '订单明细id',
user_id         string comment '用户id',
payment_time    string comment '支付时间',
total_amount    decimal(16, 2) comment '支付金额'
)
clustered by (order_detail_id) into 5 buckets
row format delimited fields terminated by ',';

--导入数据
load data local inpath '/home/hadoop/hivedata/order_detail.dat' overwrite into table order_detail_bucketed;

load data local inpath '/home/hadoop/hivedata/payment_detail.dat' overwrite into table payment_detail_bucketed;

-- 查询数据
select * from order_detail_bucketed limit 5;
-- 查询数据
select * from payment_detail_bucketed limit 5;
```

（2）设置以下参数：

```sql
--关闭cbo优化，cbo会导致hint信息被忽略，需将如下参数修改为false
set hive.cbo.enable=false;

--map join hint默认会被忽略(因为已经过时)，需将如下参数修改为false
set hive.ignore.mapjoin.hint=false;

--启用bucket map join优化功能,默认不启用，需将如下参数修改为true
set hive.optimize.bucketmapjoin=true;
```

（3）重写SQL语句：

```sql
-- 执行计划
explain
select  /*+ mapjoin(t2) */
t1.user_id,t1.product_id,t2.payment_time,t1.total_amount
from order_detail_bucketed t1
join payment_detail_bucketed t2
on t1.id = t2.order_detail_id
limit 10;
```

需要注意的是，Bucket Map Join的执行计划的基本信息和普通的Map Join无异，若想看到差异，可执行如下语句，查看执行计划的详细信息。详细执行计划中，如在Map Join Operator中看到 “**BucketMapJoin: true**”，则表明使用的Join算法为Bucket Map Join。

![[1104875-20230812114823349-1063618793.png|400]]

```sql
select  /*+ mapjoin(t2) */
t1.user_id,t1.product_id,t2.payment_time,t1.total_amount
from order_detail_bucketed t1
join payment_detail_bucketed t2
on t1.id = t2.order_detail_id
limit 10;
```

一次开启7个map task

```txt
INFO  : MapReduce Jobs Launched: 
INFO  : Stage-Stage-1: Map: 10   Cumulative CPU: 10720.17 sec   HDFS Read: 348063622 HDFS Write: 6554 SUCCESS
INFO  : Total MapReduce CPU Time Spent: 0 days 2 hours 58 minutes 40 seconds 170 msec
INFO  : Completed executing command(queryId=hadoop_20231010173015_41958817-a60c-4721-ae47-581a0da69e7d); Time taken: 418.916 seconds
```

![[Pasted image 20231010174732.png|325]]

```sql
-- 增大map内存
set mapreduce.map.memory.mb=2048;
```

一次只启动两个job

```sql
INFO  : MapReduce Jobs Launched: 
INFO  : Stage-Stage-1: Map: 10   Cumulative CPU: 230.57 sec   HDFS Read: 348064632 HDFS Write: 6554 SUCCESS
INFO  : Total MapReduce CPU Time Spent: 3 minutes 50 seconds 570 msec
INFO  : Completed executing command(queryId=hadoop_20231010181225_be598624-2136-4a81-8c1d-5bca6b010cc5); Time taken: 79.597 seconds
```

![[Pasted image 20231010181526.png|400]]
#### 5.3.3 bucket map join 优化前后对比

执行时间对比：

bucket map join还是比较耗内存的，内存越大执行越快。

| 优化前            | 42.83 second    |
| ----------------- | --------------- |
| 优化后，map内存1G | 418.916 seconds |
| 优化后，map内存2G |       79.597 seconds          |

执行计划对比：查看截图所示


### 5.4 sort merge bucket map join 优化

Sort Merge Bucket Map Join有两种触发方式，包括Hint提示和自动转换。Hint提示已过时，不推荐使用。下面是自动转换的相关参数：

```sql
--启动Sort Merge Bucket Map Join优化
set hive.optimize.bucketmapjoin.sortedmerge=true;

--使用自动转换SMB Join
set hive.auto.convert.sortmerge.join=true;
```

测试sql：

```sql
select t1.user_id,t1.product_id,t2.payment_time,t1.total_amount
from order_detail t1
join payment_detail t2
on t1.id = t2.order_detail_id
limit 10;
```

#### 5.4.1 sort merge bucket map join 优化前

上述SQL语句共有两张表一次join操作，故优化前的执行计划应包含一个Common Join任务，通过一个MapReduce Job实现。

参见[[#5.3.1 bucket map join 优化前]]

#### 5.4.2 sort merge bucket map join 优化后

两张表都相对较大，除了可以考虑采用Bucket Map Join算法，还可以考虑SMB Join。相较于Bucket Map Join，SMB Map Join对分桶大小是没有要求的。下面演示如何使用SMB Map Join。

首先需要依据源表创建两个的有序的分桶表，order_detail建议分10个bucket，payment_detail建议分5个bucket,注意==参与join的表均为分桶表，且需保证分桶内的数据是有序的，且分桶字段、排序字段和关联字段为相同字段，且其中一张表的分桶数量是另外一张表分桶数量的整数倍。==

（1）建表导入数据：

```sql
-- 建表
drop table if exists order_detail_sorted_bucketed;
create table order_detail_sorted_bucketed(
id    string   comment '订单id',
user_id   string   comment '用户id',
product_id  string   comment '商品id',
province_id  string   comment '省份id',
create_time  string   comment '下单时间',
product_num  int    comment '商品件数',
total_amount decimal(16,2) comment '下单金额'
)
clustered by (id) sorted by(id) into 10 buckets
row format delimited fields terminated by ',';

drop table if exists payment_detail_sorted_bucketed;
create table payment_detail_sorted_bucketed(
id              string comment '支付id',
order_detail_id string comment '订单明细id',
user_id         string comment '用户id',
payment_time    string comment '支付时间',
total_amount    decimal(16, 2) comment '支付金额'
)
clustered by (order_detail_id) sorted by(order_detail_id) into 5 buckets
row format delimited fields terminated by ',';

--导入数据
load data local inpath '/home/hadoop/hivedata/order_detail.dat' overwrite into table order_detail_sorted_bucketed;

load data local inpath '/home/hadoop/hivedata/payment_detail.dat' overwrite into table payment_detail_sorted_bucketed;

-- 查询数据
select * from order_detail_sorted_bucketed limit 5;
-- 查询数据
select * from payment_detail_sorted_bucketed limit 5;
```

（2）设置以下参数：

```sql
--启动Sort Merge Bucket Map Join优化
set hive.optimize.bucketmapjoin.sortedmerge=true;

--使用自动转换SMB Join
set hive.auto.convert.sortmerge.join=true;
```

（3）查看执行计划：

```sql
-- 执行计划
explain
select t1.user_id,t1.product_id,t2.payment_time,t1.total_amount
from order_detail_sorted_bucketed t1
join payment_detail_sorted_bucketed t2
on t1.id = t2.order_detail_id
limit 10;
```

![[Pasted image 20231010185629.png|231]]

（4）执行sql

```sql
select t1.user_id,t1.product_id,t2.payment_time,t1.total_amount
from order_detail_sorted_bucketed t1
join payment_detail_sorted_bucketed t2
on t1.id = t2.order_detail_id
limit 10;
```

```txt
INFO  : MapReduce Jobs Launched: 
INFO  : Stage-Stage-1: Map: 10   Cumulative CPU: 176.2 sec   HDFS Read: 348089742 HDFS Write: 6558 SUCCESS
INFO  : Total MapReduce CPU Time Spent: 2 minutes 56 seconds 200 msec
INFO  : Completed executing command(queryId=hadoop_20231010185443_435d0cf0-d7ac-4fa4-b591-94d7008e7bef); Time taken: 35.205 seconds
```

#### 5.4.3 sort merge bucket map join 优化前后

执行时间对比：

bucket map join还是比较耗内存的，内存越大执行越快。

| 优化前，map内存1G                           | 42.83 second    |
| -------------------------------- | --------------- |
| bucket map join优化后，map内存1G | 418.916 seconds |
| bucket map join优化后，map内存2G | 79.597 seconds  |
| SMB Join优化后，map内存1G                                 |   35.205 seconds              |

执行计划对比：查看截图所示
## 六、hql语法优化—数据倾斜

### 6.1 数据倾斜概述

数据倾斜问题，通常是指参与计算的数据分布不均，即某个key或者某些key的数据量远超其他key，导致在shuffle阶段，大量相同key的数据被发往同一个Reduce，进而导致该Reduce所需的时间远超其他Reduce，成为整个任务的瓶颈。

Hive中的数据倾斜常出现在分组聚合（group by）和join操作的场景中，下面分别介绍在上述两种场景下的优化思路。

数据准备：

```sql
-- 建表
drop table if exists order_detail_skew;
create table order_detail_skew(
id    string   comment '订单id',
user_id   string   comment '用户id',
product_id  string   comment '商品id',
province_id  string   comment '省份id',
create_time  string   comment '下单时间',
product_num  int    comment '商品件数',
total_amount decimal(16,2) comment '下单金额'
)
row format delimited fields terminated by '\t';

load data local inpath '/home/hadoop/hivedata/order_detail.txt' overwrite into table order_detail_skew;

-- 查询数据
select * from order_detail_skew limit 5;

-- 20000000 2千万条数据
select count(*) from order_detail_skew;
```

### 6.2 分组聚合导致的数据倾斜

如果group by分组字段的值分布不均，就可能导致大量相同的key进入同一Reduce，从而导致数据倾斜问题。

测试sql：

```sql
select province_id,count(*) from order_detail_skew group by province_id;
```
#### 6.2.1 分组聚合导致的数据倾斜优化前

order_detail表中的province_id字段是存在倾斜的，若不经过优化，通过观察任务的执行过程，是能够看出数据倾斜现象的。

```sql
--关闭map-side聚合，默认开启
set hive.map.aggr=false;

-- 执行语句
select province_id,count(*) from order_detail_skew group by province_id;
```

```txt
INFO  : MapReduce Jobs Launched: 
INFO  : Stage-Stage-1: Map: 5  Reduce: 5   Cumulative CPU: 172.64 sec   HDFS Read: 1176100928 HDFS Write: 1080 SUCCESS
INFO  : Total MapReduce CPU Time Spent: 2 minutes 52 seconds 640 msec
INFO  : Completed executing command(queryId=hadoop_20231011132959_1adac868-8d73-4eb2-a853-2bd9fb1ca8ad); Time taken: 52.484 seconds
```

![[Pasted image 20231011133504.png]]
#### 6.2.1 分组聚合导致的数据倾斜优化后-Map-Side聚合

参见[[#四、hql语法优化—分组聚合优化]]

```sql
--启用map-side聚合,默认开启的
set hive.map.aggr=true;
--关闭skew-groupby,默认关闭
set hive.groupby.skewindata=false;

select province_id,count(*) from order_detail_skew group by province_id;
```

```txt
INFO  : MapReduce Jobs Launched: 
INFO  : Stage-Stage-1: Map: 5  Reduce: 5   Cumulative CPU: 124.48 sec   HDFS Read: 1176115273 HDFS Write: 1080 SUCCESS
INFO  : Total MapReduce CPU Time Spent: 2 minutes 4 seconds 480 msec
INFO  : Completed executing command(queryId=hadoop_20231011133522_aeb377a9-f73a-4334-bdb6-a8e323fc030b); Time taken: 31.947 seconds
```

很明显可以看到开启map-side聚合后，reduce数据不再倾斜。

![[Pasted image 20231011133645.png]]
#### 6.2.2 分组聚合导致的数据倾斜优化后-Skew-GroupBy优化

```sql
--启动skew-groupby,默认关闭
set hive.groupby.skewindata=true;

--关闭map-side聚合,默认开启的
set hive.map.aggr=false;

-- 执行计划
explain
select province_id,count(*) from order_detail_skew group by province_id;
```

开启Skew-GroupBy优化后，可以很明显看到该sql执行在yarn上启动了两个mr任务，第一个mr打散数据（map端：Map-reduce partition columns: rand()；reduce端进行聚合），第二个mr按照打散后的数据进行分组聚合。

![[Pasted image 20231010213255.png|250]]

```sql
select province_id,count(*) from order_detail_skew group by province_id;
```

```txt
INFO  : MapReduce Jobs Launched: 
INFO  : Stage-Stage-1: Map: 5  Reduce: 5   Cumulative CPU: 281.35 sec   HDFS Read: 1176100358 HDFS Write: 4045 SUCCESS
INFO  : Stage-Stage-2: Map: 1  Reduce: 1   Cumulative CPU: 6.94 sec   HDFS Read: 12587 HDFS Write: 732 SUCCESS
INFO  : Total MapReduce CPU Time Spent: 4 minutes 48 seconds 290 msec
INFO  : Completed executing command(queryId=hadoop_20231011133715_5ca87dab-edcb-4cff-9902-54f4c97f19c0); Time taken: 71.146 seconds
```

![[Pasted image 20231011133944.png]]

![[Pasted image 20231011134022.png]]
### 6.3 Join导致的数据倾斜

前文提到过，未经优化的join操作，默认是使用common join算法，也就是通过一个MapReduce Job完成计算。

如果关联字段的值分布不均，就可能导致大量相同的key进入同一Reduce，从而导致数据倾斜问题。

由join导致的数据倾斜问题，有如下三种解决方案：

测试sql：

```sql
select t1.province_id,t2.province_name
from order_detail_skew t1
join province_info t2
on t1.province_id = t2.id
limit 10;
```
#### 6.3.1 Join导致的数据倾斜优化前

order_detail表中的province_id字段是存在倾斜的，若不经过优化，通过观察任务的执行过程，是能够看出数据倾斜现象的。

```sql
--关闭Map Join自动转换，默认开启
set hive.auto.convert.join=false;

select t1.province_id,t2.province_name
from order_detail_skew t1
join province_info t2
on t1.province_id = t2.id
limit 10;
```

```txt
INFO  : MapReduce Jobs Launched: 
INFO  : Stage-Stage-1: Map: 6  Reduce: 5   Cumulative CPU: 372.19 sec   HDFS Read: 1176110063 HDFS Write: 1825 SUCCESS
INFO  : Total MapReduce CPU Time Spent: 6 minutes 12 seconds 190 msec
INFO  : Completed executing command(queryId=hadoop_20231011134136_3d802055-c72a-42ee-9c0f-85b597bc7b66); Time taken: 81.505 seconds
```

![[Pasted image 20231011134353.png]]
#### 6.3.2 Join导致的数据倾斜优化后-map join

见[[#5.2 map join优化]]

```sql
--启动Map Join自动转换，默认开启
set hive.auto.convert.join=true;

--关闭skew join，默认关闭
set hive.optimize.skewjoin=false;

select t1.province_id,t2.province_name
from order_detail_skew t1
join province_info t2
on t1.province_id = t2.id
limit 10;
```

```txt
INFO  : MapReduce Jobs Launched: 
INFO  : Stage-Stage-3: Map: 5   Cumulative CPU: 39.79 sec   HDFS Read: 2176020 HDFS Write: 1785 SUCCESS
INFO  : Total MapReduce CPU Time Spent: 39 seconds 790 msec
INFO  : Completed executing command(queryId=hadoop_20231011134410_d14c768d-f1ed-43aa-b0b3-b9c909f92208); Time taken: 27.373 seconds
```

可以很明显看到开启map join以后，mr任务只有map阶段，没有reduce阶段，自然也就不会有数据倾斜发生。

![[Pasted image 20231011134531.png]]

![[Pasted image 20231011134549.png]]
#### 6.3.3 Join导致的数据倾斜优化后-skew join

![[Pasted image 20231010223208.png|400]]

```sql
--开启skew join，默认关闭
set hive.optimize.skewjoin=true;

--关闭Map Join自动转换，默认开启
set hive.auto.convert.join=false;

--触发skew join的阈值，若某个key的行数超过该参数值，则触发，默认的
-- set hive.skewjoin.key=100000;

explain
select t1.province_id,t2.province_name
from order_detail_skew t1
join province_info t2
on t1.province_id = t2.id
limit 10;
```

开启skew join后，使用explain可以很明显看到执行计划如下图所示，说明skew join生效，任务既有common join，又有部分key走了map join。

这种方案对参与join的源表大小没有要求，但是对两表中倾斜的key的数据量有要求，要求一张表中的倾斜key的数据量比较小（方便走mapjoin）。

![[Pasted image 20231010223107.png|250]]

```sql
select t1.province_id,t2.province_name
from order_detail_skew t1
join province_info t2
on t1.province_id = t2.id
limit 10;
```

```txt
INFO  : Stage-Stage-1: Map: 6  Reduce: 5   Cumulative CPU: 402.95 sec   HDFS Read: 1176114428 HDFS Write: 319761830 SUCCESS
INFO  : Stage-Stage-3: Map: 10   Cumulative CPU: 69.8 sec   HDFS Read: 931054 HDFS Write: 3570 SUCCESS
INFO  : Total MapReduce CPU Time Spent: 7 minutes 52 seconds 750 msec
INFO  : Completed executing command(queryId=hadoop_20231011134732_72750d8c-cb19-4c19-a6a1-da861509d0fc); Time taken: 110.692 seconds
```

![[Pasted image 20231011135001.png]]

![[Pasted image 20231011135051.png]]

![[Pasted image 20231011135131.png]]


对于大表join大表，可参考如下方法

```sql
--开启skew join，默认关闭
set hive.optimize.skewjoin=false;

--关闭Map Join自动转换，默认开启
set hive.auto.convert.join=false;

explain
select t1.province_id,t2.province_name
from order_detail_skew t1
join (
-- 加随机数扩容
select concat(id,'_',0) as id,province_name from province_info
union all
select concat(id,'_',1) as id,province_name from province_info
)t2
on concat(t1.province_id,'_',cast(rand()*2 as int)) = t2.id -- t1表加随机数打散
limit 10;
```

![[Pasted image 20231011131539.png|400]]

```txt
INFO  : MapReduce Jobs Launched: 
INFO  : Stage-Stage-1: Map: 6  Reduce: 5   Cumulative CPU: 301.06 sec   HDFS Read: 1176130099 HDFS Write: 1835 SUCCESS
INFO  : Total MapReduce CPU Time Spent: 5 minutes 1 seconds 60 msec
INFO  : Completed executing command(queryId=hadoop_20231011135549_f4b8702d-ca5c-43cb-8f8a-7c0acc045512); Time taken: 55.859 seconds
```

![[Pasted image 20231011135816.png]]
## 七、hql语法优化—任务并行度

对于一个分布式的计算任务而言，设置一个合适的并行度十分重要。Hive的计算任务由MapReduce完成，故并行度的调整需要分为Map端和Reduce端。
### 7.1 map端并行度

Map端的并行度，也就是Map的个数。是由输入文件的切片数决定的。一般情况下，Map端的并行度无需手动调整。

以下特殊情况可考虑调整map端并行度：

（1）查询的表中存在大量小文件

按照Hadoop默认切片策略，一个小文件会单独启动一个map task负责计算。若查询的表中存在大量小文件，则会启动大量map task，造成计算资源的浪费。这种情况下，可以使用Hive提供的CombineHiveInputFormat，多个小文件合并为一个切片，从而控制map task个数。相关参数如下：

```sql
-- 新版本hive默认是这个，旧版本可能是 set hive.input.format=org.apache.hadoop.mapred.TextInputFormat
set hive.input.format=org.apache.hadoop.hive.ql.io.CombineHiveInputFormat;
```

（2）map端有复杂的查询逻辑

若SQL语句中有正则替换、json解析等复杂耗时的查询逻辑时，map端的计算会相对慢一些。若想加快计算速度，在计算资源充足的情况下，可考虑增大map端的并行度，令map task多一些，每个map task计算的数据少一些。相关参数如下：

```sql
--一个切片的最大值 256M
set mapreduce.input.fileinputformat.split.maxsize=256000000;
```
### 7.2 reduce端并行度

Reduce端的并行度，也就是Reduce个数。相对来说，更需要关注。Reduce端的并行度，可由用户自己指定，也可由Hive自行根据该MR Job输入的文件大小进行估算。

Reduce端的并行度的相关参数如下：

```sql
--指定Reduce端并行度，默认值为-1，表示用户未指定
set mapreduce.job.reduces;

--Reduce端并行度最大值,默认1009
set hive.exec.reducers.max;

--单个Reduce Task计算的数据量，用于估算Reduce并行度,默认是256M
set hive.exec.reducers.bytes.per.reducer;
```

Reduce端并行度的确定逻辑如下：

若指定参数**mapreduce.job.reduces**的值为一个非负整数，则Reduce并行度为指定值。否则，Hive自行估算Reduce并行度，估算逻辑如下：

假设Job输入的文件大小为**totalInputBytes**

参数**hive.exec.reducers.bytes.per.reducer**的值为bytesPerReducer。

参数**hive.exec.reducers.max**的值为maxReducers。

则Reduce端的并行度为：

![](file:///C:\Users\iamwu\AppData\Local\Temp\ksohtml15016\wps1.jpg) 

根据上述描述，可以看出，==Hive自行估算Reduce并行度时，是以整个MR Job输入的文件大小作为依据的==。因此，在某些情况下其估计的并行度很可能并不准确，此时就需要用户根据实际情况来指定Reduce并行度了。

### 7.3 任务并行度-优化案例

测试sql：

```sql
select province_id,count(*) from order_detail_skew group by province_id;
```

（1）优化前

```sql
desc formatted order_detail_skew;
--Reduce端并行度最大值,默认1009
set hive.exec.reducers.max;
--单个Reduce Task计算的数据量，用于估算Reduce并行度,默认是256M
set hive.exec.reducers.bytes.per.reducer;
```

结果为

```txt
totalSize=1176009934
hive.exec.reducers.max=1009
hive.exec.reducers.bytes.per.reducer=256000000 
```

上述sql语句，在不指定Reduce并行度时，Hive自行估算并行度的逻辑如下：

```txt
totalInputBytes= 1136009934
bytesPerReducer=256000000
maxReducers=1009
```

经计算，Reduce并行度为

![](file:///C:\Users\iamwu\AppData\Local\Temp\ksohtml15016\wps3.jpg)


上述sql语句，在默认情况下，是会进行map-side聚合的，也就是Reduce端接收的数据，实际上是map端完成聚合之后的结果。观察任务的执行过程，会发现，每个map端输出的数据只有34条记录，共有5个map task。

```sql
--启用map-side聚合（默认开启） 
set hive.map.aggr=true; 
--关闭skew-groupby,默认关闭
set hive.groupby.skewindata=false;

select province_id,count(*) from order_detail_skew group by province_id;
```

```txt
INFO  : MapReduce Jobs Launched: 
INFO  : Stage-Stage-1: Map: 5  Reduce: 5   Cumulative CPU: 88.54 sec   HDFS Read: 1176115308 HDFS Write: 1080 SUCCESS
INFO  : Total MapReduce CPU Time Spent: 1 minutes 28 seconds 540 msec
INFO  : Completed executing command(queryId=hadoop_20231011142532_d7eb011a-719d-431c-b4f6-c0067f3bc628); Time taken: 30.221 seconds
```

![[Pasted image 20231011142704.png]]

![[Pasted image 20231011142748.png]]

也就是说Reduce端实际只会接收170条记录，故理论上Reduce端并行度设置为1就足够了。这种情况下，用户可通过以下参数，自行设置Reduce端并行度为1。

（2）优化后

```sql
--指定Reduce端并行度，默认值为-1，表示用户未指定
set mapreduce.job.reduces=1;
select province_id,count(*) from order_detail_skew group by province_id;
```

```txt
INFO  : MapReduce Jobs Launched: 
INFO  : Stage-Stage-1: Map: 5  Reduce: 1   Cumulative CPU: 104.92 sec   HDFS Read: 1176086916 HDFS Write: 732 SUCCESS
INFO  : Total MapReduce CPU Time Spent: 1 minutes 44 seconds 920 msec
INFO  : Completed executing command(queryId=hadoop_20231011143013_ce09e15e-37c9-4943-bd5e-6e21fa910a2c); Time taken: 28.634 seconds
```

![[Pasted image 20231011143122.png]]
## 八、hql语法优化—小文件合并

小文件合并优化，分为两个方面，分别是Map端输入的小文件合并，和Reduce端输出的小文件合并。
### 8.1 Map端输入文件合并

合并Map端输入的小文件，是指==将多个小文件划分到一个切片中，进而由一个Map Task去处理==。目的是防止为单个小文件启动一个Map Task，浪费计算资源。见[[#7.1 map端并行度]]

相关参数为：

```sql
--可将多个小文件切片，合并为一个切片，进而由一个map任务处理
set hive.input.format=org.apache.hadoop.hive.ql.io.CombineHiveInputFormat;
--一个切片的最大值 256M
set mapreduce.input.fileinputformat.split.maxsize=256000000;
```
### 8.2 Reduce输出文件合并

合并Reduce端输出的小文件，是指==将多个小文件合并成大文件==。目的是减少HDFS小文件数量。==其原理是根据计算任务输出文件的平均大小进行判断，若符合条件，则单独启动一个额外的任务进行合并。==

相关参数为：

```sql
--开启合并map only任务输出的小文件,默认true
set hive.merge.mapfiles=true;

--开启合并map reduce任务输出的小文件,默认flase
set hive.merge.mapredfiles=true;

--合并后的文件大小,默认256000000（256M）
set hive.merge.size.per.task=256000000;

--触发小文件合并任务的阈值，若某计算任务输出的文件平均大小低于该值，则触发合并。默认是16000000（16M）
set hive.merge.smallfiles.avgsize=16000000;
```
### 8.3 小文件合并-优化案例

现有一个需求，计算各省份订单金额总和，下表为结果表。

```sql
drop table if exists order_amount_by_province;
create table order_amount_by_province(
province_id string comment '省份id',
order_amount decimal(16,2) comment '订单金额'
)
location '/order_amount_by_province';
```

```sql
insert overwrite table order_amount_by_province
select
    province_id,
    sum(total_amount)
from order_detail_skew
group by province_id;
```

（1）优化前

根据任务并行度一节所需内容，见[[#7.2 reduce端并行度]]，可分析出，默认情况下，该sql语句的Reduce端并行度为5，故最终输出文件个数也为5，下图为输出文件，可以看出，5个均为小文件。

```txt
INFO  : MapReduce Jobs Launched: 
INFO  : Stage-Stage-1: Map: 5  Reduce: 5   Cumulative CPU: 155.42 sec   HDFS Read: 1176128463 HDFS Write: 2015 SUCCESS
INFO  : Stage-Stage-3: Map: 1  Reduce: 1   Cumulative CPU: 7.23 sec   HDFS Read: 13279 HDFS Write: 548 SUCCESS
INFO  : Total MapReduce CPU Time Spent: 2 minutes 42 seconds 650 msec
INFO  : Completed executing command(queryId=hadoop_20231011173824_6ac97917-291a-4220-80fd-d7b67e7fb473); Time taken: 61.731 seconds
```

最后一个Stage-3：统计结果表的数据情况。

![[Pasted image 20231011174746.png|400]]

（2）优化后

若想避免小文件的产生，可采取方案有两个。

1）合理设置任务的Reduce端并行度，见[[#7.2 reduce端并行度]]。若将上述计算任务的并行度设置为1，就能保证其输出结果只有一个文件。

2）启用Hive合并小文件优化

设置以下参数：

```sql
--开启合并map reduce任务输出的小文件,默认flase
set hive.merge.mapredfiles=true;
```

```txt
INFO  : MapReduce Jobs Launched: 
INFO  : Stage-Stage-1: Map: 5  Reduce: 5   Cumulative CPU: 120.19 sec   HDFS Read: 1176128473 HDFS Write: 2015 SUCCESS
INFO  : Stage-Stage-8: Map: 1  Reduce: 1   Cumulative CPU: 6.74 sec   HDFS Read: 13286 HDFS Write: 548 SUCCESS
INFO  : Stage-Stage-3: Map: 1   Cumulative CPU: 2.72 sec   HDFS Read: 3781 HDFS Write: 426 SUCCESS
INFO  : Total MapReduce CPU Time Spent: 2 minutes 9 seconds 650 msec
INFO  : Completed executing command(queryId=hadoop_20231011175513_3958b728-3cdb-46cf-808a-eea4a19795f7); Time taken: 79.393 seconds
```

Stage-8：统计结果表的数据情况。

Stage-3：进行map小文件合并

![[Pasted image 20231011175913.png]]
## 九、其他优化

### 9.1 CBO优化

CBO是指Cost based Optimizer，即基于计算成本的优化。

在Hive中，计算成本模型考虑到了：数据的行数、CPU、本地IO、HDFS IO、网络IO等方面。Hive会计算同一SQL语句的不同执行计划的计算成本，并选出成本最低的执行计划。==目前CBO在hive的MR引擎下主要用于join的优化==，例如多表join的join顺序。

```sql
--是否启用cbo优化 ，默认开启
set hive.cbo.enable=true;
```

测试sql：

```sql
explain
select *
from order_detail_skew od
join product_info product on od.product_id = product.id
join province_info province on od.province_id = province.id
limit 10;
```

==关闭CBO优化：==

```sql
--是否启用cbo优化 ，默认开启
set hive.cbo.enable=false;

--为了测试效果更加直观，关闭map join自动转换,默认开启
set hive.auto.convert.join=false;
```

根据执行计划，可以看出，三张表的join顺序如下：

![[Pasted image 20231011181011.png|275]]

==开启CBO优化：==

```sql
--是否启用cbo优化 ，默认开启
set hive.cbo.enable=true;

--为了测试效果更加直观，关闭map join自动转换,默认开启
set hive.auto.convert.join=false;
```

![[Pasted image 20231011182409.png|325]]

根据上述案例可以看出，CBO优化对于执行计划中join顺序是有影响的，其之所以会将province_info的join顺序提前，是因为province info的数据量较小，将其提前，会有更大的概率使得中间结果的数据量变小，从而使整个计算任务的数据量减小，也就是使计算成本变小。
### 9.2 谓词下推

谓词下推（predicate pushdown）是指，尽量将过滤操作前移，以减少后续计算步骤的数据量。

相关参数为：

```sql
--是否启动谓词下推（predicate pushdown）优化，默认开启
set hive.optimize.ppd=true;
```

需要注意的是：CBO优化也会完成一部分的谓词下推优化工作，因为在执行计划中，谓词越靠前，整个计划的计算成本就会越低。

测试sql：

```sql
explain
select * 
from order_detail t1
join province_info t2
on t1.province_id = t2.id
where t1.province_id='2';
```

==关闭谓词下推优化：==

```sql
--是否启动谓词下推（predicate pushdown）优化，默认开启
set hive.optimize.ppd=false;

--为了测试效果更加直观，关闭cbo优化
set hive.cbo.enable=false;
```

通过执行计划可以看到，过滤操作位于执行计划中的join操作之后。

开启谓词下推优化：

```sql
--是否启动谓词下推（predicate pushdown）优化，默认开启
set hive.optimize.ppd=true;

--为了测试效果更加直观，关闭cbo优化
set hive.cbo.enable=false;
```

通过执行计划可以看出，过滤操作位于执行计划中的join操作之前。
### 9.3 矢量化查询

Hive的矢量化查询优化，依赖于CPU的矢量化计算，CPU的矢量化计算的基本原理如下图：

![[Pasted image 20231011185631.png|400]]

Hive的矢量化查询，可以极大的提高一些典型查询场景（例如scans, filters, aggregates, and joins）下的CPU使用效率。

相关参数如下：

```sql
-- 默认开启
set hive.vectorized.execution.enabled=true;
```

若执行计划中，出现“Execution mode: vectorized”字样，即表明使用了矢量化计算。

官网参考连接：[Vectorized Query Execution - Apache Hive - Apache Software Foundation](https://cwiki.apache.org/confluence/display/Hive/Vectorized+Query+Execution#VectorizedQueryExecution-Limitations)
### 9.4 fetch抓取

Fetch抓取是指，Hive中对某些情况的查询可以不必使用MapReduce计算。例如：select * from emp;在这种情况下，==Hive可以简单地读取emp对应的存储目录下的文件，然后输出查询结果到控制台。==

```sql
--是否在特定场景转换为fetch 任务
--设置为none表示不转换
--设置为minimal表示支持select *，分区字段过滤，Limit等
--设置为more表示支持select 任意字段,包括函数，过滤，和limit等
set hive.fetch.task.conversion=more;
```
### 9.5 本地模式

大多数的Hadoop Job是需要Hadoop提供的完整的可扩展性来处理大数据集的。不过，有时Hive的输入数据量是非常小的。在这种情况下，为查询触发执行任务消耗的时间可能会比实际job的执行时间要多的多。对于大多数这种情况，==Hive可以通过本地模式在单台机器上处理所有的任务。对于小数据集，执行时间可以明显被缩短。==

相关参数如下：

```sql
--开启自动转换为本地模式,默认是false
set hive.exec.mode.local.auto=true;

--设置local MapReduce的最大输入数据量，当输入数据量小于这个值时采用local  MapReduce的方式，默认为134217728，即128M(134217728/1024/1024)
set hive.exec.mode.local.auto.inputbytes.max=134217728;

--设置local MapReduce的最大输入文件个数，当输入文件个数小于这个值时采用local MapReduce的方式，默认为4
set hive.exec.mode.local.auto.input.files.max=10;
```

测试sql：

```sql
select count(*) from product_info;
```

==关闭本地模式：==

```sql
--开启自动转换为本地模式,默认是false
set hive.exec.mode.local.auto=false;
```

走yarn集群：

```txt
INFO  : The url to track the job: http://hadoop102:8088/proxy/application_1697001201720_0027/
INFO  : Starting Job = job_1697001201720_0027, Tracking URL = http://hadoop102:8088/proxy/application_1697001201720_0027/
INFO  : Kill Command = /opt/module/hadoop-3.3.6/bin/mapred job  -kill job_1697001201720_0027
INFO  : Hadoop job information for Stage-1: number of mappers: 1; number of reducers: 1
INFO  : 2023-10-11 19:23:21,714 Stage-1 map = 0%,  reduce = 0%
INFO  : 2023-10-11 19:23:28,962 Stage-1 map = 100%,  reduce = 0%, Cumulative CPU 5.3 sec
INFO  : 2023-10-11 19:23:34,123 Stage-1 map = 100%,  reduce = 100%, Cumulative CPU 9.03 sec
INFO  : MapReduce Total cumulative CPU time: 9 seconds 30 msec
INFO  : Ended Job = job_1697001201720_0027
INFO  : MapReduce Jobs Launched: 
INFO  : Stage-Stage-1: Map: 1  Reduce: 1   Cumulative CPU: 9.03 sec   HDFS Read: 25298714 HDFS Write: 107 SUCCESS
INFO  : Total MapReduce CPU Time Spent: 9 seconds 30 msec
INFO  : Completed executing command(queryId=hadoop_20231011192313_32bb5cf2-f901-4634-9c73-66ad01a6fff9); Time taken: 22.535 seconds
```

==开启本地模式：==

```sql
--开启自动转换为本地模式,默认是false
set hive.exec.mode.local.auto=true;
```

走本地模式：

```txt
INFO  : The url to track the job: http://localhost:8080/
INFO  : Job running in-process (local Hadoop)
INFO  : 2023-10-11 19:25:33,634 Stage-1 map = 100%,  reduce = 100%
INFO  : Ended Job = job_local1107004526_0001
INFO  : MapReduce Jobs Launched: 
INFO  : Stage-Stage-1:  HDFS Read: 50667566 HDFS Write: 2306394761 SUCCESS
INFO  : Total MapReduce CPU Time Spent: 0 msec
INFO  : Completed executing command(queryId=hadoop_20231011192531_76427298-da92-4c7e-a24b-e57d2b3ecf3d); Time taken: 1.684 seconds
```

```sql
-- 表大小约332M
select count(*) from order_detail;
```

order_detail的数据量超过128M，走yarn集群。

```txt
INFO  : Starting Job = job_1697001201720_0029, Tracking URL = http://hadoop102:8088/proxy/application_1697001201720_0029/
INFO  : Kill Command = /opt/module/hadoop-3.3.6/bin/mapred job  -kill job_1697001201720_0029
INFO  : Hadoop job information for Stage-1: number of mappers: 2; number of reducers: 1
INFO  : 2023-10-11 19:27:47,202 Stage-1 map = 0%,  reduce = 0%
INFO  : 2023-10-11 19:27:55,448 Stage-1 map = 50%,  reduce = 0%, Cumulative CPU 7.53 sec
INFO  : 2023-10-11 19:27:56,476 Stage-1 map = 100%,  reduce = 0%, Cumulative CPU 12.39 sec
INFO  : 2023-10-11 19:28:01,601 Stage-1 map = 100%,  reduce = 100%, Cumulative CPU 16.24 sec
INFO  : MapReduce Total cumulative CPU time: 16 seconds 240 msec
INFO  : Ended Job = job_1697001201720_0029
INFO  : MapReduce Jobs Launched: 
INFO  : Stage-Stage-1: Map: 2  Reduce: 1   Cumulative CPU: 16.24 sec   HDFS Read: 347971750 HDFS Write: 107 SUCCESS
INFO  : Total MapReduce CPU Time Spent: 16 seconds 240 msec
INFO  : Completed executing command(queryId=hadoop_20231011192739_eb511702-5f9d-4bcb-9086-abb799bf2786); Time taken: 23.304 seconds
INFO  : OK
INFO  : Concurrency mode is disabled, not creating a lock manager
```
### 9.6 并行执行

Hive会将一个SQL语句转化成一个或者多个Stage，每个Stage对应一个MR Job。默认情况下，==Hive同时只会执行一个Stage==。但是某SQL语句可能会包含多个Stage，==但这多个Stage可能并非完全互相依赖，也就是说有些Stage是可以并行执行的==。此处提到的并行执行就是指这些Stage的并行执行。相关参数如下：

```sql
--启用并行执行优化，默认是false
set hive.exec.parallel=true;       

--同一个sql允许最大并行度，默认为8
set hive.exec.parallel.thread.number=8;
```
### 9.7 严格模式

Hive可以通过设置某些参数防止危险操作：

（1）分区表不使用分区过滤

将hive.strict.checks.no.partition.filter设置为true时，对于分区表，==除非where语句中含有分区字段过滤条件来限制范围，否则不允许执行==。换句话说，就是用户不允许扫描所有分区。进行这个限制的原因是，通常分区表都拥有非常大的数据集，而且数据增加迅速。没有进行分区限制的查询可能会消耗令人不可接受的巨大资源来处理这个表。

```sql
-- 开启分区表需要过滤分区。默认是false
set hive.strict.checks.no.partition.filter=true;
```

（2）使用order by没有limit过滤

将hive.strict.checks.orderby.no.limit设置为true时，对于使用了order by语句的查询，要求必须使用limit语句。因为order by为了执行排序过程会将所有的结果数据分发到同一个Reduce中进行处理，强制要求用户增加这个limit语句可以防止Reduce额外执行很长一段时间（开启了limit可以在数据进入到Reduce之前就减少一部分数据）。

```sql
-- 对于使用了order by语句的查询，要求必须使用limit语句。默认是false
set hive.strict.checks.orderby.no.limit=true;
```

（2）笛卡尔积

将hive.strict.checks.cartesian.product设置为true时，==会限制笛卡尔积的查询==。对关系型数据库非常了解的用户可能期望在执行JOIN查询的时候不使用ON语句而是使用where语句，这样关系数据库的执行优化器就可以高效地将WHERE语句转化成那个ON语句。不幸的是，Hive并不会执行这种优化，因此，如果表足够大，那么这个查询就会出现不可控的情况。

```sql
-- 限制笛卡尔积的查询。默认是false
set hive.strict.checks.cartesian.product=true;
```
