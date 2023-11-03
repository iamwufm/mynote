---
author: wufm
title: Spark3.x集群安装
rating: 6
time: 2023-10-27 周五
tags:
  - Spark
代码:
---
## 一、集群组成

安装在三台机器上

| hadoop101 | hadoop102 | hadoop103 |
| --------- | --------- | --------- |
| Worker    | Worker    | Worker    |
| Master          |           |           |

## 二、环境的准备

### 2.1 hadoop集群安装

见[[Hadoop3.x集群安装]]

## 三、安装Spark集群

### 3.1 下载Spark安装包

官网下载：[Apache Spark™ - Unified Engine for large-scale data analytics](https://spark.apache.org/)
### 3.2 安装Spark

在hadoop101机器上进行

```shell
# 1.把安装包放在/opt/software/目录下
cd /opt/software/
rz

# 2.解压spark到/opt/module 目录下
tar -zxf spark-3.5.0-bin-hadoop3.gz -C /opt/module/

# 重命名
cd /opt/module/
mv spark-3.5.0-bin-hadoop3/ spark-3.5.0

# 3.配置spark环境变量
sudo vim /etc/profile.d/my_env.sh

# 添加如下内容
#SPARK_HOME
export SPARK_HOME=/opt/module/spark-3.5.0
export PATH=$PATH:$SPARK_HOME/bin
export PATH=$PATH:$SPARK_HOME/sbin

# 4.让环境变量生效
source /etc/profile
```

### 3.3 修改Spark配置文件

```shell
cd /opt/module/spark-3.5.0/conf
cp spark-env.sh.template spark-env.sh
cp workers.template workers

vim spark-env.sh

# 新增如下参数，指定master地址
SPARK_MASTER_HOST=hadoop101
SPARK_WORKER_CORES=4

vim workers

# 新增如下内容，指定worker地址
hadoop101
hadoop102
hadoop103
```
### 3.4 分发脚本


```shell
cd /opt/module
# 在hadoop101上把内容复制给hadoop102和hadoop103

# 1.同步spark
# 也可以写成scp -r /opt/module/hadoop-3.3.6  hadoop03:/opt/module
scp -r spark-3.5.0 hadoop102:$PWD
scp -r spark-3.5.0 hadoop103:$PWD
```

去hadoop102和hadoop103机器上添加hbase的环境变量

```shell
sudo vim /etc/profile.d/my_env.sh

# 添加如下内容
#SPARK_HOME
export SPARK_HOME=/opt/module/spark-3.5.0
export PATH=$PATH:$SPARK_HOME/bin
export PATH=$PATH:$SPARK_HOME/sbin

# 4.让环境变量生效
source /etc/profile
```
## 四、启动Spark集群

```shell
# 启动master
start-master.sh

# 启动worker
start-workers.sh 
```

网页访问：hadoop101:8080

![[Pasted image 20231027181119.png|675]]
## 五、测试Spark集群

