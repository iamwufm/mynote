---
author: wufm
title: Hadoop3.x安装
rating: 3
time: 2023-08-24 周四
tags:
  - hadoop安装
  - 完全分布模式
---

## 一、集群的组成

ip地址前九位数字必须跟网关保持一致，因为网关是大佬，对外访问会先通过大佬。后面数字范围是0-255，同时要避免跟网关、子网ip和其他ip产生冲突。参照[[VM与CentOS安装#3.2.1 可用ip范围]]

本VMnet8的网关是192.168.204.2。

NameNode、SecondaryNameNode和ResourceManager不要安装在同一台机器上，都是容易消耗内存的。
==DataNode和NodeManager应该部署在一起。==

![[Drawing 2023-09-21 16.27.20.excalidraw|450]]
## 二、环境准备

### 2.1 安装虚拟机

硬件要求：内存4G、硬盘50G

主机名：hadoop101

ip地址：192.168.204.101

root密码：abcd1234..

详细安装参考见 [[VM与CentOS安装]]，暂时不看[[VM与CentOS安装#四、克隆机器]]

### 2.2 虚拟机的配置要求

#### 2.2.1 能ping通外网

使用yum要求虚拟机可以正常上网，所以第一步测试虚拟机时候可以ping通外网

```shell
ping baidu.com
```
#### 2.2.2 安装工具包

```shell
# 如果是最小安装，还需要安装工具包集合，包含ifconfig等命令
yum install -y net-tools

# 安装epel-release
# epel-release是为“红帽系”的操作系统提供额外的软件包，适用于RHEL、CentOS和Scientific Linux。相当于是一个软件仓库，大多数rpm在官方repository中找不到
yum install -y epel-release

# 安装vim
yum install -y vim

# 安装rz、sz，方便上传下载
yum install -y lrzsz

# 安装远程同步工具
yum install -y rsync 

# 主要用于同步服务器时间
yum install  -y ntp
```
#### 2.2.3 关闭防火墙

在企业开发时，通常单个服务器的防火墙是关闭的。公司整体对外会设置非常安全的防火墙。

```shell
# 关闭防火墙（查看状态：systemctl status firewalld）
systemctl stop firewalld

# 关闭防火墙自启
systemctl disable firewalld.service
```
#### 2.2.4 创建hadoop用户和配置权限

```shell
# 创建hadoop用户，并修改hadoop用户的密码(密码为1234)
useradd hadoop
passwd hadoop

# 配置hadoop用户具有root权限，方便后期加sudo执行root权限的命令
vim /etc/sudoers

# 在%wheel这行添加如下内容。强制保存退出（:wq!）
# hadoop这一行不要直接放到root行下面，因为所有用户都属于 wheel 组，你先配置了hadoop具有免密功能，但是程序执行到%wheel行时，该功能又被覆盖回需要密码。所以hadoop要放到%wheel 这行下面。
hadoop ALL=(ALL) NOPASSWD:ALL
```
#### 2.2.5 创建文件夹

 在/opt目录下创建文件夹，并修改所属主和所属组

```shell
# 创建文件夹
# 软件包安装路径
mkdir /opt/module
# 软件包存放路径
mkdir /opt/software
# 数据存放路径
mkdir /opt/data

# 修改 module、software 文件夹的所有者和所属组均为 hadoop 用户
chown hadoop:hadoop /opt/module
chown hadoop:hadoop /opt/software
chown hadoop:hadoop /opt/data/
```

#### 2.2.6 配置域名映射

在window和linux系统上配置域名映射，不需要每次输入ip

```shell
window路径文件：c:/windows/system32/drivers/etc/hosts
linux路径：vim /etc/hosts
# 添加如下内容并保存：
192.168.204.101	hadoop101
192.168.204.102	hadoop102
192.168.204.103	hadoop103
```
#### 2.2.7 安装JDK

```shell
# 1. 卸载虚拟机自带的JDK,如果虚拟机是最小化安装不需要执行这一步
rpm -qa | grep -i java | xargs -n1 rpm -e --nodeps

# rpm -qa：查询所安装的所有 rpm 软件包
# grep -i：忽略大小写
# xargs -n1：表示每次只传递一个参数
# rpm -e –nodeps：强制卸载软件

# 2. 切换hadoop用户，包括用户环境也一起切换
su - hadoop

# 3.把安装包放在/opt/software/目录下
cd /opt/software/
rz

# 4.解压JDK到/opt/module 目录下
tar -zxvf jdk-8u144-linux-x64.tar.gz -C /opt/module/

# 5.配置JDK环境变量
sudo vim /etc/profile.d/my_env.sh

# 添加如下内容
#JAVA_HOME
export JAVA_HOME=/opt/module/jdk1.8.0_144
export PATH=$PATH:$JAVA_HOME/bin

# 6.让环境变量生效
source /etc/profile

# 7.验证java是否安装成功
java -version
```
## 三、安装hadoop集群

==没特殊说明，以下操作均在hadoop101机器上的hadoop用户下执行。==
### 3.1 安装其他两台虚拟机

采用完整克隆方法安装，具体操作参考：[[VM与CentOS安装#四、克隆机器]]

主机名和ip地址参考[[Hadoop3.x集群安装#一、集群的组成]]
### 3.2 安装Hadoop和修改配置文件

Hadoop包下载地址：[Apache Hadoop官网](https://hadoop.apache.org/releases.html)

阿里云镜像下载：[阿里云镜像](https://mirrors.aliyun.com/apache/hadoop/core/)
#### 3.2.1 安装Hadoop

```shell
# 1.把安装包放在/opt/software/目录下
cd /opt/software/
rz

# 2.解压hadoop到/opt/module 目录下
tar -zxvf hadoop-3.3.6.tar.gz -C /opt/module/

# 3.配置Hadoop环境变量
sudo vim /etc/profile.d/my_env.sh

# 添加如下内容
#HADOOP_HOME
export HADOOP_HOME=/opt/module/hadoop-3.3.6
export PATH=$PATH:$HADOOP_HOME/bin
export PATH=$PATH:$HADOOP_HOME/sbin

# 4.让环境变量生效
source /etc/profile

# 5.验证java是否安装成功（如果不显示版本号，可重启虚拟机sudo reboot）
hadoop version
```

```text
Hadoop重要目录：
bin：存放对Hadoop相关服务（hdfs、yarn、mapred）进行操作的脚本
sbin：存放启动或停止Hadoop相关服务的脚本
etc：Hadoop的配置文件目录，存放Hadoop的配置文件
lib：存放Hadoop的本地库（对数据进行压缩解压功能）
share：存放Hadoop的依赖jar包、文档和官方案例
```
#### 3.2.2 修改配置文件

Hadoop配置文件分两类：默认配置文件和自定义配置文件，**只有用户想修改某一默认配置值时，才需要修改自定义配置文件，更改相应属性值**。

| 自定义文件      | 默认文件           | 重要配置说明                                                                                           |
| --------------- | ------------------ | ------------------------------------------------------------------------------------------------------ |
| core-site.xml   | core-default.xml   | 指定hadoop的默认文件系统|
| hdfs-site.xml   | hdfs-default.xml   |    HDFS配置                                                                                                    |
| yarn-site.xml   | yarn-default.xml   |    Yarn配置                                                                                                    |
| mapred-site.xml | mapred-default.xml |      MapReduce配置                                                                                                  |

```shell
# 进入自定义配置文件目录
cd /opt/module/hadoop-3.3.6/etc/hadoop
```

1. 修改core-site.xml

```shell
vim core-site.xml
```

```xml
<!--在<configuration>标签里新增如下内容-->

<!-- 1.指定hadoop的默认文件系统为：hdfs -->
<property>
<name>fs.defaultFS</name>
<value>hdfs://hadoop101:8020</value>
</property>

<!-- 指定 hadoop 数据的存储目录 -->
<property>
<name>hadoop.tmp.dir</name>
<value>/opt/data/hadoop-3.3.6</value>
</property>

<!-- 配置 HDFS 网页登录使用的静态用户为 hadoop -->
<property>
<name>hadoop.http.staticuser.user</name>
<value>hadoop</value>
</property>
```

2. 修改hdfs-site.xml

```shell
vim hdfs-site.xml
```

```xml
<!--在<configuration>标签里新增如下内容-->

<!-- NameNode web 端访问地址-->
<property>
<name>dfs.namenode.http-address</name>
<value>hadoop101:9870</value>
</property>

<!-- SecondaryNameNode web 端访问地址-->
<property>
<name>dfs.namenode.secondary.http-address</name>
<value>hadoop103:9868</value>
</property>
```

3. 修改yarn-site.xml

```shell
vim yarn-site.xml
```

```xml
<!--在<configuration>标签里新增如下内容-->

<!-- 指定 MR 走 shuffle -->
<property>
<name>yarn.nodemanager.aux-services</name>
<value>mapreduce_shuffle</value>
</property>

<!-- 指定 ResourceManager 的地址-->
<property>
<name>yarn.resourcemanager.hostname</name>
<value>hadoop102</value>
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

4. 修改mapred-site.xml

```shell
vim mapred-site.xml
```

```xml
<!--在<configuration>标签里新增如下内容-->

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

5. 修改workers文件

```shell
vim workers

# 添加如下内容（datanode的列表，使用start-dfs.sh等命令会遍历此文件）
hadoop101
hadoop102
hadoop103
```
### 3.3 SSH无密码登陆配置

免密登陆原理：

![[Drawing 2023-08-29 10.09.09.excalidraw]]

```shell
# 先后在hadoop101、hadoop102和hadoop103执行以下操作

# 在hadoop101上生成公钥和私钥
# 1. 输入后敲（三个回车），就会生成两个文件 id_rsa（私钥）、id_rsa.pub（公钥）
ssh-keygen -t rsa

# 2.将公钥拷贝到要免密登录的目标机器上
ssh-copy-id hadoop101
ssh-copy-id hadoop102
ssh-copy-id hadoop103

# 3.可以进入相应目录，查看生成的信息
cd /home/hadoop/.ssh
ll
```

| 文件 名         | 说明                                    |
| --------------- | --------------------------------------- |
| known_hosts     | 记录ssh访问过计算机的公钥（public key） |
| id_rsa          | 生成的私钥                              |
| id_rsa.pub      | 生成的公钥                              |
| authorized_keys | 存放授权过的无密登陆服务器公钥                                        |
### 3.4 集群时间同步

如果服务器在公网环境（能连接外网），可以不采用集群时间同步，因为服务器会定期和公网时间及逆行校准；**如果服务器在内网环境，必须要配置集群时间同步**，否则时间久了，会产生时间偏差，导致集群执行任务不同步。

解决方法之一：==找一台机器，作为时间服务器，所有的机器与这台机器的时间进行定时的同步。==

![[Drawing 2023-08-29 16.48.14.excalidraw]]

（1）在hadoop101上的操作

```shell
# 在hadoop101上修改ntp.conf 配置文件
sudo vim /etc/ntp.conf

添加一行
# 授权 192.168.204.0-192.168.204.255网段上的所有机器可以从这台机器上查询和同步时间
restrict 192.168.204.0 mask 255.255.255.0 nomodify notrap

# 注释掉几行
# 集群在局域网中，不使用其他互联网上的时间
server 0.centos.pool.ntp.org iburst
server 1.centos.pool.ntp.org iburst
server 2.centos.pool.ntp.org iburst
server 3.centos.pool.ntp.org iburst

# 添加两行
# 当该节点丢失网络连接，依然可以采用本地时间作为时间服务器为集群中的其他节点提供时间同步
server 127.127.1.0
fudge 127.127.1.0 stratum 10
```

```shell
# 1.修改hadoop101的/etc/sysconfig/ntpd文件
sudo vim /etc/sysconfig/ntpd

# 增加以下内容
# 让硬件时间与系统时间一起同步
SYNC_HWCLOCK=yes

# 2. 重新启动 ntpd 服务
sudo systemctl start ntpd

# 3.设置 ntpd 服务开机启动
sudo systemctl enable ntpd
```

（2）在其他机器上的操作（hadoop102和hadoop103）

```shell
# 1.关闭其他机器上 ntp 服务和自启动
sudo systemctl stop ntpd
sudo systemctl disable ntpd

# 2.在其他机器上配置1分钟与时间服务器同步一次
sudo crontab -e

# 添加如下内容，每隔一分钟同步hadoop02的时间
*/1 * * * * /usr/sbin/ntpdate hadoop101

# 测试，任意修改其他机器上的时间
sudo date -s "2021-9-11 11:11:11"

# 一分钟后查看时间
sudo date
```
### 3.5 分发脚本

可以实现服务器与服务器之间的数据拷贝命令：

**rsync 和 scp 区别：用 rsync 做文件的复制要比 scp 的速度快，rsync 只对差异文件做更新。scp 是把所有文件都复制过去。**

完全拷贝：

`scp -r $pdir/$fname $user@$host:$pdir/$fname`

参数说明：
- -r ：递归

`rsync -av $pdir/$fname $user@$host:$pdir/$fname`

参数说明：
- -a：归档拷贝
- -v：显示复制过程

```shell
# 测试scp和rsync

# 在hadoop101上执行

# 1.编写测试脚本
mkdir /home/hadoop/test
cd /home/hadoop/test

vim test1.txt

# 添加如下内容
hello world
hello linux
hello python

# 2.测试scp（也可以写成scp -r ./test  hadoop03:/home/hadoop）
scp -r ../test  hadoop102:$PWD

# 测试rsync
rsync -av ../test  hadoop103:$PWD
```

```shell
# 在hadoop101上编写集群分发脚本

# 1.把集群分发脚本放在家目录的bin下
mkdir /home/hadoop/bin
cd /home/hadoop/bin

# 2. 创建集群分发脚本（xsync 要同步的文件名称）
vim xsync

# 添加如下内容
```

```shell
#!/bin/bash

#1. 判断参数个数
if [ $# -lt 1 ]
then
	echo "Not Enough Arguement!"
	exit;
fi

#2. 遍历集群所有机器
for host in hadoop101 hadoop102 hadoop103
do
	echo ==================== $host ====================
	# 3.遍历所有目录，挨个发送
	for file in $@
	do
		# 4.判断文件是否存在
		if [ -e $file ]
		then
			# 获取父目录 -p获取真实目录，比如软链接会给出真出实际目录
			pdir=$(cd -P $(dirname $file); pwd)
			# 获取当前文件的名称
			fname=$(basename $file)
			# 如果目录不存在，则创建
			ssh $host "mkdir -p $pdir"
			rsync -av $pdir/$fname $host:$pdir
		else
			echo $file does not exists!
		fi
	done
done
```

```shell
# 1.修改脚本 xsync 具有执行权限
chmod +x xsync

# 2.测试脚本是否可用
xsync /home/hadoop/bin

# 将脚本复制到/bin 中，以便全局调用
sudo cp xsync /bin/
```

```shell
# 同步环境变量配置
xsync /etc/profile.d/my_env.sh

# 同步/opt/module/目录
xsync /opt/module/

# 在hadoop102和hadoop103执行以下语句， 环境变量生效
source /etc/profile
```
### 3.6 初始化namenode的元数据目录

只需要执行一次，生成一个全新的元数据存储目录（具体路径：/opt/data/hadoop-3.3.6/dfs/name/current）
，生成记录元数据的存储目录fsimage和生成集群的相关标识，如==**集群id-clusterID**==

```shell
# hadoop101上执行初始化动作
hadoop namenode -format
```
## 四、启动集群

日志可以在以下路径查看

```shell
/opt/module/hadoop-3.3.6/logs
```
### 4.1 逐个启动

要先启动namenode，再启动datanode。

首次启动datanode会生成==**clusterID**==（/opt/data/hadoop-3.3.6/dfs/data），后面会根据这个id跟namenode通讯。

```shell
# 启动或停止Hdfs

# 1.启动namenode（在hadoop101）
hdfs --daemon start namenode
# 2.启动datanode（可在任意机器）
hdfs --daemon start datanode
# 3.启动secondarynamenode（在hadoop103）
hdfs --daemon start seconarynamenode
# 关闭，把启动命令的start改为stop
```

```shell
# 启动或停止yarn

# 1.启动resourcemanager（在hadoop102）
yarn --daemon start resourcemanager
# 2.启动nodemanager（任意机器）
yarn --daemon start nodemanager

# 关闭，把启动命令的start改为stop
```

```shell
# 在 hadoop101 启动历史服务器
mapred --daemon start historyserver

# 关闭，把启动命令的start改为stop
```
### 4.2 一键启动

根据workers文件，启动datanodes

```shell
# 一键启动hdfs（在hadoop101）
start-dfs.sh

# 一键启动yarn（在hadoop102）
start-yarn.sh

# 在 hadoop101 启动历史服务器
mapred --daemon start historyserver

# 关闭，把启动命令的start改为stop
```
### 4.3 网页访问

```shell
# web端查看HDFS的NameNodo（查看HDFS上存储的数据信息）
http://hadoop101:9870

# web端查看YARN的resourceManager（查看Yarn上运行的Job信息）
http://hadoop102:8088

# web端查看历史任务JobHistory 
http://hadoop101:19888
```

```shell
# 1.在namenode所在机器查看namenode进程id(jps可以监听所有的java程序)
jps 
# 2.通过进程id查找namenode监控的端口
netstat -ntlp|grep 进程id
# 3.浏览访问namenode提供的web:
hadoop02:端口号（http://hadoop02:9870，旧版本是http://hadoop02:50070）
# 4.同理，也可以通过相同的方法访问datanode提供的web
```

==查看HDFS上存储的数据信息：== ^41540d

![[Pasted image 20230921173827.png|400]]

![[Pasted image 20230921173853.png|400]]

==查看Yarn上运行的Job信息：== ^6b710f

![[Pasted image 20230921174035.png|425]]

![[Pasted image 20230921174225.png|425]]

![[Pasted image 20230921174323.png|425]]

![[Pasted image 20230921174406.png|425]]

==查看历史任务：==

![[Pasted image 20230921174809.png|450]]
### 4.4 测试集群

（1）上传文件。可通过页面查看[[#^41540d]]

```shell
# 测试上传文件

# 1.在hdfs上创建input目录（或hdfs dfs -mkdir /input）
hadoop fs -mkdir /input

# 2.上传小文件
hadoop fs -put /home/hadoop/test/test1.txt /input

# 3. 上传大文件
hadoop fs -put /opt/software/jdk-8u144-linux-x64.tar.gz /

# 4.查看文件存储在什么位置
cd /opt/data/hadoop-3.3.6/dfs/data/current/BP-1344113917-192.168.204.102-1693305174390/current/finalized/subdir0/subdir0

# 5.查看 HDFS 在磁盘存储文件内容
# 小文件
cat blk_1073741825

# 大文件（需要拼接）
cat blk_1073741826 >>tmp.tar.gz
cat blk_1073741827 >>tmp.tar.gz
tar -zxvf tmp.tar.gz

# 删除
rm -rf tmp.tar.gz
rm -rf jdk1.8.0_144/
```

（2）下载文件

```shell
hadoop fs -get /jdk-8u144-linux-x64.tar.gz /home/hadoop
```

（3）执行wordcount 程序。可通过页面查看[[#^6b710f]]

```shell
hadoop jar /opt/module/hadoop-3.3.6/share/hadoop/mapreduce/hadoop-mapreduce-examples-3.3.6.jar wordcount /input /output
```
### 4.5 编写集群常用脚本

```shell
cd /home/hadoop/bin
```

（1）集群启动、关闭脚本

```shell
# 使用方式：myhadoop.sh start 或 myhadoop.sh start
vim myhadoop.sh

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
	echo " =================== 启动 hadoop 集群 ==================="
	echo " --------------- 启动 hdfs ---------------"
	ssh hadoop101 "start-dfs.sh"
	echo " --------------- 启动 yarn ---------------"
	ssh hadoop102 "start-yarn.sh"
	echo " --------------- 启动 historyserver ---------------"
	ssh hadoop101 "mapred --daemon start historyserver"
;;
"stop")
	echo " =================== 关闭 hadoop 集群 ==================="
	echo " --------------- 关闭 historyserver ---------------"
	ssh hadoop101 "mapred --daemon stop historyserver"
	echo " --------------- 关闭 yarn ---------------"
	ssh hadoop102 "stop-yarn.sh"
	echo " --------------- 关闭 hdfs ---------------"
	ssh hadoop101 "stop-dfs.sh"
;;
*)
	echo "Input Args Error..."
;;
esac
```

```shell
# 赋予脚本执行权限
chmod +x myhadoop.sh

# 将脚本复制到/bin 中，以便全局调用
sudo cp myhadoop.sh /bin/
```

（2）查看三台服务器 Java 进程脚本

```shell
# 使用方式：jpsall
vim jpsall

# 添加如下内容
```

```shell
#!/bin/bash
for host in hadoop101 hadoop102 hadoop103
do
	echo =============== $host ===============
	ssh $host jps
done
```

```shell
# 赋予脚本执行权限
chmod +x jpsall

# 将脚本复制到/bin 中，以便全局调用
sudo cp jpsall /bin/
```

```shell
# 把脚本同步到其他机器
xsync /home/hadoop/bin
xsync /bin/xsync /bin/myhadoop.sh /bin/jpsall
```