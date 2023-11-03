---
author: wufm
title: Phoenix2.x安装
rating: 2
time: 2023-10-20 周五
tags:
  - Phoenix
  - Hbase
代码:
---

## 一、环境准备

### 1.1 安装Hbase集群

见[[Hbase2.x集群安装]]
## 二、安装Phoenix

### 2.1 下载Phoenix安装包

官网下载：[Overview | Apache Phoenix](https://phoenix.apache.org/)
### 2.2 安装Phoenix

在hadoop103安装：

```shell
# 1.把安装包放在/opt/software/目录下
cd /opt/software/
rz

# 2.解压hbase到/opt/module 目录下
tar -zxf phoenix-hbase-2.5-5.1.3-bin.tar.gz -C /opt/module/

# 重命名
cd /opt/module/
mv phoenix-hbase-2.5-5.1.3-bin/ phoenix-hbase-2.5-5.1.3

# 3.配置Hadoop环境变量
sudo vim /etc/profile.d/my_env.sh

# 添加如下内容
#phoenix
export PHOENIX_HOME=/opt/module/phoenix-hbase-2.5-5.1.3
export PHOENIX_CLASSPATH=$PHOENIX_HOME
export PATH=$PATH:$PHOENIX_HOME/bin

# 4.让环境变量生效
source /etc/profile

# 复制 server 包并拷贝到各个节点的 hbase/lib
cd /opt/module/phoenix-hbase-2.5-5.1.3
cp phoenix-server-hbase-2.5-5.1.3.jar  /opt/module/hbase-2.5.5/lib/
xsync  /opt/module/hbase-2.5.5/lib/phoenix-server-hbase-2.5-5.1.3.jar
```
### 2.3 修改配置文件

```shell
cd /opt/module/hbase-2.5.5/conf

vim hbase-site.xml
# 添加如下内容
```

```xml
<!-- 开启建立命名空间-->
<property> 
	<name>phoenix.schema.isNamespaceMappingEnabled</name> 
	<value>true</value> 
</property> 

<property> 
	<name>phoenix.schema.mapSystemTablesToNamespace</name> 
	<value>true</value> 
</property>

<!-- 开启索引-->
<property> 
	<name>hbase.regionserver.wal.codec</name> 
	<value>org.apache.hadoop.hbase.regionserver.wal.IndexedWALEditCodec</value> 
</property>
```

分发脚本：

```shell
xsync hbase-site.xml
```

## 三、启动服务

（1）重启 HBase

```shell
# 在hadoop101上执行
stop-hbase.sh 

start-hbase.sh 
```

（2）启动Phoenix客户端

```shell
sqlline.py hadoop101,hadoop102,hadoop103:2181
```

（3）测试下

显示所有表

```sql
!table 或 !tables
```

![[Pasted image 20231020223923.png]]

退出客户端

```sql
!quit 或 !exit
```