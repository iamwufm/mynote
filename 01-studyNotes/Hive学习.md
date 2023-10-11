---
author: wufm
title: Hive学习
rating: 4
time: 2023-09-28 周四
tags:
  - Hive
代码:
---
## 一、什么是Hive

### 1.1 hive基本思想

Hive是基于Hadoop的一个数据仓库工具(离线)，可以将==结构化的数据文件映射为一张数据库表，并提供类SQL查询功能==。

hive官网：[Apache Hive](https://hive.apache.org/)

![[hive基本思想.excalidraw|600]]
hive只要是把sql翻译成mr程序或spark程序等（主要根据自己设置的引擎：set hive.execution.engine。支持三种mr（默认）、tez和spark），然后提交给集群运行。

主要把hive安装在集群中或者集群外（能跟集群通讯并且有hadoop环境）即可。hive不是很消耗资源，安装在哪里没有要求。
### 1.2 为什么使用hive

（1）直接使用hadoop所面临的问题
- 人员学习成本太高
- 项目周期要求太短
- MapReduce实现复杂查询逻辑开发难度太大

（2）为什么要使用Hive
- 操作接口采用类SQL语法，提供快速开发的能力。
- 避免了去写MapReduce，减少开发人员的学习成本。
- 功能扩展很方便。
### 1.3 hive的特点

1. 可扩展：Hive可以自由的扩展集群的规模，一般情况下不需要重启服务。

2. 延展性：Hive支持用户自定义函数，用户可以根据自己的需求来实现自己的函数。

3. 容错：良好的容错性，节点出现问题SQL仍可完成执行。
## 二、Hive安装

见[[Hive安装]]

## 三、hive建库建表与数据导入

hive的DDL操作：[LanguageManual DDL - Apache Hive - Apache Software Foundation](https://cwiki.apache.org/confluence/display/hive/languagemanual+ddl#LanguageManualDDL-Overview)

为了方便测试，可以用本地模式，或者把yarn运行模式从yarn改为local。

启动hdfs集群，hive服务器，hive客户端。以下操作在hive客户端进行

```shell
# 1.在hadoop101上执行
start-dfs.sh

# 在hadoop102上执行
# 非本地模式运行需要启动
start-yarn.sh

# 2.启动服务器
hiveserver2

# 3.启动客户端
beeline -u jdbc:hive2://hadoop102:10000 -n hadoop

-- 本地模式，默认为flase
set hive.exec.mode.local.auto=true;
```
### 3.1 建库

hive中有一个默认的库：

库名： default

库目录（可通过 desc database default 查看）： hdfs://hadoop101:8020/user/hive/warehouse

新建库：

```sql
create database my_test;
```

库建好后，在hdfs中会生成一个库目录： hdfs://hadoop101:8020/user/hive/warehouse/my_test.db


数据库常用操作：

```mysql
-- 查看有哪些库 或 show databases like 'my*';
show databases;

-- 新建数据库 或 create database if not exists my_test2;
create database my_test2;

-- 查看数据库信息
desc database my_test2;
desc database extended my_test2;

-- 切换数据库
use my_test2;

-- 删除数据库 if exists
# 删除空数据库
drop database my_test2;
# 删除非空数据库
drop database my_test2 cascade;
```
### 3.2 建表

#### 3.2.1 基本建表语句

表建好后，会在所属的库目录中生成一个表目录
/user/hive/warehouse/my_test.db/t_order

hive会认为表数据文件中的字段分隔符为 `\001`，可以指定字段分隔符为“,”。

```sql
use my_test;
drop table if exists t_order;
create table t_order
(
 id string
,create_time string
,amount float
,uid string
)
row format delimited fields terminated by ',';
```

常用规范建表语句：

```sql
-- temporary：临时表，该表只在当前会话可见，会话结束，表会被删除。
-- external：外部表，与之相对应的是内部表（管理表）。管理表意味着Hive会完全接管该表，包括元数据和HDFS中的数据。而外部表则意味着Hive只接管元数据，而不完全接管HDFS中的数据。

create [temporary] [external] table [db_name.]table_name
(
 字段1 数据类型 comment '字段注释'
,字段2 数据类型 commment '字段注释'
...
)
comment '表注释'
partitioned by (分区字段1 数据类型 comment '分区字段1注释',....)
clustered by (字段1,字段2,...) sorted by (字段1 desc,字段2,...) into num_buckets buckets -- 创建分桶表
row format delimited fields terminated by char  -- 列分隔符
collection items terminated by char -- map、struct和array中每个元素之间的分隔符
map keys terminated by char -- map中的key与value的分隔符
lines terminated by char -- 行分隔符
stored as file_format -- 指定文件格式，常用的文件格式有，textfile（默认值），sequencefile，orc、parquet等等
location hdfs_path -- 指定表所对应的HDFS路径，若不指定路径，其默认值${hive.metastore.warehouse.dir}/db_name.db/table_name
;
```
#### 3.2.2 删除表

```sql
drop table t_order;
```

删除表的效果是：
- hive会从元数据库中清除关于这个表的信息（show tables）；
- hive还会从hdfs中删除这个表的表目录；
#### 3.2.3 内部表与外部表

外部表和内部表的特性差别：
1. 内部表的目录在hive的仓库目录中 VS 外部表的目录由用户指定
2. drop一个内部表时：hive会清除相关元数据，并删除表数据目录
3. drop一个外部表时：hive只会清除相关元数据；

一个hive的数据仓库，最底层的表，一定是来自于外部系统，为了不影响外部系统的工作逻辑，在hive中可建external表来映射这些外部系统产生的数据目录；然后，后续的etl操作，产生的各种表建议用managed_table

```sql
create external table t_access(ip string,url string,access_time string)
row format delimited fields terminated by ','
location '/access/log';
```
#### 3.2.4 分区表

Hive中的分区就是把一张大表的数据按照业务需要分散的==存储到多个目录==，每个目录就称为该表的一个分区。在查询时通过where子句中的表达式选择查询所需要的分区，这样的查询效率会提高很多。

比如，网站每天产生的浏览记录，浏览记录应该建一个表来存放，但是，有时候，我们可能只需要对某一天的浏览记录进行分析。这时，就可以将这个表建为分区表，每天的数据导入其中的一个分区；当然，每日的分区目录，应该有一个目录名（分区字段）

（1）创建带分区的表（==注意：分区字段不能是表定义中的已存在字段==）

```sql
-- 多分区字段，用逗号隔开
create table t_access(ip string,url string,access_time string)
partitioned by(dt string)
row format delimited fields terminated by ',';
```

（2）准备数据

```shell
cd /home/hadoop
mkdir hivedata
cd hivedata

vim access.log.2017-08-04.log

# 添加如下内容
192.168.33.3,http://www.edu360.cn/stu,2017-08-04 15:30:20
192.168.33.3,http://www.edu360.cn/teach,2017-08-04 15:35:20
192.168.33.4,http://www.edu360.cn/stu,2017-08-04 15:30:20
192.168.33.4,http://www.edu360.cn/job,2017-08-04 16:30:20
192.168.33.5,http://www.edu360.cn/job,2017-08-04 15:40:20

vim access.log.2017-08-05.log

# 添加如下内容
192.168.33.3,http://www.edu360.cn/stu,2017-08-05 15:30:20
192.168.44.3,http://www.edu360.cn/teach,2017-08-05 15:35:20
192.168.33.44,http://www.edu360.cn/stu,2017-08-05 15:30:20
192.168.33.46,http://www.edu360.cn/job,2017-08-05 16:30:20
192.168.33.55,http://www.edu360.cn/job,2017-08-05 15:40:20

vim access.log.2017-08-06.log

# 添加如下内容
192.168.133.3,http://www.edu360.cn/register,2017-08-06 15:30:20
192.168.111.3,http://www.edu360.cn/register,2017-08-06 15:35:20
192.168.34.44,http://www.edu360.cn/pay,2017-08-06 15:30:20
192.168.33.46,http://www.edu360.cn/excersize,2017-08-06 16:30:20
192.168.33.55,http://www.edu360.cn/job,2017-08-06 15:40:20
192.168.33.46,http://www.edu360.cn/excersize,2017-08-06 16:30:20
192.168.33.25,http://www.edu360.cn/job,2017-08-06 15:40:20
192.168.33.36,http://www.edu360.cn/excersize,2017-08-06 16:30:20
192.168.33.55,http://www.edu360.cn/job,2017-08-06 15:40:20
```

（3）向分区中导入数据

```sql
-- 导入数据
load data local inpath '/home/hadoop/hivedata/access.log.2017-08-04.log' into table t_access partition(dt='20170804');

load data local inpath '/home/hadoop/hivedata/access.log.2017-08-05.log' into table t_access partition(dt='20170805');

load data local inpath '/home/hadoop/hivedata/access.log.2017-08-06.log' into table t_access partition(dt='20170806');

-- 查看某表的所有分区
show partitions t_access;

# 结果
+--------------+
|  partition   |
+--------------+
| dt=20170804  |
| dt=20170805  |
| dt=20170806  |
+--------------+
```

![[Pasted image 20231008104115.png]]

（3）针对分区数据进行查询

```sql
-- 统计8月4号的总PV。（实质：就是将分区字段当成表字段来用，就可以使用where子句指定分区了）
select count(*) from t_access where dt='20170804';

-- 统计表中所有数据总的PV
select count(*) from t_access
```

（4）修复分区

若分区元数据和HDFS的分区路径不一致，还可使用msck命令进行修复，以下是该命令的用法说明。

```sql
msck repair table table_name [add/drop/sync partitions];
```

- msck repair table table_name add partitions：该命令会增加HDFS路径存在但元数据缺失的分区信息。
- msck repair table table_name drop partitions：该命令会删除HDFS路径已经删除但元数据仍然存在的分区信息。
- msck repair table table_name sync partitions：该命令会同步HDFS路径和元数据分区信息，相当于同时执行上述的两个命令。
- msck repair table table_name：等价于msck repair table table_name **add** partitions命令。

（5）动态分区

```sql
-- 建表
create table t_access_dynamic(ip string,url string,access_time string)
partitioned by(stat_dt string)
row format delimited fields terminated by ',';

-- 开启动态分区（默认true）
set hive.exec.dynamic.partition=true;
-- 动态分区的模式，默认strict（严格模式）
set hive.exec.dynamic.partition.mode=nonstrict;

-- 动态分区插入数据
insert overwrite table t_access_dynamic partition(stat_dt)
select ip,url,access_time,dt
from t_access;

show partitions t_access_dynamic;
```

#### 3.2.5 分桶表

分区提供一个隔离数据和优化查询的便利方式。不过，并非所有的数据集都可形成合理的分区。对于一张表或者分区，Hive 可以进一步组织成桶，也就是更为细粒度的数据范围划分，==分区针对的是数据的存储路径，分桶针对的是数据文件。==

分桶表的基本原理是，首先为每行数据计算一个指定字段的数据的hash值，然后模以一个指定的分桶数，最后将取模运算结果相同的行，写入同一个文件中，这个文件就称为一个分桶（bucket）。

```shell
cd /home/hadoop/hivedata

vim stu_buck.dat
# 添加如下数据
年级一,student1,8
年级一,student2,10
年级一,student3,6
年级一,student4,7
年级一,student5,8
年级二,student6,9
年级二,student7,10
年级一,student8,8
年级一,student9,7
年级二,student10,11
年级二,student11,13
年级三,student12,9
年级三,student13,9
年级二,student14,10
年级三,student15,12
年级三,student16,10
```

```sql
-- 建表
-- 分桶表
create table stu_buck(
grade string, 
name string,
age int
)
clustered by(grade) 
into 3 buckets
row format delimited fields terminated by ',';

-- 本地模式，默认为flase
set hive.exec.mode.local.auto=flase;
set mapreduce.job.reduces=-1;

-- 导入数据
-- Hive新版本load数据可以直接跑MapReduce，老版的Hive需要将数据传到一张表里，再通过查询的方式导入到分桶表里面。
load data local inpath '/home/hadoop/hivedata/stu_buck.dat' into table stu_buck;

-- 观察每桶的数据
```

![[Pasted image 20231009093306.png]]

```sql
-- 建表
-- 分桶有序表
create table stu_buck_sort(
grade string, 
name string,
age int
)
clustered by(grade) sorted by(age desc,name)
into 3 buckets
row format delimited fields terminated by ',';

-- 导入数据
load data local inpath '/home/hadoop/hivedata/stu_buck.dat' into table stu_buck_sort;
```

![[Pasted image 20231009094608.png]]

和非分桶表相比，分桶表的使用好处有以下几点：
1. 基于分桶字段查询时，减少全表扫描

```sql
-- 基于分桶字段grade查询来自于年级一的数据
-- 不再需要进行全表扫描过滤
-- 根据分桶的规则hash_function(年级一) mod 3计算出分桶编号
-- 查询指定分桶里面的数据就可以找出结果  此时是分桶扫描而不是全表扫描
select * from stu_buck_sort where grade = '年级一';
```

2. JOIN时可以提高MR程序效率，减少笛卡尔积数量。对于JOIN操作两个表的关联字段都是分桶字段，那么将保存相同列值的桶进行JOIN操作就可以，可以大大较少JOIN的数据量。
3. 分桶表数据进行抽样。当数据量特别大时，对全体数据进行处理存在困难时，抽样就显得尤其重要了。抽样可以从被抽取的数据中估计和推断出整体的特性，是科学实验、质量检验、社会调查普遍采用的一种经济有效的工作和研究方法。
#### 3.2.6 CTAS建表语法

（1）可以通过已存在表来建表，新建的t_user_2表结构定义与源表t_user一致，但是没有数据

```sql
create table t_access_2 like t_access;

-- 查看表信息
desc t_access_2;
```

（2）在建表的同时插入数据

```sql
create table t_access_user
as
select ip,url from t_access;
```
### 3.3 数据导入导出

#### 3.3.1 数据导入hive表

```sql
drop table if exists t_order;
create table t_order
(
 id string
,uid string
)
row format delimited fields terminated by ',';;

-- 查询表数据
select * from t_order;

-- 清空表数据
truncate table t_order;
```

```shell
cd /home/hadoop/hivedata
vim t_order.dat
```

```txt
order001,u001
order002,u001
order003,u005
order004,u002
order005,u003
```

（1）手动用hdfs命令，将文件放入表目录

```shell
hdfs dfs -put t_order.dat /user/hive/warehouse/my_test.db/t_order
```

（2）用hive命令导入数据到表目录（load）

```sql
load data [local] inpath 'filepath' [overwrite] into table tablename [partition (partcol1=val1, partcol2=val2 ...)];
```

关键字说明：
- local：表示从本地加载数据到Hive表；否则从HDFS加载数据到Hive表。
- overwrite：表示覆盖表中已有数据，否则表示追加。
- partition：表示上传到指定分区，若目标是分区表，需指定分区。

用hive命令导入本地数据到表目录

```sql
load data local inpath '/home/hadoop/hivedata/t_order.dat' into table t_order;
```

用hive命令导入hdfs数据到表目录

```shell
hdfs dfs -put t_order.dat /
```

```sql
load data inpath '/t_order.dat' into table t_order;
```

==注意：导本地文件和导HDFS文件的区别：==
- 本地文件导入表：复制
- hdfs文件导入表：移动
#### 3.3.2 hive数据导出

（1）将hive表中的数据导入HDFS的文件

```sql
insert overwrite directory '/access-data'
row format delimited fields terminated by ','
select * from t_access;
```

![[Pasted image 20231007130858.png]]

（2）将hive表中的数据导入本地目录

```sql
insert overwrite local directory '/home/hadoop/hivedata/access-data'
row format delimited fields terminated by ','
select * from t_access;
```

![[Pasted image 20231007131107.png|400]]

（3）将hive表中的数据导入另外一张hive表

语法：

```sql
insert (into | overwrit) table tablename [partition (partcol1=val1, partcol2=val2 ...)] select_statement;
```

关键字说明：
- into：将结果追加到目标表
- overwrit：用结果覆盖原有数据

```sql
-- 新建一张hive表
create table t_access_test like t_access;

-- 写入数据
insert into table t_access_test partition(dt = '20231008')
select ip,url,access_time from t_access;

show partitions t_access_test;

# 结果
+--------------+
|  partition   |
+--------------+
| dt=20231008  |
+--------------+

-- 写入数据
-- 开启动态分区
set hive.exec.dynamic.partition.mode=nonstrict;
insert into table t_access_test partition(dt)
select ip,url,access_time,dt from t_access;

show partitions t_access_test;

# 结果
+--------------+
|  partition   |
+--------------+
| dt=20170804  |
| dt=20170805  |
| dt=20170806  |
| dt=20231008  |
+--------------+
```
### 3.4 数据类型

#### 3.4.1 数字类型

| 数据类型        | 说明               | 定义                                                    |
| --------------- | ------------------ | ------------------------------------------------------- |
| tinyint         | 1byte有符号整数    | -128 to 127                                             |
| smallint        | 2byte有符号整数    | -32,768 to 32,767                                       |
| ==int==/integer | 4byte有符号整数    | -2,147,483,648 to 2,147,483,647                         |
| ==bigint==      | 8byte有符号整数    | -9,223,372,036,854,775,808 to 9,223,372,036,854,775,807 |
| float           | 4byte单精度浮点数  |                                                         |
| double          | 8byte双精度浮点数  |                                                         |
| ==decimal==     | 十进制精准数字类型 | decimal(16,2)                                           |

**注：类型转换**

Hive的基本数据类型可以做类型转换，转换的方式包括隐式转换以及显示转换。
1. 隐式转换（任何整数类型都可以隐式地转换为一个范围更广的类型）
	- tinyint可以转换成int，int可以转换成bigint
	- 所有整数类型、float和string类型都可以隐式地转换成double。
	- tinyint、smallint、int都可以转换为float
2. 显示转换（见[[#5.1.1 类型转换函数]]）
#### 3.4.2 日期时间类型

- ==timestamp== (note: only available starting with hive 0.8.0)
- ==date== (note: only available starting with hive 0.12.0)

#### 3.4.3 字符串类型

- ==string==
- varchar (note: only available starting with hive 0.12.0)
- char (note: only available starting with hive 0.13.0)

| 数据类型 | 说明                                                | 定义        |
| -------- | --------------------------------------------------- | ----------- |
| ==string==   | 字符串，无需指定最大长度                            |             |
| varchar  | 字符序列，需指定最大长度，最大长度的范围是[1,65535] | varchar(32) |
| char         |          字符序列，需指定最大长度                                           |  char(8)           |
#### 3.4.4 混杂类型

- boolean
- binary：二进制数据
#### 3.4.5 复合类型

##### 3.4.5.1 array数组类型

array<data_type> (note: negative values and non-constant expressions are allowed as of hive 0.14.)

```shell
cd /home/hadoop/hivedata

vim t_movie.dat

# 添加如下内容
战狼2,吴京:吴刚:龙母,2017-08-16
三生三世十里桃花,刘亦菲:痒痒,2017-08-20
```

```sql
-- 建表
create table t_movie(moive_name string,actors array<string>,first_show date)
row format delimited fields terminated by ','
collection items terminated by ':';

-- 导入数据
load data local inpath '/home/hadoop/hivedata/t_movie.dat' into table t_movie;

-- 查询
select * from t_movie;
select moive_name,actors[0] from t_movie;
select moive_name,actors from t_movie where array_contains(actors,'吴刚');
select moive_name,size(actors) from t_movie;
```
##### 3.4.5.2 map类型

MAP<primitive_type, data_type> (Note: negative values and non-constant expressions are allowed as of [Hive 0.14](https://issues.apache.org/jira/browse/HIVE-7325).)

```shell
cd /home/hadoop/hivedata

vim t_person.dat

# 添加如下内容
1,zhangsan,father:xiaoming#mother:xiaohuang#brother:xiaoxu,28
2,lisi,father:mayun#mother:huangyi#brother:guanyu,22
3,wangwu,father:wangjianlin#mother:ruhua#sister:jingtian,29
4,mayun,father:mayongzhen#mother:angelababy,26
```

```sql
-- 建表
create table t_person(id int,name string,family_members map<string,string>,age int)
row format delimited fields terminated by ','
collection items terminated by '#'
map keys terminated by ':';

-- 导入数据
load data local inpath '/home/hadoop/hivedata/t_person.dat' into table t_person;

-- 查询
select * from t_person;
select id,name,family_members['father'] as father from t_person;
select id,name,map_keys(family_members) as relation from t_person;
select id,name,map_values(family_members) from t_person;
select id,name,map_values(family_members)[0] from t_person;
```
##### 3.4.5.3 struct类型

struct<col_name : data_type, ...>


```shell
cd /home/hadoop/hivedata

vim t_person_struct.dat

# 添加如下内容
1,zhangsan,18:male:beijing
2,lisi,28:female:shanghai
```

```sql
-- 建表
create table t_person_struct(id int,name string,info struct<age:int,sex:string,addr:string>)
row format delimited fields terminated by ','
collection items terminated by ':';

-- 导入数据
load data local inpath '/home/hadoop/hivedata/t_person_struct.dat' into table t_person_struct;

-- 查询
select * from t_person_struct;
select id,name,info.age from t_person_struct;
```
### 3.5 表常用操作

#### 3.5.1 查看

```sql
-- 查看有哪些表
show tables;
show tables like 't_access*';

-- 查看基本信息
desc t_access;

-- 查看更多信息
desc formatted t_access;
```

#### 3.5.2 修改表

仅修改Hive元数据，不会触动表中的数据，用户需要确定实际的数据布局符合元数据的定义。

```sql
-- 重命名 alter table table_name rename to new_table_name
alter table t_access_2 rename to t_access_3;

desc t_access_3;

-- 新增列
alter table t_access_3 add columns(age int);

desc t_access_3;

-- 更新列
alter table t_access_3 change column age ages double;

desc t_access_3;

-- 替换列数据类型
alter table t_access_3 replace columns(ip varchar(50), url varchar(100));

desc t_access_3;

-- 添加分区
show partitions t_access_3;

alter table t_access_3 add partition (dt='20231007');

show partitions t_access_3;

-- 修改分区名：
alter table t_access_3 partition(dt='20231007') rename to partition(dt='20231006');

show partitions t_access_3;

-- 删除分区
alter table t_access_3 drop partition (dt='20231006');

show partitions t_access_3;
```

#### 3.5.3 删除表

```sql
-- 清空表（不能删除外部表中数据）
truncate table t_access_3;

-- 删除表
drop table if exists t_access_3;
```
## 四、hive查询语句

hive查询语句：[LanguageManual Select - Apache Hive - Apache Software Foundation](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Select)

```sql
-- select 执行顺序
select [all | distinct] select_expr, select_expr, ... -- 4.select语句
	from table_reference	-- 1.从什么表查
	[where where_condition]	-- 2.过滤条件
	[group by col_list		-- 3.分组
	[having col_list]		-- 5.分组后过滤
	[order by col_list]		-- 6.排序
	[cluster by col_list| [distribute by col_list] [sort by col_list]] -- 6.分区排序
	[limit [offset,] rows]	-- 7.限制输出的行数
```
### 4.1 基本查询

#### 4.1.1 全表和特定列查询

```sql
-- 全表查询
select * from t_access;
-- 选择特定列查询
select ip, url from t_access;
```

注意：
1. SQL 语言大小写不敏感。 
2. SQL 可以写在一行或者多行。
3. 关键字不能被缩写也不能分行。
4. 各子句一般要分行写。
5. 使用缩进提高语句的可读性。
#### 4.1.2 列别名和表别名

字段 as 列别名
字段 列别名

```sql
select
ip as ip1,
url url1
,t1.access_time
from t_access t1;
```
#### 4.1.3 limit语句

```sql
-- 显示5行
select * from t_access limit 5;
-- 表示从第2行开始，向下抓取3行
select * from t_access limit 2,3;
```
#### 4.1.4 where语句

```sql
-- 查询出日期大于20170805的数据
select * from t_access where dt > '20170805';
```
#### 4.1.5 关系运算函数

如下操作符主要用于where和having语句中。

| **操作符**              | **支持的数据类型** | **描述**                                                                                                                                                                                                                                                                 |
| ----------------------- | ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| A=B                     | 基本数据类型       | 如果A等于B则返回true，反之返回false                                                                                                                                                                                                                                      |
| A<=>B                   | 基本数据类型       | 如果A和B都为null或者都不为null，则返回true，如果只有一边为null，返回false                                                                                                                                                                                                |
| A<>B, A!=B              | 基本数据类型       | A或者B为null则返回null；如果A不等于B，则返回true，反之返回false                                                                                                                                                                                                          |
| A<B                     | 基本数据类型       | A或者B为null，则返回null；如果A小于B，则返回true，反之返回false                                                                                                                                                                                                          |
| A<=B                    | 基本数据类型       | A或者B为null，则返回null；如果A小于等于B，则返回true，反之返回false                                                                                                                                                                                                      |
| A>B                     | 基本数据类型       | A或者B为null，则返回null；如果A大于B，则返回true，反之返回false                                                                                                                                                                                                          |
| A>=B                    | 基本数据类型       | A或者B为null，则返回null；如果A大于等于B，则返回true，反之返回false                                                                                                                                                                                                      |
| A [not] between B and C | 基本数据类型       | 如果A，B或者C任一为null，则结果为null。如果A的值大于等于B而且小于或等于C，则结果为true，反之为false。如果使用not关键字则可达到相反的效果。                                                                                                                               |
| A is null               | 所有数据类型       | 如果A等于null，则返回true，反之返回false                                                                                                                                                                                                                                 |
| A is not null           | 所有数据类型       | 如果A不等于null，则返回true，反之返回false                                                                                                                                                                                                                               |
| in（数值1，数值2）      | 所有数据类型       | 使用 in运算显示列表中的值                                                                                                                                                                                                                                                |
| A [not] like B          | string 类型        | B是一个SQL下的简单正则表达式，也叫通配符模式，如果A与其匹配的话，则返回true；反之返回false。B的表达式说明如下：‘x%’表示A必须以字母‘x’开头，‘%x’表示A必须以字母‘x’结尾，而‘%x%’表示A包含有字母‘x’,可以位于开头，结尾或者字符串中间。如果使用not关键字则可达到相反的效果。 |
| A rlike B, A regexp B   | string 类型        | B是基于java的正则表达式，如果A与其匹配，则返回true；反之返回false。匹配使用的是JDK中的正则表达式接口实现的，因为正则也依据其中的规则。例如，正则表达式必须和整个字符串A相匹配，而不是只需与其字符串匹配。                                                                |
#### 4.1.6 逻辑运算函数

| **操作符** | **含义** |
| ---------- | -------- |
| and        | 逻辑并   |
| or         | 逻辑或   |
| not        | 逻辑否   |

```sql
-- 查询出日期大于20170805的数据，且 ip大于192.168.33.55
select * from t_access where dt > '20170805' and ip > '192.168.33.55';

-- 查询出日期大于20170805的数据，或者 ip大于192.168.33.55
select * from t_access where dt > '20170805' or ip > '192.168.33.55';

-- 查询除了ip为192.168.33.3 或192.168.33.55外的数据
select * from t_access where ip not in ('192.168.33.3','192.168.33.55');
```
#### 4.1.7 聚合函数

- `count(*)`，表示统计所有行数，包含null值；
- count(某列)，表示该列一共有多少行，不包含null值；
- max()，求最大值，不包含null，除非所有值都是null；
- min()，求最小值，不包含null，除非所有值都是null；
- sum()，求和，不包含null。
- avg()，求平均值，不包含null。

```sql
-- 求总行数（count）
select count(*) cnt from t_access;

-- 求访问时间的最大值（max）
select max(access_time) max_sal from t_access;

-- 求访问时间的最小值（min）
select min(access_time) min_sal from t_access;
```

![[count底层原理.excalidraw]]

### 4.2 分组

#### 4.2.1 group by 语句

Group By语句通常会和聚合函数一起使用，按照一个或者多个列队结果进行分组，然后对每个组执行聚合操作。

```sql
-- 求8月5号以后，每天每个页面的总访问次数，及访问者中ip地址中最大的
select dt,url,count(1) as cnts,max(ip) as max_ip
from t_access
where dt >= '20170805'
group by dt,url;
```

![[求平均值的底层原理.excalidraw|500]]
#### 4.2.2 having 语句

为什么where必须写在group by的前面，为什么group by后面的条件只能用having？

因为，where是用于在真正执行查询逻辑之前过滤数据用的，==having是对group by聚合之后的结果进行再过滤==

```sql
-- 求8月5号以后，每天每个页面的总访问次数，及访问者中ip地址中最大的，且只查询出总访问次数>2 的记录
select dt,url,count(1) as cnts,max(ip) as max_ip
from t_access
where dt >= '20170805'
group by dt,url
having cnts>2;
```
### 4.3 join语句

#### 4.3.1 数据准备

```shell
cd /home/hadoop/hivedata

vim t_a.dat

# 添加如下内容
a,1
b,2
c,3
d,4

vim t_b.dat

# 添加如下内容
a,xx
b,yy
d,zz
e,pp
```

```sql
-- 建表
create table t_a(name string,numb int)
row format delimited fields terminated by ',';

create table t_b(name string,nick string)
row format delimited fields terminated by ',';

-- 导入数据
load data local inpath '/home/hadoop/hivedata/t_a.dat' into table t_a;
load data local inpath '/home/hadoop/hivedata/t_b.dat' into table t_b;

-- 查询
select * from t_a;
select * from t_b;
```
#### 4.3.2 笛卡尔积

两个表中的所有行互相连接

```sql
select a.*,b.*
from t_a a 
inner join t_b b;
```

```txt
+---------+---------+---------+---------+
| a.name  | a.numb  | b.name  | b.nick  |
+---------+---------+---------+---------+
| a       | 1       | a       | xx      |
| b       | 2       | a       | xx      |
| c       | 3       | a       | xx      |
| d       | 4       | a       | xx      |
| a       | 1       | b       | yy      |
| b       | 2       | b       | yy      |
| c       | 3       | b       | yy      |
| d       | 4       | b       | yy      |
| a       | 1       | d       | zz      |
| b       | 2       | d       | zz      |
| c       | 3       | d       | zz      |
| d       | 4       | d       | zz      |
| a       | 1       | e       | pp      |
| b       | 2       | e       | pp      |
| c       | 3       | e       | pp      |
| d       | 4       | e       | pp      |
+---------+---------+---------+---------+
```

#### 4.3.3 内连接inner join（join）

显示两个表都有的数据

```sql
select a.*,b.*
from t_a a 
join t_b b 
on a.name=b.name;
```

```txt
+---------+---------+---------+---------+
| a.name  | a.numb  | b.name  | b.nick  |
+---------+---------+---------+---------+
| a       | 1       | a       | xx      |
| b       | 2       | b       | yy      |
| d       | 4       | d       | zz      |
+---------+---------+---------+---------+
```

![[join底层原理.excalidraw|500]]

大多数情况下，Hive会对每对join连接对象启动一个MapReduce任务。如果有三个表进行关联，先启动一个mapreduce程序对前两个表进行关联，后面再启动一个mapreduce对前一步结果和最后一个表进行关联。
#### 4.3.4 左连接left outer join（left join）

以左表为主表进行关联

```sql
select a.*,b.*
from t_a a 
left join t_b b 
on a.name=b.name;
```

```txt
+---------+---------+---------+---------+
| a.name  | a.numb  | b.name  | b.nick  |
+---------+---------+---------+---------+
| a       | 1       | a       | xx      |
| b       | 2       | b       | yy      |
| c       | 3       | NULL    | NULL    |
| d       | 4       | d       | zz      |
+---------+---------+---------+---------+
```
#### 4.3.5 右连接right outer join（right join）

以右表为主表进行关联

```sql
select a.*,b.*
from t_a a 
right join t_b b 
on a.name=b.name;
```

```txt
+---------+---------+---------+---------+
| a.name  | a.numb  | b.name  | b.nick  |
+---------+---------+---------+---------+
| a       | 1       | a       | xx      |
| b       | 2       | b       | yy      |
| d       | 4       | d       | zz      |
| NULL    | NULL    | e       | pp      |
+---------+---------+---------+---------+
```
#### 4.3.6 全连接full outer join（full join）

两表所有数据进行连接

```sql
select a.*,b.*
from t_a a 
full join t_b b 
on a.name=b.name;
```

```txt
+---------+---------+---------+---------+
| a.name  | a.numb  | b.name  | b.nick  |
+---------+---------+---------+---------+
| a       | 1       | a       | xx      |
| b       | 2       | b       | yy      |
| c       | 3       | NULL    | NULL    |
| d       | 4       | d       | zz      |
| NULL    | NULL    | e       | pp      |
+---------+---------+---------+---------+
```
#### 4.3.7 左半连接left semi join

显示左表有，右表也有的数据，但是右表的字段不显示

==注意： left semi join的 select子句中，不能有右表的字段==

```sql
select a.*
from t_a a 
left semi join t_b b 
on a.name=b.name;

-- 相当于
select a.*
from t_a a 
left join t_b b 
on a.name=b.name
where b.name is not null;
```

```txt
+---------+---------+
| a.name  | a.numb  |
+---------+---------+
| a       | 1       |
| b       | 2       |
| d       | 4       |
+---------+---------+
```

#### 4.3.8 联合（union & union all）

union和union all都是上下拼接sql的结果，这点是和join有区别的，join是左右关联，union和union all是上下拼接。==union去重，union all不去重。==

union和union all在上下拼接sql结果时有两个要求：
1. 两个sql的结果，列的个数必须相同
2. 两个sql的结果，上下所对应列的类型必须一致

```sql
select name,cast(numb as string) as nick from t_a
union all
select name,nick from t_b;
```

```txt
+-----------+-----------+
| _u1.name  | _u1.nick  |
+-----------+-----------+
| a         | xx        |
| b         | yy        |
| d         | zz        |
| e         | pp        |
| a         | 1         |
| b         | 2         |
| c         | 3         |
| d         | 4         |
+-----------+-----------+
```
### 4.4 子查询

```sql
-- 求8月5号以后，每天每个页面的总访问次数，及访问者中ip地址中最大的，且只查询出总访问次数>2 的记录
select *
from
(select dt,url,count(1) as cnts,max(ip) as max_ip
from t_access
where dt >= '20170805'
group by dt,url) t1
where t1.cnts>2;
```
### 4.5 排序

#### 4.5.1 全局排序（order by）

oder by：全局排序，只有一个Reduce。
- asc（ascend）：升序（默认）
- desc（descend）：降序

```sql
-- 升序
select * from t_access order by dt;

-- 降序
select * from t_access order by dt desc;

-- 多字段,按dt升序，dt相同按ip降序
select * from t_access order by dt,ip desc;
```
#### 4.5.2 每个reduce内部排序（sort by）

sort by：对于大规模的数据集order by的效率非常低。在很多情况下，并不需要全局排序，此时可以使用**sort by**。

sort by为==每个reduce产生一个排序文件==。每个Reduce内部进行排序，对全局结果集来说不是排序。

```sql
-- 关掉本地模式，默认为flase
set hive.exec.mode.local.auto=flase;


--查看设置reducere个数，默认mapreduce.job.reduces=-1 
set mapreduce.job.reduces;

-- 设置reduce个数
set mapreduce.job.reduces=2;

-- 把结果写入本地文件
-- 根据ip升序
insert overwrite local directory '/home/hadoop/hivedata/sortby-result'
row format delimited fields terminated by ','
select * from t_access sort by ip;

```

#### 4.5.3 分区（distribute by）

Distribute By：在有些情况下，我们需要控制某个特定行应该到哪个Reducer，通常是为了进行后续的聚集操作。**distribute by**子句可以做这件事。==distribute by类似MapReduce中partition（自定义分区）==，进行分区，结合sort by使用。 

对于distribute by进行测试，一定要分配多reduce进行处理，否则无法看到distribute by的效果。

```sql
-- 关掉本地模式，默认为flase
set hive.exec.mode.local.auto=flase;


--查看设置reducere个数，默认mapreduce.job.reduces=-1 
set mapreduce.job.reduces;

-- 设置reduce个数
set mapreduce.job.reduces=2;

-- 把结果写入本地文件
-- 按dt分区，分区内按照ip升序排序
insert overwrite local directory '/home/hadoop/hivedata/distribute-result'
row format delimited fields terminated by ','
select * from t_access distribute by dt sort by ip;
```

注意：
-  distribute by的分区规则是根据分区字段的hash码与reduce的个数进行相除后，余数相同的分到一个区。
-  Hive要求**distribute by**语句要写在sort by语句之前。
- 演示完以后mapreduce.job.reduces的值要设置回-1，否则分区or分桶表load跑MapReduce的时候会报错。

#### 4.5.4 分区排序（cluster by）

当distribute by和sort by字段相同时，可以使用cluster by方式。

cluster by除了具有distribute by的功能外还兼具sort by的功能。但是排序只能是升序排序，不能指定排序规则为asc或者desc。

```sql
-- 关掉本地模式，默认为flase
set hive.exec.mode.local.auto=flase;


--查看设置reducere个数，默认mapreduce.job.reduces=-1 
set mapreduce.job.reduces;

-- 设置reduce个数
set mapreduce.job.reduces=2;

-- 把结果写入本地文件
-- 按dt分区，分区内按照dt升序排序
insert overwrite local directory '/home/hadoop/hivedata/cluster-result'
row format delimited fields terminated by ','
select * from t_access cluster by dt;

-- 等效
insert overwrite local directory '/home/hadoop/hivedata/distribute-result'
row format delimited fields terminated by ','
select * from t_access distribute by dt sort by dt;
```
## 五、hive函数使用

Hive会将常用的逻辑封装成**函数**给用户进行使用，类似于Java中的函数。

**好处**：避免用户反复写逻辑，可以直接拿来使用。

**重点**：用户需要知道函数叫什么，能做什么。

Hive提供了大量的内置函数，按照其特点可大致分为如下几类：单行函数、聚合函数、炸裂函数、窗口函数。


hive所有的函数手册：[LanguageManual UDF - Apache Hive - Apache Software Foundation](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF)

```sql
-- 查看系统内置函数
show functions;

-- 查看内置函数用法
desc function upper;

-- 查看内置函数详细信息
desc function extended upper;
```
### 5.1 单行函数

==单行函数的特点是一进一出，即输入一行，输出一行。==
#### 5.1.1 类型转换函数

```sql
select cast("5" as int);
select cast("2017-08-03" as date);
select cast(current_timestamp as date);
```
#### 5.1.2 算术运算函数


| **运算符** | **描述**       |
| ---------- | -------------- |
| A+B        | A和B 相加      |
| A-B        | A减去B         |
| `A*B`        | A和B 相乘      |
| A/B        | A除以B         |
| A%B        | A对B取余       |
| A&B        | A和B按位取与   |
| A\|B       | A和B按位取或   |
| A^B        | A和B按位取异或 |
| ~A         | A按位取反      |

```sql
select 3 + 1;
```
#### 5.1.3 数值函数

```sql
select round(5.4) ;  --  5 四舍五入取整
select round(5.1345,3);  -- 5.135 四舍五入保留三位小数
select ceil(5.4); -- 6 向上取整
select floor(5.4); -- 5 向下取整
select abs(-5.4);  -- 5.4 取绝对值
select greatest(3,5,6);  --  6 取最大值
select least(3,5,6); -- 3 取最小值
```
#### 5.1.4 字符串函数

##### 5.1.4.1 字符串截取

substr(string A, int start)

substr(string A, int start, int len)

```sql
-- bcdefg。从第二个开始截取
select substr("abcdefg",2);
-- fg。从倒数第二个截取
select substr("abcdefg",-2);
-- bcd。从第二个开始截取，取3个
select substr("abcdefg",2,3);
```
##### 5.1.4.2 替换

replace(string A, string B, string C)：将字符串A中的子字符串B替换为C

regexp_replace(string A, string B, string C)：将字符串A中的符合java正则表达式B的部分替换为C。正则表达式见[[常用正则表达式]]

```sql
select replace('zhangsan','san','si') as a,regexp_replace('nihao,1234222','[0-9]','.') as b,regexp_replace('100-200', '(\\d+)', 'num') as c;
```

```txt
+----------+----------------+----------+
|    a     |       b        |    c     |
+----------+----------------+----------+
| zhangsi  | nihao,.......  | num-num  |
+----------+----------------+----------+
```

##### 5.1.4.3 正则匹配

regexp：若字符串符合正则表达式，则返回true，否则返回false。正则表达式见[[常用正则表达式]]

```sql
select 'dfsaaaa' regexp 'dfsa+' as a,'dfsaaaa' regexp 'dfsb+' as b;
```

```txt
+-------+--------+
|   a   |   b    |
+-------+--------+
| true  | false  |
+-------+--------+
```
##### 5.1.4.4 拼接字符串

concat(string A, string B, string C, ……)：将A,B,C……等字符拼接为一个字符串

concat_ws(string A, string…| array(string))：使用分隔符A拼接多个字符串，或者一个数组的所有元素。

```sql
-- abxy。有null值拼接，结果为null
select concat("ab","xy");
-- NULL
select concat("ab",null);

-- 192.168.33.44。有null值拼接，跳过null
select concat_ws(".","192","168","33","44");
-- 168.33.44
select concat_ws(".",null,"168","33","44");
-- beijing-shenzhen-shanghai
select concat_ws('-',array('beijing','shenzhen','shanghai'));
```
##### 5.1.4.5 替换null值

nvl(A,B)：若A的值不为null，则返回A，否则返回B。

coalesce(A,B,C...)：若A的值不为null，则返回A，否则继续往下查看。

```sql
select nvl(1,2) as a,nvl(null,3) as b,coalesce('aa',null) as c,coalesce(null,null,'haha') as d;
```

```txt
+----+----+-----+-------+
| a  | b  |  c  |   d   |
+----+----+-----+-------+
| 1  | 3  | aa  | haha  |
+----+----+-----+-------+
```

##### 5.1.4.6 字符串切割

split(string str, string pat)：按照正则表达式pat匹配到的内容分割str，分割后的字符串，以数组的形式返回。正则表达式见[[常用正则表达式]]

```sql
-- ["a","b","c","d"]
select split('a-b-c-d','-');
```
##### 5.1.4.7 重复字符串

repeat(string A, int n)：将字符串A重复n遍

```sql
-- 123123123
select repeat('123', 3);
```

##### 5.1.4.8 字符串长度

```sql
-- 13
select length("192.168.33.44");
```

##### 5.1.4.9 解析json字符串

get_json_object(string json_string, string path)：解析json的字符串json_string，返回path指定的内容。如果输入的json字符串无效，那么返回NULL。

```sql
-- 获取json数组里面的数据
--  {"name":"大海海","sex":"男","age":"25"} 
select get_json_object('[{"name":"大海海","sex":"男","age":"25"},{"name":"小宋宋","sex":"男","age":"47"}]','$.[0]');

-- 获取json具体数据
-- 大海海
select get_json_object('[{"name":"大海海","sex":"男","age":"25"},{"name":"小宋宋","sex":"男","age":"47"}]','$.[0].name');
```

解析json生成一张表

```sql
select json_tuple('{"name":"大海海","sex":"男","age":"25"}','name','sex','age') as(name,sex,age);
```

```txt
+-------+------+------+
| name  | sex  | age  |
+-------+------+------+
| 大海海   | 男    | 25   |
+-------+------+------+
```
#### 5.1.5 日期函数

##### 5.1.5.1 获取日期

current_timestamp：当前的日期加时间，并且精确的毫秒

current_date：当前日期

unix_timestamp：返回当前或指定时间的时间戳

```sql
-- 获取当前时间 2023-10-07 17:37:17.958 
select current_timestamp;
-- 获取当前日期 2023-10-07 
select current_date;
-- 获取当前时间戳 1696671439
select unix_timestamp();
```

##### 5.1.5.2 日期和字符串互相转换

from_unixtime(bigint unixtime[, string format])：转化UNIX时间戳（从 1970-01-01 00:00:00 UTC 到指定时间的秒数）到当前时区的时间格式

date_format：将标准日期解析成指定格式字符串

时间戳转字符串：

```sql
-- 2023-10-07 09:38:55
select from_unixtime(unix_timestamp());
--  2023/10/07 09:39:00
select from_unixtime(unix_timestamp(),"yyyy/MM/dd HH:mm:ss");
```

标准日期解析成字符串：

```sql
--  2020年-03月-02日
select date_format('2020-03-02','yyyy年-MM月-dd日');
```

字符串转unix时间戳：

```sql
-- 1502387430
select unix_timestamp("2017-08-10 17:50:30");
-- 1502387430
select unix_timestamp("2017/08/10 17:50:30","yyyy/MM/dd HH:mm:ss");
```

字符串转成时间：

```sql
-- 2017-09-17 
select to_date("2017-09-17 16:58:32");
-- 2017-09-17 00:00:00.0
select timestamp('2017-09-17');

select cast('2017-09-17' as date) as a,cast('2017-09-17' as timestamp);
```
##### 5.1.5.3 日期运算

month (string date)：获取日期中的月

day (string date)：获取日期中的日
hour (string date)：获取日期中的小时

datediff(string enddate, string startdate)：两个日期相差的天数（结束日期减去开始日期的天数

date_add(string startdate, int days)：日期加天数

date_sub (string startdate, int days)：日期减天数

```sql
-- 8
select month('2022-08-09 07:11:00');
-- 9
select day('2022-08-09 07:11:00');
-- 7
select hour('2022-08-09 07:11:00');
```

```sql
--  'yyyy-MM-dd HH:mm:ss' or 'yyyy-MM-dd'
-- 9
select datediff('2022-10-10','2022-10-01');
-- 2022-08-10
select date_add('2022-08-08',2);
-- 2022-08-06 
select date_sub('2022-08-08',2);

-- 取上个月
-- 2022-07-08
select add_months('2022-08-08',-1)
```
#### 5.1.6 流程控制函数

```sql
-- old
select case when age<28 then 'youngth'
when age>27 and age<40 then 'zhongnian'
else 'old' end as yy
from (select 50 as age) t1;
```

```sql
-- zhoumo
select case 7 when 6 then 'zhoumo'
when 7 then 'zhoumo'
else 'gongzuori' end as yy;
```

```sql
-- working
select if(50 > 25,'working','worked');
```
#### 5.1.7 集合函数

##### 5.1.7.1 array集合

声明array：

```sql
-- ["1","2","3","4"]
select array('1','2','3','4');
-- 1
select array('1','2','3','4')[0];
```

`array_contains(Array<T>, value)`  数组是否存在某个值，返回boolean值

```sql
-- true
select array_contains(array('a','b','c'),'c');
-- false
select array_contains(array('a','b','c'),'d');
```

`sort_array(Array<T>)` 返回排序后的数组

```sql
--  ["a","b","c","d"]
select sort_array(array('b','d','a','c'));
```

`size(Array<T>)`  返回一个int值

```sql
-- 4
select size(array('b','d','a','c'));
```

##### 5.1.7.2 map集合

声明map：

```sql
-- {"xiaohai":"男","dahai":"女"} 
select map('xiaohai','男','dahai','女');
-- 男
select map('xiaohai','男','dahai','女')['xiaohai'];
```

map_keys(Map<K.V>)  返回一个数组

```sql
-- ["xiaohai","dahai"]
select map_keys(map('xiaohai','男','dahai','女'));
```

map_values(Map<K.V>) 返回一个数组

```sql
-- ["男","女"]
select map_values(map('xiaohai','男','dahai','女'));
```

size(Map<K.V>)  返回一个int值

```sql
-- 2
select size(map('xiaohai','男','dahai','女'));
```
##### 5.1.7.3 struct集合

```sql'
-- {"name":"xiaosong","age":18,"weight":80} 
select named_struct('name','xiaosong','age',18,'weight',80);

-- xiaosong
select named_struct('name','xiaosong','age',18,'weight',80).name;
```
#### 5.1.8 单行函数-案例演示

##### 5.1.8.1 数据准备

```sql
-- 建表
create table employee(
name string, -- 姓名
sex string, -- 性别
birthday string, -- 出生年月
hiredate string, -- 入职日期
job string, -- 岗位
salary double, -- 薪资
bonus double, -- 奖金
friends array<string>, -- 朋友
children map<string,int> -- 孩子
);

-- 导入数据
insert into employee  
  values('张无忌','男','1980/02/12','2022/08/09','销售',3000,12000,array('阿朱','小昭'),map('张小无',8,'张小忌',9)),
        ('赵敏','女','1982/05/18','2022/09/10','行政',9000,2000,array('阿三','阿四'),map('赵小敏',8)),
        ('宋青书','男','1981/03/15','2022/04/09','研发',18000,1000,array('王五','赵六'),map('宋小青',7,'宋小书',5)),
        ('周芷若','女','1981/03/17','2022/04/10','研发',18000,1000,array('王五','赵六'),map('宋小青',7,'宋小书',5)),
        ('郭靖','男','1985/03/11','2022/07/19','销售',2000,13000,array('南帝','北丐'),map('郭芙',5,'郭襄',4)),
        ('黄蓉','女','1982/12/13','2022/06/11','行政',12000,null,array('东邪','西毒'),map('郭芙',5,'郭襄',4)),
        ('杨过','男','1988/01/30','2022/08/13','前台',5000,null,array('郭靖','黄蓉'),map('杨小过',2)),
        ('小龙女','女','1985/02/12','2022/09/24','前台',6000,null,array('张三','李四'),map('杨小过',2));

-- 查看表数据
select * from employee;
```

##### 5.1.8.2 需求与实现

==统计每个月的入职人数==

```sql
select substr(replace(hiredate,'/','-'),1,7) as year_month
,count(1) as cn
from employee
group by substr(replace(hiredate,'/','-'),1,7)
;
```

```txt
+-------------+-----+
| year_month  | cn  |
+-------------+-----+
| 2022-04     | 2   |
| 2022-06     | 1   |
| 2022-07     | 1   |
| 2022-08     | 2   |
| 2022-09     | 2   |
+-------------+-----+
```

==查询每个人的年龄（岁 +月）==

```sql
select t1.name,t1.birthday,concat(year_age,'岁',month_age,'月') as age
from(
select name
,birthday,int(months_between(current_date,replace(birthday,'/','-'))/12) as year_age
,int(months_between(current_date,replace(birthday,'/','-'))%12) as month_age
from employee
)t1
;
```

```txt
+----------+--------------+--------+
| t1.name  | t1.birthday  |  age   |
+----------+--------------+--------+
| 张无忌      | 1980/02/12   | 43岁7月  |
| 赵敏       | 1982/05/18   | 41岁4月  |
| 宋青书      | 1981/03/15   | 42岁6月  |
| 周芷若      | 1981/03/17   | 42岁6月  |
| 郭靖       | 1985/03/11   | 38岁6月  |
| 黄蓉       | 1982/12/13   | 40岁9月  |
| 杨过       | 1988/01/30   | 35岁8月  |
| 小龙女      | 1985/02/12   | 38岁7月  |
+----------+--------------+--------+
```

==按照薪资，奖金的和进行倒序排序，如果奖金为null，置位0==

```
select name,salary,bonus,(nvl(salary,0) + nvl(bonus,0)) as total
from employee
order by total desc
;
```

```txt
+-------+----------+----------+----------+
| name  |  salary  |  bonus   |  total   |
+-------+----------+----------+----------+
| 周芷若   | 18000.0  | 1000.0   | 19000.0  |
| 宋青书   | 18000.0  | 1000.0   | 19000.0  |
| 郭靖    | 2000.0   | 13000.0  | 15000.0  |
| 张无忌   | 3000.0   | 12000.0  | 15000.0  |
| 黄蓉    | 12000.0  | NULL     | 12000.0  |
| 赵敏    | 9000.0   | 2000.0   | 11000.0  |
| 小龙女   | 6000.0   | NULL     | 6000.0   |
| 杨过    | 5000.0   | NULL     | 5000.0   |
+-------+----------+----------+----------+
```

==查询每个人有多少个朋友==

```sql
select name,friends,size(friends) as friends_cn
from employee
;
```

```txt
+-------+--------------+-------------+
| name  |   friends    | friends_cn  |
+-------+--------------+-------------+
| 张无忌   | ["阿朱","小昭"]  | 2           |
| 赵敏    | ["阿三","阿四"]  | 2           |
| 宋青书   | ["王五","赵六"]  | 2           |
| 周芷若   | ["王五","赵六"]  | 2           |
| 郭靖    | ["南帝","北丐"]  | 2           |
| 黄蓉    | ["东邪","西毒"]  | 2           |
| 杨过    | ["郭靖","黄蓉"]  | 2           |
| 小龙女   | ["张三","李四"]  | 2           |
+-------+--------------+-------------+
```

==查询每个人的孩子的姓名==

```txt
select name,children,map_keys(children) as children_name
from employee
;
```

```txt
+-------+--------------------+----------------+
| name  |      children      | children_name  |
+-------+--------------------+----------------+
| 张无忌   | {"张小无":8,"张小忌":9}  | ["张小无","张小忌"]  |
| 赵敏    | {"赵小敏":8}          | ["赵小敏"]        |
| 宋青书   | {"宋小青":7,"宋小书":5}  | ["宋小青","宋小书"]  |
| 周芷若   | {"宋小青":7,"宋小书":5}  | ["宋小青","宋小书"]  |
| 郭靖    | {"郭芙":5,"郭襄":4}    | ["郭芙","郭襄"]    |
| 黄蓉    | {"郭芙":5,"郭襄":4}    | ["郭芙","郭襄"]    |
| 杨过    | {"杨小过":2}          | ["杨小过"]        |
| 小龙女   | {"杨小过":2}          | ["杨小过"]        |
+-------+--------------------+----------------+
```

==查询每个岗位男女各多少人==

```sql
select job
,sum(case when sex='男' then 1 else 0 end) as man_cn
,sum(case when sex='女' then 1 else 0 end) as female_cn
from employee
group by job
;
```

```txt
+------+---------+------------+
| job  | man_cn  | female_cn  |
+------+---------+------------+
| 前台   | 1       | 1          |
| 研发   | 1       | 1          |
| 行政   | 0       | 2          |
| 销售   | 2       | 0          |
+------+---------+------------+
```

### 5.2 高级聚合函数

#### 5.2.1 高级聚合函数

多进一出 （多行传入，一个行输出）

普通聚合见[[#4.1.7 聚合函数]]

collect_list 收集并形成list集合，结果不去重

collect_set 收集并形成set集合，结果去重

```sql
select sex, collect_list(job)
from employee
group by sex
;

select sex, collect_set(job)
from employee
group by sex
;
```

```txt
-- collect_list
+------+------------------------+
| sex  |          _c1           |
+------+------------------------+
| 女    | ["行政","研发","行政","前台"]  |
| 男    | ["销售","研发","销售","前台"]  |
+------+------------------------+

-- collect_set
+------+-------------------+
| sex  |        _c1        |
+------+-------------------+
| 女    | ["行政","研发","前台"]  |
| 男    | ["销售","研发","前台"]  |
+------+-------------------+
```
#### 5.2.2 高级聚合函数-案例演示

==每个月的入职人数以及姓名==

```sql
select substr(replace(hiredate,'/','-'),1,7) as year_month
,count(1) as cn
,collect_list(name)
from employee
group by substr(replace(hiredate,'/','-'),1,7)
;
```

```txt
+-------------+-----+----------------+
| year_month  | cn  |      _c2       |
+-------------+-----+----------------+
| 2022-04     | 2   | ["宋青书","周芷若"]  |
| 2022-06     | 1   | ["黄蓉"]         |
| 2022-07     | 1   | ["郭靖"]         |
| 2022-08     | 2   | ["张无忌","杨过"]   |
| 2022-09     | 2   | ["赵敏","小龙女"]   |
+-------------+-----+----------------+
```
### 5.3 炸裂函数

UDTF(Table-Generating Functions)，接收一行数据，输出一行或多行数据。

![[UDTF.excalidraw|300]]
#### 5.3.1 数据准备

```shell
cd /home/hadoop/hivedata
vim t_stu_subject.dat

# 添加如下数据
1,zhangsan,化学:物理:数学:语文
2,lisi,化学:数学:生物:生理:卫生
3,wangwu,化学:语文:英语:体育:生物
```

```sql
-- 建表
create table t_stu_subject(id int,name string,subjects array<string>)
row format delimited fields terminated by ','
collection items terminated by ':';

-- 导入数据
load data local inpath '/home/hadoop/hivedata/t_stu_subject.dat' into table t_stu_subject;

-- 查看数据
select * from t_stu_subject;
```
#### 5.3.2 炸裂函数：explode()

```sql
select explode(subjects)
from t_stu_subject;
```

生成一张表

```txt
+------+
| col  |
+------+
| 化学   |
| 物理   |
| 数学   |
| 语文   |
| 化学   |
| 数学   |
| 生物   |
| 生理   |
| 卫生   |
| 化学   |
| 语文   |
| 英语   |
| 体育   |
| 生物   |
+------+
```
#### 5.3.3 lateral view

理解： lateral view 相当于两个表在join
- 左表：是原表
- 右表：是explode(某个集合字段)之后产生的表
- 而且：这个join只在同一行的数据间进行

```sql
select t1.id,t1.name,t2.subject
from t_stu_subject t1
lateral view explode(t1.subjects) t2 as subject;
```

```txt
+--------+-----------+-------------+
| t1.id  |  t1.name  | t2.subject  |
+--------+-----------+-------------+
| 1      | zhangsan  | 化学          |
| 1      | zhangsan  | 物理          |
| 1      | zhangsan  | 数学          |
| 1      | zhangsan  | 语文          |
| 2      | lisi      | 化学          |
| 2      | lisi      | 数学          |
| 2      | lisi      | 生物          |
| 2      | lisi      | 生理          |
| 2      | lisi      | 卫生          |
| 3      | wangwu    | 化学          |
| 3      | wangwu    | 语文          |
| 3      | wangwu    | 英语          |
| 3      | wangwu    | 体育          |
| 3      | wangwu    | 生物          |
+--------+-----------+-------------+
```
### 5.4 窗口函数

窗口函数，能为==每行数据划分一个窗口，然后对窗口范围内的数据进行计算==，最后将计算结果返回给该行数据。
#### 5.4.1 数据准备

```shell
cd /home/hadoop/hivedata

vim business.dat

# 添加如下内容
neil,2017-01-08,55
mart,2017-04-08,62
mart,2017-04-09,68
neil,2017-05-10,12
mart,2017-04-11,75
neil,2017-06-12,80
mart,2017-04-13,94

vim score_info.dat

# 添加如下内容
1,语文,99
2,语文,98
3,语文,95
4,数学,100
5,数学,100
6,数学,99
```

```sql
-- 建表
create table business(name string,orderdate string,cost int)
row format delimited fields terminated by ',';

create table score_info(stu_id int,course string,score int)
row format delimited fields terminated by ',';

-- 导入数据
load data local inpath '/home/hadoop/hivedata/business.dat' overwrite into table business;
load data local inpath '/home/hadoop/hivedata/score_info.dat' into table score_info;

-- 查看数据
select * from business;
select * from score_info;
```
#### 5.4.2 窗口函数-聚合函数

- max：最大值。
- min：最小值。
- sum：求和。
- avg：平均值。
- count：计数


```sql
select name,orderdate,cost, 
--所有行相加 
sum(cost) over() as sample1,
-- 按name分组，组内数据相加 
sum(cost) over(partition by name) as sample2,
--按name分组，组内数据按订单时间升序累加
sum(cost) over(partition by name order by orderdate) as sample3, 
-- 和sample3一样,由起点到当前行的聚合
sum(cost) over(partition by name order by orderdate rows between unbounded preceding and current row ) as sample4 ,
--  当前行和前面一行做聚合 
sum(cost) over(partition by name order by orderdate rows between 1 preceding and current row) as sample5,
-- 当前行和前边一行及后面一行 
sum(cost) over(partition by name order by orderdate rows between 1 preceding and 1 following ) as sample6,
-- 当前行及后面所有行
sum(cost) over(partition by name order by orderdate rows between current row and unbounded following) as sample7  
from business;
```

```txt
+-------+-------------+-------+----------+----------+----------+----------+----------+----------+----------+
| name  |  orderdate  | cost  | sample1  | sample2  | sample3  | sample4  | sample5  | sample6  | sample7  |
+-------+-------------+-------+----------+----------+----------+----------+----------+----------+----------+
| mart  | 2017-04-08  | 62    | 446      | 299      | 62       | 62       | 62       | 130      | 299      |
| mart  | 2017-04-09  | 68    | 446      | 299      | 130      | 130      | 130      | 205      | 237      |
| mart  | 2017-04-11  | 75    | 446      | 299      | 205      | 205      | 143      | 237      | 169      |
| mart  | 2017-04-13  | 94    | 446      | 299      | 299      | 299      | 169      | 169      | 94       |
| neil  | 2017-01-08  | 55    | 446      | 147      | 55       | 55       | 55       | 67       | 147      |
| neil  | 2017-05-10  | 12    | 446      | 147      | 67       | 67       | 67       | 147      | 92       |
| neil  | 2017-06-12  | 80    | 446      | 147      | 147      | 147      | 92       | 92       | 80       |
+-------+-------------+-------+----------+----------+----------+----------+----------+----------+----------+
```
#### 5.4.3 窗口函数-跨行取值函数

```sql
select name,orderdate,cost, 
-- 取前面一行值，如果没有值给默认值为'1900-01-01'
lag(orderdate,1,'1900-01-01') over(partition by name order by orderdate ) as time1, 
-- 取前面第二行值
lag(orderdate,2) over (partition by name order by orderdate) as time2,
-- 取后面一行的值
lead(orderdate,1) over (partition by name order by orderdate) as time2
from business;
```

```txt
+-------+-------------+-------+-------------+-------------+-------------+
| name  |  orderdate  | cost  |    time1    |    time2    |    time2    |
+-------+-------------+-------+-------------+-------------+-------------+
| mart  | 2017-04-08  | 62    | 1900-01-01  | NULL        | 2017-04-09  |
| mart  | 2017-04-09  | 68    | 2017-04-08  | NULL        | 2017-04-11  |
| mart  | 2017-04-11  | 75    | 2017-04-09  | 2017-04-08  | 2017-04-13  |
| mart  | 2017-04-13  | 94    | 2017-04-11  | 2017-04-09  | NULL        |
| neil  | 2017-01-08  | 55    | 1900-01-01  | NULL        | 2017-05-10  |
| neil  | 2017-05-10  | 12    | 2017-01-08  | NULL        | 2017-06-12  |
| neil  | 2017-06-12  | 80    | 2017-05-10  | 2017-01-08  | NULL        |
+-------+-------------+-------+-------------+-------------+-------------+
```

```sql
select name,orderdate,cost, 
-- 取第一行的值,false是否跳过null，默认不跳过
first_value(orderdate,false) over(partition by name order by orderdate ) as first_time, 
-- 取最后一行的值，按照开窗原理，取当前行，因为当前行是最后一行
last_value(orderdate) over (partition by name order by orderdate) as last_time,
-- 取第一行的值
first_value(orderdate) over (partition by name order by orderdate desc) as last_time2
from business;
```

```txt
+-------+-------------+-------+-------------+-------------+-------------+
| name  |  orderdate  | cost  | first_time  |  last_time  | last_time2  |
+-------+-------------+-------+-------------+-------------+-------------+
| mart  | 2017-04-13  | 94    | 2017-04-08  | 2017-04-13  | 2017-04-13  |
| mart  | 2017-04-11  | 75    | 2017-04-08  | 2017-04-11  | 2017-04-13  |
| mart  | 2017-04-09  | 68    | 2017-04-08  | 2017-04-09  | 2017-04-13  |
| mart  | 2017-04-08  | 62    | 2017-04-08  | 2017-04-08  | 2017-04-13  |
| neil  | 2017-06-12  | 80    | 2017-01-08  | 2017-06-12  | 2017-06-12  |
| neil  | 2017-05-10  | 12    | 2017-01-08  | 2017-05-10  | 2017-06-12  |
| neil  | 2017-01-08  | 55    | 2017-01-08  | 2017-01-08  | 2017-06-12  |
+-------+-------------+-------+-------------+-------------+-------------+
```
#### 5.4.4 窗口函数-排名函数

```sql
select course,
score,
rank() over(partition by course order by score desc) rk,
dense_rank() over(partition by course order by score desc) dense_rn,
row_number() over(partition by course order by score desc) rn
from score_info;
```

```txt
+---------+--------+-----+-----------+-----+
| course  | score  | rk  | dense_rn  | rn  |
+---------+--------+-----+-----------+-----+
| 数学      | 100    | 1   | 1         | 1   |
| 数学      | 100    | 1   | 1         | 2   |
| 数学      | 99     | 3   | 2         | 3   |
| 语文      | 99     | 1   | 1         | 1   |
| 语文      | 98     | 2   | 2         | 2   |
| 语文      | 95     | 3   | 3         | 3   |
+---------+--------+-----+-----------+-----+
```
### 5.5 自定义函数

根据用户自定义函数类别分为以下三种：
1. UDF（User-Defined-Function）：一进一出。
2. UDAF（User-Defined Aggregation Function）：用户自定义聚合函数，多进一出。类似于：count/max/min
3. UDTF（User-Defined Table-Generating Functions）：用户自定义表生成函数，一进多出。如lateral view explode()

官方文档地址：[HivePlugins - Apache Hive - Apache Software Foundation](https://cwiki.apache.org/confluence/display/Hive/HivePlugins)

hive中如何定义自己的函数：
1、先写一个java类，实现你所想要的函数的功能
2、将java程序打成jar包，上传到hive所在的机器

（1）创建临时函数

```sql
-- 将jar包添加到hive的classpath，临时生效
add jar /home/hadoop/hivedata/hive-udf-1.0-SNAPSHOT.jar;

-- 创建临时函数与开发好的java class关联
create temporary function my_len as "com.bigdata.udf.MyUDF";

-- 使用函数
select my_len('hello');

-- 删除函数
drop temporary function my_len;
```

注意：临时函数只跟会话有关系，跟库没有关系。==只要创建临时函数的会话不断，在当前会话下，任意一个库都可以使用==，==其他会话全都不能使用==。

（2）创建永久函数

注意：因为add jar本身也是临时生效，所以在创建永久函数的时候，需要制定路径（并且因为元数据的原因，这个路径还得是HDFS上的路径）。

```shell
hadoop fs -mkdir -p /udf
hadoop fs -put hive-udf-1.0-SNAPSHOT.jar /udf
```

```sql
-- 创建永久函数
create function my_len2 as "com.bigdata.udf.MyUDF" using jar "hdfs://hadoop101:8020/udf/hive-udf-1.0-SNAPSHOT.jar";

-- 使用函数
select my_len2('hello');

-- 删除永久函数
drop function my_len2;
```

注意：永久函数跟会话没有关系，创建函数的会话断了以后，其他会话也可以使用。永久函数创建的时候，在函数名之前需要自己加上库名，如果不指定库名的话，会默认把当前库的库名给加上。

永久函数使用的时候，需要在指定的库里面操作，或者在其他库里面使用的话加上，**库名.函数名。**
## 六、文件格式和压缩

### 6.1 文件格式

hive支持很多种文件格式：text file | sequence file  | parquet | orc

text file 和 sequence file都是行存储；parquet和orc都是列存储；text file是默认的存储格式。

（1）先建一个存储文本文件的表

```sql
create table t_access_text(ip string,url string,access_time string)
row format delimited fields terminated by ','
stored as textfile;

-- 导入文本数据到表中：
load data local inpath '/home/hadoop/hivedata/access-data/000000_0' into table t_access_text;

select * from t_access_text;
```

（2）建一个存储sequence file文件的表

```sql
create table t_access_seq(ip string,url string,access_time string)
stored as sequencefile;

-- 从文本表中查询数据插入sequencefile表中，生成数据文件就是sequencefile格式的了
insert into t_access_seq
select * from t_access_text;

select * from t_access_seq;
```

（3）建一个存储parquet文件的表

```sql
create table t_access_parq(ip string,url string,access_time string)
stored as parquet;

-- 从文本表中查询数据插入sequencefile表中，生成数据文件就是sequencefile格式的了
insert into t_access_parq
select * from t_access_text;

select * from t_access_parq;
```

（4）建一个存储orc文件的表

```sql
create table t_access_orc(ip string,url string,access_time string)
stored as orc;

-- 从文本表中查询数据插入sequencefile表中，生成数据文件就是sequencefile格式的了
insert into t_access_orc
select * from t_access_text;

select * from t_access_orc;
```
### 6.2 压缩

在Hive表中和计算过程中，保持数据的压缩，对磁盘空间的有效利用和提高查询性能都是十分有益的。

在Hive中，不同文件类型的表，声明数据压缩的方式是不同的。

（1）TextFile

若一张表的文件类型为TextFile，若需要对该表中的数据进行压缩，多数情况下，无需在建表语句做出声明。直接将压缩后的文件导入到该表即可，Hive在查询表中数据时，可自动识别其压缩格式，进行解压。

需要注意的是，在执行往表中导入数据的SQL语句时，用户需设置以下参数，来保证写入表中的数据是被压缩的。

```sql
--SQL语句的最终输出结果是否压缩
set hive.exec.compress.output=true;
--输出结果的压缩格式（以下示例为snappy）
set mapreduce.output.fileoutputformat.compress.codec =org.apache.hadoop.io.compress.SnappyCodec;
```

（2）Parquet

若一张表的文件类型为Parquet，若需要对该表数据进行压缩，需在建表语句中声明压缩格式如下：

```sql
create table orc_table
(column_specs)
stored as parquet
tblproperties ("parquet.compression"="snappy");
```

（3）orc

若一张表的文件类型为ORC，若需要对该表数据进行压缩，需在建表语句中声明压缩格式如下：

```sql
create table orc_table
(column_specs)
stored as orc
tblproperties ("orc.compress"="snappy");
```

（4）单个MR的中间结果进行压缩

单个MR的中间结果是指Mapper输出的数据，对其进行压缩可降低shuffle阶段的网络IO，可通过以下参数进行配置：

```sql
--开启MapReduce中间数据压缩功能
set mapreduce.map.output.compress=true;

--设置MapReduce中间数据数据的压缩方式（以下示例为snappy）
set mapreduce.map.output.compress.codec=org.apache.hadoop.io.compress.SnappyCodec;
```

（5）单条SQL语句的中间结果进行压缩

单条SQL语句的中间结果是指，两个MR（一条SQL语句可能需要通过MR进行计算）之间的临时数据，可通过以下参数进行配置：

```sql
--是否对两个MR之间的临时数据进行压缩
set hive.exec.compress.intermediate=true;
--压缩格式（以下示例为snappy）
set hive.intermediate.compression.codec=org.apache.hadoop.io.compress.SnappyCodec;
```

## 七、explain查看执行计划（重点）

### 7.1 查看sql执行计划

```sql
explain [formatted | extended | dependency] query-sql
```

关键词为可选项：
- formatted：将执行计划以JSON字符串的形式输出
- extended： 输出执行计划中的额外信息，通常是读写的文件名等信息
- dependency：输出执行计划读取的表及分区

```sql
explain
create table order_detail_2
as
select od.user_id,od.product_id,od.province_id,product.product_name,province.province_name
from order_detail od
join product_info product on od.product_id = product.id
join province_info province on od.province_id = province.id;
```

可以用Hive执行计划可视化工具来查看。见[[windows上安装hive执行可视化工具]]

```txt
+----------------------------------------------------+
|                      Explain                       |
+----------------------------------------------------+
| STAGE DEPENDENCIES:                                |
|   Stage-1 is a root stage                          |
|   Stage-7 depends on stages: Stage-1               |
|   Stage-6 depends on stages: Stage-7               |
|   Stage-0 depends on stages: Stage-6               |
|   Stage-8 depends on stages: Stage-0               |
|   Stage-3 depends on stages: Stage-8               |
|                                                    |
| STAGE PLANS:                                       |
|   Stage: Stage-1                                   |
|     Map Reduce                                     |
|       Map Operator Tree:                           |
|           TableScan                                |
|             alias: od                              |
|             Statistics: Num rows: 13066777 Data size: 11760099340 Basic stats: COMPLETE Column stats: NONE |
|             Filter Operator                        |
|               predicate: (product_id is not null and province_id is not null) (type: boolean) |
|               Statistics: Num rows: 13066777 Data size: 11760099340 Basic stats: COMPLETE Column stats: NONE |
|               Select Operator                      |
|                 expressions: user_id (type: string), product_id (type: string), province_id (type: string) |
|                 outputColumnNames: _col0, _col1, _col2 |
|                 Statistics: Num rows: 13066777 Data size: 11760099340 Basic stats: COMPLETE Column stats: NONE |
|                 Reduce Output Operator             |
|                   key expressions: _col1 (type: string) |
|                   sort order: +                    |
|                   Map-reduce partition columns: _col1 (type: string) |
|                   Statistics: Num rows: 13066777 Data size: 11760099340 Basic stats: COMPLETE Column stats: NONE |
|                   value expressions: _col0 (type: string), _col2 (type: string) |
|           TableScan                                |
|             alias: product                         |
|             Statistics: Num rows: 1 Data size: 252857088 Basic stats: COMPLETE Column stats: NONE |
|             Filter Operator                        |
|               predicate: id is not null (type: boolean) |
|               Statistics: Num rows: 1 Data size: 252857088 Basic stats: COMPLETE Column stats: NONE |
|               Select Operator                      |
|                 expressions: id (type: string), product_name (type: string) |
|                 outputColumnNames: _col0, _col1    |
|                 Statistics: Num rows: 1 Data size: 252857088 Basic stats: COMPLETE Column stats: NONE |
|                 Reduce Output Operator             |
|                   key expressions: _col0 (type: string) |
|                   sort order: +                    |
|                   Map-reduce partition columns: _col0 (type: string) |
|                   Statistics: Num rows: 1 Data size: 252857088 Basic stats: COMPLETE Column stats: NONE |
|                   value expressions: _col1 (type: string) |
|       Reduce Operator Tree:                        |
|         Join Operator                              |
|           condition map:                           |
|                Inner Join 0 to 1                   |
|           keys:                                    |
|             0 _col1 (type: string)                 |
|             1 _col0 (type: string)                 |
|           outputColumnNames: _col0, _col1, _col2, _col4 |
|           Statistics: Num rows: 14373455 Data size: 12936109554 Basic stats: COMPLETE Column stats: NONE |
|           File Output Operator                     |
|             compressed: false                      |
|             table:                                 |
|                 input format: org.apache.hadoop.mapred.SequenceFileInputFormat |
|                 output format: org.apache.hadoop.hive.ql.io.HiveSequenceFileOutputFormat |
|                 serde: org.apache.hadoop.hive.serde2.lazybinary.LazyBinarySerDe |
|                                                    |
|   Stage: Stage-7                                   |
|     Map Reduce Local Work                          |
|       Alias -> Map Local Tables:                   |
|         $hdt$_2:province                           |
|           Fetch Operator                           |
|             limit: -1                              |
|       Alias -> Map Local Operator Tree:            |
|         $hdt$_2:province                           |
|           TableScan                                |
|             alias: province                        |
|             Statistics: Num rows: 1 Data size: 3690 Basic stats: COMPLETE Column stats: NONE |
|             Filter Operator                        |
|               predicate: id is not null (type: boolean) |
|               Statistics: Num rows: 1 Data size: 3690 Basic stats: COMPLETE Column stats: NONE |
|               Select Operator                      |
|                 expressions: id (type: string), province_name (type: string) |
|                 outputColumnNames: _col0, _col1    |
|                 Statistics: Num rows: 1 Data size: 3690 Basic stats: COMPLETE Column stats: NONE |
|                 HashTable Sink Operator            |
|                   keys:                            |
|                     0 _col2 (type: string)         |
|                     1 _col0 (type: string)         |
|                                                    |
|   Stage: Stage-6                                   |
|     Map Reduce                                     |
|       Map Operator Tree:                           |
|           TableScan                                |
|             Map Join Operator                      |
|               condition map:                       |
|                    Inner Join 0 to 1               |
|               keys:                                |
|                 0 _col2 (type: string)             |
|                 1 _col0 (type: string)             |
|               outputColumnNames: _col0, _col1, _col2, _col4, _col6 |
|               Statistics: Num rows: 15810800 Data size: 14229720817 Basic stats: COMPLETE Column stats: NONE |
|               Select Operator                      |
|                 expressions: _col0 (type: string), _col1 (type: string), _col2 (type: string), _col4 (type: string), _col6 (type: string) |
|                 outputColumnNames: _col0, _col1, _col2, _col3, _col4 |
|                 Statistics: Num rows: 15810800 Data size: 14229720817 Basic stats: COMPLETE Column stats: NONE |
|                 File Output Operator               |
+----------------------------------------------------+
|                      Explain                       |
+----------------------------------------------------+
|                   compressed: false                |
|                   Statistics: Num rows: 15810800 Data size: 14229720817 Basic stats: COMPLETE Column stats: NONE |
|                   table:                           |
|                       input format: org.apache.hadoop.mapred.TextInputFormat |
|                       output format: org.apache.hadoop.hive.ql.io.HiveIgnoreKeyTextOutputFormat |
|                       serde: org.apache.hadoop.hive.serde2.lazy.LazySimpleSerDe |
|                       name: my_test.order_detail_2 |
|       Execution mode: vectorized                   |
|       Local Work:                                  |
|         Map Reduce Local Work                      |
|                                                    |
|   Stage: Stage-0                                   |
|     Move Operator                                  |
|       files:                                       |
|           hdfs directory: true                     |
|           destination: hdfs://hadoop101:8020/user/hive/warehouse/my_test.db/order_detail_2 |
|                                                    |
|   Stage: Stage-8                                   |
|       Create Table Operator:                       |
|         Create Table                               |
|           columns: user_id string, product_id string, province_id string, product_name string, province_name string |
|           input format: org.apache.hadoop.mapred.TextInputFormat |
|           output format: org.apache.hadoop.hive.ql.io.IgnoreKeyTextOutputFormat |
|           serde name: org.apache.hadoop.hive.serde2.lazy.LazySimpleSerDe |
|           name: my_test.order_detail_2             |
|                                                    |
|   Stage: Stage-3                                   |
|     Stats Work                                     |
|       Basic Stats Work:                            |
|                                                    |
+----------------------------------------------------+
```

Explain呈现的执行计划，由一系列Stage组成，这一系列Stage具有依赖关系，==每个Stage对应一个MapReduce Job，或者一个文件系统操作等==。

==若某个Stage对应的一个MapReduce Job，其Map端和Reduce端的计算逻辑分别由Map Operator Tree和Reduce Operator Tree进行描述==，**Operator Tree由一系列的Operator组成**，一个Operator代表在Map或Reduce阶段的一个单一的逻辑操作，例如TableScan Operator，Select Operator，Join Operator等。

一共有两个job（mapreduce程序），以上执行计划整理为如下图所示：

![[执行计划.excalidraw|800]]

### 7.2 查看sql执行结果

```sql
create table order_detail_2
as
select od.user_id,od.product_id,od.province_id,product.product_name,province.province_name
from order_detail od
join product_info product on od.product_id = product.id
join province_info province on od.province_id = province.id;
```

一共执行了两个job（mapreduce程序），分别是Stage-1和Stage-6的执行计划。

```txt
INFO  : Query ID = hadoop_20231010094705_7e16951e-28bc-4517-9e8c-e50fa9c96b96
INFO  : Total jobs = 2
INFO  : Launching Job 1 out of 2
...
INFO  : number of splits:6
INFO  : Submitting tokens for job: job_1696901935450_0001
INFO  : The url to track the job: http://hadoop102:8088/proxy/application_1696901935450_0001/
...
INFO  : Hadoop job information for Stage-1: number of mappers: 6; number of reducers: 5
INFO  : 2023-10-10 09:47:24,024 Stage-1 map = 0%,  reduce = 0%
...
INFO  : 2023-10-10 09:49:10,446 Stage-1 map = 100%,  reduce = 100%, Cumulative CPU 996.82 sec
INFO  : MapReduce Total cumulative CPU time: 16 minutes 36 seconds 820 msec
INFO  : Ended Job = job_1696901935450_0001
INFO  : Starting task [Stage-7:MAPREDLOCAL] in serial mode
INFO  : Execution completed successfully
INFO  : MapredLocal task succeeded
INFO  : Launching Job 2 out of 2
...
INFO  : number of splits:3
INFO  : Submitting tokens for job: job_1696901935450_0002
INFO  : The url to track the job: http://hadoop102:8088/proxy/application_1696901935450_0002/
..
INFO  : Hadoop job information for Stage-6: number of mappers: 3; number of reducers: 0
INFO  : 2023-10-10 09:49:29,120 Stage-6 map = 0%,  reduce = 0%
...
INFO  : 2023-10-10 09:49:59,287 Stage-6 map = 100%,  reduce = 0%, Cumulative CPU 88.79 sec
INFO  : MapReduce Total cumulative CPU time: 1 minutes 28 seconds 790 msec
INFO  : Ended Job = job_1696901935450_0002
INFO  : Starting task [Stage-0:MOVE] in serial mode
INFO  : Moving data to directory hdfs://hadoop101:8020/user/hive/warehouse/my_test.db/order_detail_2 from hdfs://hadoop101:8020/user/hive/warehouse/my_test.db/.hive-staging_hive_2023-10-10_09-47-05_497_4491738927780648895-1/-ext-10002
INFO  : Starting task [Stage-8:DDL] in serial mode
INFO  : Starting task [Stage-3:STATS] in serial mode
INFO  : MapReduce Jobs Launched: 
INFO  : Stage-Stage-1: Map: 6  Reduce: 5   Cumulative CPU: 996.82 sec   HDFS Read: 1201396670 HDFS Write: 890191081 SUCCESS
INFO  : Stage-Stage-6: Map: 3   Cumulative CPU: 88.79 sec   HDFS Read: 891053634 HDFS Write: 690018420 SUCCESS
INFO  : Total MapReduce CPU Time Spent: 18 minutes 5 seconds 610 msec
INFO  : Completed executing command(queryId=hadoop_20231010094705_7e16951e-28bc-4517-9e8c-e50fa9c96b96); Time taken: 175.389 seconds
INFO  : OK
INFO  : Concurrency mode is disabled, not creating a lock manager
No rows affected (176.013 seconds)
```

![[Pasted image 20231010102727.png]]
## 八、企业级调优

参见[[hive企业级调优]]

