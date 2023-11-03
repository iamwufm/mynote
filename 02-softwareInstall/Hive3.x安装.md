---
author: wufm
title: Hive3.x安装
rating: 4
time: 2023-09-28 周四
tags:
  - Hive
代码:
---
## 一、环境准备

### 1.1 hadoop安装

见[[Hadoop3.x集群安装]]
## 二、hive下载

hive官网：[Apache Hive](https://hive.apache.org/)

![[Pasted image 20230928165901.png|400]]

![[Pasted image 20230928165920.png|400]]

![[Pasted image 20230928165949.png|400]]
## 三、安装hive

### 3.1 最简安装：用内嵌derby作为元数据库

准备工作：安装hive的机器上应该有HADOOP环境（安装目录，HADOOP_HOME环境变量）

安装：直接解压一个hive安装包即可

此时，安装的这个hive实例使用其内嵌的derby数据库作为记录元数据的数据库

此模式不便于让团队成员之间共享协作（再打开一个客户端窗口启动hive，会产生java.sql.SQLException异常。）

原因在于Hive默认使用的元数据库为**derby**。derby数据库的特点是同一时间只允许一个客户端访问。如果多个Hive客户端同时访问，就会报错。==由于在企业开发中，都是多人协作开发，需要多客户端同时访问Hive，怎么解决呢？我们可以将Hive的元数据改为用MySQL存储，MySQL支持多客户端同时访问。==
### 3.2 标准安装：将mysql作为元数据库

#### 3.2.1 mysql安装

在hadoop103上安装mysql。

见[[在linux上安装mysql]]
#### 3.2.2 hive的元数据库配置

在hadoop102上安装hive

解压hive包：

```shell
# 上传hive安装包
cd /opt/software/
rz

# 解压并重命名
tar -zxf apache-hive-3.1.3-bin.tar.gz -C /opt/module/

cd /opt/module

mv apache-hive-3.1.3-bin hive-3.1.3
```

在hadoop102上执行

（1）新增hive-site.xml文件，增加mysql信息

```shell
cd /opt/module/hive-3.1.3/conf

# 添加如下参数
vim hive-site.xml
```

```xml
<configuration>

<property>
	<name>javax.jdo.option.ConnectionURL</name>
	<value>jdbc:mysql://hadoop103:3306/hive?createDatabaseIfNotExist=true</value>
	<description>JDBC connect string for a JDBC metastore</description>
</property>

<property>
	<name>javax.jdo.option.ConnectionDriverName</name>
	<value>com.mysql.jdbc.Driver</value>
	<description>Driver class name for a JDBC metastore</description>
</property>

<property>
	<name>javax.jdo.option.ConnectionUserName</name>
	<value>root</value>
	<description>username to use against metastore database</description>
</property>

<property>
	<name>javax.jdo.option.ConnectionPassword</name>
	<value>1234</value>
	<description>password to use against metastore database</description>
</property>

</configuration>
```

（2）上传一个mysql的驱动jar包到hive的安装目录的lib中

```xml
cd /opt/module/hive-3.1.3/lib

rz
```

驱动包可以从官网下载：[MySQL :: Download MySQL Connector/J (Archived Versions)](https://downloads.mysql.com/archives/c-j/)

也可以通过pom文件下载

```xml
<dependency>  
    <groupId>mysql</groupId>  
    <artifactId>mysql-connector-java</artifactId>  
    <version>5.1.39</version>  
</dependency>
```

（3）配置HIVE_HOME到系统环境变量中：/etc/profile

```shell
sudo vim /etc/profile.d/my_env.sh

# 添加如下内容
#HIVE_HOME
export HIVE_HOME=/opt/module/hive-3.1.3
export PATH=$PATH:$HIVE_HOME/bin

# 让环境变量生效
source /etc/profile
```

（3）初始化数据库（执行一次就好，安装多台hive，只需要在一台执行就好）

```shell
schematool -dbType mysql -initSchema
```
（4）hive启动测试

==一定先要启动hdfs集群和yarn集群，mysql服务也要启动==

```shell
hive
```
## 四、hive使用方式


```shell
hive -help
```

```txt
usage: hive
 -d,--define <key=value>          Variable substitution to apply to Hive
                                  commands. e.g. -d A=B or --define A=B
    --database <databasename>     Specify the database to use
 -e <quoted-query-string>         SQL from command line
 -f <filename>                    SQL from files
 -H,--help                        Print help information
    --hiveconf <property=value>   Use value for given property
    --hivevar <key=value>         Variable substitution to apply to Hive
                                  commands. e.g. --hivevar A=B
 -i <filename>                    Initialization SQL file
 -S,--silent                      Silent mode in interactive shell
 -v,--verbose                     Verbose mode (echo executed SQL to the
                                  console)
```
### 4.1 交互式命令

```shell
hive
show databases;
```

hive常用命令入门可参考[[#五、快速入门]]
### 4.2 启动hive服务使用

Hive的hiveserver2服务的作用是提供jdbc/odbc接口，为用户提供远程访问Hive数据的功能，==例如用户期望在个人电脑中访问远程服务中的Hive数据，就需要用到Hiveserver2==。

![[hiveserver2服务.excalidraw]]

（1）可以把hive包copy给其中一台服务器

```shell
# hadoop104有HADOOP_HOME环境
cd /opt/module
scp -r hive-3.1.3 hadoop104:$PWD

# 添加hadoop104的HIVE_HOME环境变量
# 在hadoop104上执行
sudo vim /etc/profile.d/my_env.sh

# 添加如下内容
#HIVE_HOME
export HIVE_HOME=/opt/module/hive-3.1.3
export PATH=$PATH:$HIVE_HOME/bin

# 让环境变量生效
source /etc/profile
```

#### 4.2.1 方式一：只启动hiveserver2服务

（2）启动服务器

```shell
# hadoop102上执行
# 方式一：前台启动服务器。ctrl+c或者关闭关闭终端进程 程序退出。也可以使用hive --service hiveserver2
hiveserver2

# 方式二：后台运行。最后一个&是后台运行的意思。1是标准输出，2是标准错误。/dev/null表示不输出。nohup控制台不显示。下面的句子的意思是不记录任何日志。只记录异常日志可写() nohup hiveserver2 1>/dev/null 2>error.log  &
nohup hiveserver2 1>/dev/null 2>&1 &

# 查看进程。kill- 9 进程号
ps -ef | grep hiveserver2
```

（3）启动客户端

方式一：==启动beeline客户端==

```shell
# 在hadoop104上执行

#  或者 直接输入 beeline -u jdbc:hive2://hadoop102:10000 -n hadoop
beeline
!connect jdbc:hive2://hadoop102:10000
hadoop
密码直接回车
```

出现错误

```txt
Error: Could not open client transport with JDBC Uri: jdbc:hive2://hadoop102:10000: Failed to open new session: java.lang.RuntimeException: org.apache.hadoop.ipc.RemoteException(org.apache.hadoop.security.authorize.AuthorizationException): User: hadoop is not allowed to impersonate hadoop (state=08S01,code=0)
```

修改hadoop的core-site.xml，增加如下内容

```shell
cd /opt/module/hadoop-3.3.6/etc/hadoop/

vim core-site.xml
```

```xml
<!--配置所有节点的hadoop用户都可作为代理用户-->
<property>
    <name>hadoop.proxyuser.hadoop.hosts</name>
    <value>*</value>
</property>

<!--配置hadoop用户能够代理的用户组为任意组-->
<property>
    <name>hadoop.proxyuser.hadoop.groups</name>
    <value>*</value>
</property>

<!--配置hadoop用户能够代理的用户为任意用户-->
<property>
    <name>hadoop.proxyuser.hadoop.users</name>
    <value>*</value>
</property>
```

```shell
# 分发
xsync core-site.xml
# 重启hdfs集群
stop-dfs.sh
start-dfs.sh

# 重新启动客户端
ctrl + c

beeline -u jdbc:hive2://hadoop102:10000 -n hadoop

show databases;
```

方式二：==也可以个人电脑远程登陆：[[datagrip连接hive]]==

#### 4.2.1 方式二：还启动metastore服务

==Hive的metastore服务的作用是为Hive CLI或者Hiveserver2提供元数据访问接口==。有两种运行模式，分别为嵌入式模式和独立服务模式

生产环境中，不推荐使用嵌入式模式。因为其存在以下两个问题：
- 嵌入式模式下，每个Hive CLI都需要直接连接元数据库，当Hive CLI较多时，数据库压力会比较大。
- 每个客户端都需要用户元数据库的读写权限，元数据库的安全得不到很好的保证。

![[metastore服务.excalidraw|600]]

**配置独立服务模式：**

==注意：主机名需要改为metastore服务所在节点，端口号无需修改，metastore服务的默认端口就是9083。==

```shell
# metastore服务
hive --service metastore
```

自动化启动脚本见[[#九、常用脚本]]
### 4.3 脚本化运行

大量的hive查询任务，如果用交互式shell来进行输入的话，显然效率及其低下，因此，生产中更多的是使用脚本化运行机制：

该机制的核心点是：==hive可以用一次性命令的方式来执行给定的hql语句==

```shell
hive -e "drop table if exists default.t_bash"
hive -e "set hive.exec.mode.local.auto=true;create table default.t_bash as select * from default.t_stu"
```

然后，进一步，可以将上述命令写入shell脚本中，以便于脚本化运行hive任务，并控制、调度众多hive任务，示例如下：

```shell
# 创建shell文件（chmod +x t_test.sh） (./t_test.sh)
vim t_test.sh

# 添加如下内容
#!/bin/bash
hive -e "drop table if exists default.t_test"
hive -e "select * from default.t_stu"

hive -e "select * from default.t_bash"

hql="create table default.t_test as select * from default.t_stu"

hive -e "$hql"
```

如果要执行的hql语句特别复杂，那么，可以把hql语句写入一个文件：

```shell
# 用 hive -f  test.sql 执行
vim test.sql

# 添加如下内容
set hive.exec.mode.local.auto=true;
select * from default.t_stu;
select count(1) from default.t_stu;
```

执行文件中的sql语句并将结果写入文件中

```shell
hive -f test.sql > hive_result.txt
```
## 五、快速入门

（1）开启hdfs集群、yarn集群和mysql服务

```shell
# 在hadoop101上执行
start-dfs.sh 

# 在hadoop102上执行
start-yarn.sh

# 在hadoop103上执行（查看状态：sudo service mysql status）
sudo service mysql start

# 关闭 start 改为 stop
```

（2）在hive中创建一张表

```shell
# 在hadoop102上执行
hive

# 查看hive底层引擎
set hive.execution.engine;

# 结果
hive.execution.engine=mr

# 建表.默认分隔符是 ^A (\001)
create table t_stu(id int,name string,age int,sex string);
```

```sql
-- 在MySQL可查询到表的元数据
select t3.`NAME` -- 数据库名
	,t3.DB_LOCATION_URI -- hdfs路径
	,t2.TBL_NAME -- 表名
	,t1.COLUMN_NAME -- 字段名
	,t1.TYPE_NAME -- 字段类型
	,t1.INTEGER_IDX -- 字段顺序
from hive.COLUMNS_V2 t1 -- 表字段
left join hive.TBLS t2 -- 表
	on t1.CD_ID = t2.TBL_ID
left join hive.DBS t3 -- 数据库
	on t2.DB_ID = t3.DB_ID
order by t1.INTEGER_IDX
```

在namenode可查看到目录：

![[Pasted image 20230928180046.png]]

（3）造点数据放在表中

```shell
在windows上用notePad++创建test1.txt文件。输入以下内容，然后替换
# 添加如下内容
```

```txt
1:zhangsan:18:男
2:lisi:29:男
3:wangwu:18:女
4:laoliu:30:男
5:laoqi:20:女
6:laoba:25:男
7:laojiu:17:女
```

![[Pasted image 20230928182810.png|400]]


```shell
# 上传文件到虚拟机
cd ~
mkdir hivetest
cd hivetest
rz

hdfs dfs -put test1.txt /user/hive/warehouse/t_stu
```

（4）查询数据

在（2）中执行

```shell
select * from t_stu;
```

![[Pasted image 20230928183143.png|400]]

```shell
# 让提示符显示当前库
set hive.cli.print.current.db=true;

# 显示查询结果时显示字段名称
set hive.cli.print.header=true;
```

![[Pasted image 20230928183313.png|400]]

但是这样设置只对当前会话有效，重启hive会话后就失效，解决办法：在linux的当前用户目录中，编辑一个.hiverc文件，将参数写入其中：

```shell
# 在hadoop102上
cd ~

vim .hiverc

# 添加如下参数
set hive.cli.print.header=true;
set hive.cli.print.current.db=true;
```

```shell
select count(id) as total from t_stu;

# 为了方便测试，可以用本地模式。yarn集群可以关掉
set hive.exec.mode.local.auto=true;
```

在hadoop102中执行sql语句。hadoop集群中mapreduce程序配置yarn。map task 、reduce task 和AM并非是都是hadoop102。说明hive语句只生成mr程序，然后丢到hadoop集群运行。

![[Pasted image 20230928185747.png|400]]

![[Pasted image 20230928185812.png|400]]

![[Pasted image 20230928190237.png|375]]

![[Pasted image 20230928190328.png|400]]

![[Pasted image 20230928185947.png]]

![[Pasted image 20230928190011.png]]


![[Pasted image 20230928190040.png]]


## 六、其他命令操作

```shell
# 查看hdfs文件系统
dfs -ls /;

# 查看本地文件系统
! ls /opt/module/datas;

# 查看在hive中输入的所有历史命令
# 进入root目录或者用户的根目录 # cd /root
cd ~
cat .hivehistory
```

## 七、Hive参数配置方式

查看当前所有的配置信息

```shell
set;
```

上述三种设定方式的优先级依次递增。即**配置文件 < 命令行参数< 参数声明**。注意某些系统级的参数，例如log4j相关的设定，必须用前两种方式设定，因为那些参数的读取在会话建立以前已经完成了。
### 7.1 配置文件方式

默认配置文件：hive-default.xml

用户自定义配置文件：hive-site.xml

注意：==用户自定义配置会覆盖默认配置==。另外，Hive也会读入Hadoop的配置，因为Hive是作为Hadoop的客户端启动的，Hive的配置会覆盖Hadoop的配置。==配置文件的设定对本机启动的所有Hive进程都有效==。

### 7.2 命令行参数方式

启动Hive时，可以在命令行添加-hiveconf param=value来设定参数。例如：

```shell
hive -hiveconf mapreduce.job.reduces=10;

# 查看参数设置
set mapreduce.job.reduces;
```

==注意：仅对本次Hive启动有效。==

### 7.3 参数声明方式

可以在HQL中使用SET关键字设定参数，例如：

```shell
set mapreduce.job.reduces=10;

# 查看参数设置
set mapreduce.job.reduces;
```

==注意：仅对本次Hive启动有效。==
## 八、Hive常见属性配置

### 8.1 Hive客户端显示当前库和表头

方式一：用户根目录修改

```shell
# 在hadoop102上
cd ~

vim .hiverc

# 添加如下参数
set hive.cli.print.header=true;
set hive.cli.print.current.db=true;
```

方式二：配置文件修改

```shell
cd /opt/module/hive-3.1.3/conf

# 添加如下内容
vim hive-site.xml
```

```xml
<!-- 显示表头-->
<property>
    <name>hive.cli.print.header</name>
    <value>true</value>
</property>

<!-- 显示当前库-->
<property>
    <name>hive.cli.print.current.db</name>
    <value>true</value>
</property>
```

### 8.2 Hive运行日志路径配置

Hive的log默认存放在/tmp/hadoop/hive.log目录下（当前用户名下）

```shell
cd /opt/module/hive-3.1.3/conf

cp hive-log4j2.properties.template hive-log4j2.properties

vim hive-log4j2.properties

# 修改如下参数
property.hive.log.dir=/opt/module/data/hive/logs
```
### 8.3 Hive的JVM堆内存设置

新版本的Hive启动的时候，默认申请的JVM堆内存大小为256M，JVM堆内存申请的太小，导致后期开启本地模式，执行复杂的SQL时经常会报错：java.lang.OutOfMemoryError: Java heap space，因此最好提前调整一下HADOOP_HEAPSIZE这个参数。

```shell
cd /opt/module/hive-3.1.3/conf

cp hive-env.sh.template hive-env.sh

vim hive-env.sh

# 修改如下参数
export HADOOP_HEAPSIZE=2048
```

## 九、常用脚本

使用于[[#4.2 启动hive服务使用]]

```shell
cd ~/bin

# 使用方式 hiveservices.sh start|stop|restart|status
vim hiveservices.sh

# 添加如下内容
```

```shell
#!/bin/bash

HIVE_LOG_DIR=$HIVE_HOME/logs

if [ ! -d $HIVE_LOG_DIR ]
then
	mkdir -p $HIVE_LOG_DIR
fi

#检查进程是否运行正常，参数1为进程名，参数2为进程端口
function check_process()
{
	pid=$(ps -ef 2>/home/hadoop/null | grep -v grep | grep -i $1 | awk '{print $2}')
	echo $pid
	ppid=$(netstat -nltp 2>/home/hadoop/null | grep $2 | awk '{print $7}' | cut -d '/' -f 1)
	echo $ppid
	[[ "$pid" =~ "$ppid" ]] && [ "$ppid" ] && return 0 || return 1
}

function hive_start()
{
	metapid=$(check_process HiveMetastore 9083)
	cmd="nohup hive --service metastore >$HIVE_LOG_DIR/metastore.log 2>&1 &"
	[ -z "$metapid" ] && eval $cmd || echo "Metastroe服务已启动"
	server2pid=$(check_process HiveServer2 10000)
	cmd="nohup hive --service hiveserver2 >$HIVE_LOG_DIR/hiveServer2.log 2>&1 &"
	[ -z "$server2pid" ] && eval $cmd || echo "HiveServer2服务已启动"
}

function hive_stop()
{
	metapid=$(check_process HiveMetastore 9083)
	[ "$metapid" ] && kill $metapid || echo "Metastore服务未启动"
	server2pid=$(check_process HiveServer2 10000)
	[ "$server2pid" ] && kill $server2pid || echo "HiveServer2服务未启动"
}

case $1 in
"start")
	hive_start
	;;
"stop")
	hive_stop
	;;
"restart")
	hive_stop
	sleep 2
	hive_start
	;;
"status")
	check_process HiveMetastore 9083 >/home/hadoop/null && echo "Metastore服务运行正常" || echo "Metastore服务运行异常"
	check_process HiveServer2 10000 >/home/hadoop/null && echo "HiveServer2服务运行正常" || echo "HiveServer2服务运行异常"
	;;

*)
	echo Invalid Args!
	echo 'Usage: '$(basename $0)' start|stop|restart|status'
	;;
esac
```

```shell
# 添加执行权限
chmod +x hiveservices.sh

# 启动Hive后台服务
hiveservices.sh start
```