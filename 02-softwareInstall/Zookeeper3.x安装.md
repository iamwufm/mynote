---
author: wufm
title: Zookeeper3.x安装
rating: 3
time: 2023-09-24 周日
tags:
  - Zookeeper安装
  - Zookeeper
代码:
---
## 一、集群的组成

![[zookeeper集群组成.excalidraw]]
## 二、下载安装包

官方网站下载：[Apache ZooKeeper](https://zookeeper.apache.org/)

点击下载，跳转页面后选择想要下载的版本下载即可。

![[Pasted image 20230924110006.png|400]]

![[Pasted image 20230924110129.png|400]]

![[Pasted image 20230924110159.png|450]]
## 三，环境准备

安装 JDK。见[[Hadoop3.x集群安装#2.2.7 安装JDK]]
## 四、安装zookeeper

### 4.1 解压安装包

把压缩包上传到hadoop101机器的/opt/software

```shell
cd /opt/software

# 解压安装包
tar -zxvf apache-zookeeper-3.8.2-bin.tar.gz -C /opt/module/
```
### 4.2 修改配置文件

在hadoop101上的操作：

```shell
cd /opt/module/
# 更改名称
mv apache-zookeeper-3.8.2-bin/ zookeeper-3.8.2
cd /opt/module/zookeeper-3.8.2/conf
cp zoo_sample.cfg zoo.cfg

# 修改 dataDir 路径。最后增加server内容
vim zoo.cfg
```

```cfg
dataDir=/opt/data/zookeeper-3.8.2

# server.A=B:C:D
# A是一个数字，表示这个是第几号服务器
# B是这个服务器的地址；在B下的myid文件要与A的内容一致
# C是这个服务器 Follower 与集群中的 Leader 服务器交换信息的端口
# D是用来执行选举时服务器相互通信的端口。
server.1=hadoop101:2888:3888
server.2=hadoop102:2888:3888
server.3=hadoop103:2888:3888
```

```shell
# 创建目录
mkdir -p /opt/data/zookeeper-3.8.2
cd /opt/data/zookeeper-3.8.2
# 添加1并保存
vim myid
```

### 4.3 配置环境变量

```shell
# 3.配置Hadoop环境变量
sudo vim /etc/profile.d/my_env.sh

# 添加如下内容
#ZOOKEEPER_HOME
export ZOOKEEPER_HOME=/opt/module/zookeeper-3.8.2
export PATH=$PATH:$ZOOKEEPER_HOME/bin

# 4.让环境变量生效
source /etc/profile
```
### 4.3 分发并修改配置文件

把hadoop101的zookeeper分发到其他机器

```shell
# 同步zookeeper的数据目录并修改myid
cd /opt/data
scp -r zookeeper-3.8.2 hadoop102:$PWD
scp -r zookeeper-3.8.2 hadoop103:$PWD

# 修改hadoop102-103上的myid，分别修改为2和3
cd /opt/data/zookeeper-3.8.2
vim myid
```

```shell
# 2.同步zookeeper
cd /opt/module
scp -r zookeeper-3.8.2 hadoop102:$PWD
scp -r zookeeper-3.8.2 hadoop103:$PWD
```

```shell
# 3.同步环境变量配置
sudo scp -r /etc/profile.d/my_env.sh hadoop102:/etc/profile.d
sudo scp -r /etc/profile.d/my_env.sh hadoop103:/etc/profile.d

# 在hadoop102和hadoop103执行以下语句， 环境变量生效
source /etc/profile
```
## 五、启动集群

```shell
# 1.启动单个QuorumPeerMain（半数以上启动才能正常工作）
zkServer.sh start

# jps查看进程

# 查看状态（三台服务，至少需要启动两台才能正常工作）
zkServer.sh status

# 2.关闭QuorumPeerMain
zkServer.sh stop
```

## 六、测试集群

参见[[Zookeeper3.x学习#3.1 客户端命令行操作（shell）]]
## 七、编写集群常用脚本

在hadoop101上：

```shell
cd /home/hadoop/bin
```

（1）集群启动、关闭脚本

```shell
# 使用方式：myzk.sh start 或 myzk.sh stop
vim myzk.sh

# 添加如下内容
```

```shell
#!/bin/bash
if [ $# -lt 1 ]
then
	echo "No Args Input..."
exit ;
fi

case $1 in
"start")
	for host in hadoop101 hadoop102 hadoop103
	do
		echo "---------- zookeeper $host 启动 ------------"
		ssh $host "zkServer.sh start"
	done
	echo "================================================="
	for host in hadoop101 hadoop102 hadoop103
	do
		echo "---------- zookeeper $host 状态 ------------"
		ssh $host "zkServer.sh status"
	done
;;
"stop")
	for host in hadoop101 hadoop102 hadoop103
	do
		echo "---------- zookeeper $host 停止 ------------"
		ssh $host "zkServer.sh stop"
	done
;;
"stop")
	for host in hadoop101 hadoop102 hadoop103
	do
		echo "---------- zookeeper $host 状态 ------------"
		ssh $host "zkServer.sh status"
	done
;;
*)
	echo "Input Args Error"
;;
esac

```

```shell
# 赋予脚本执行权限
chmod +x myzk.sh

# 将脚本复制到/bin 中，以便全局调用
# sudo cp zookeeper.sh /bin/
```