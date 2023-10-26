---
author: wufm
title: ElasticSearch8.x安装
rating: 4
time: 2023-10-24 周二
tags:
  - elasticSearch
代码:
---
## 一、集群的组成

安装在三台机器上

| hadoop101     | hadoop102     | hadoop103     |
| ------------- | ------------- | ------------- |
| ElasticSearch       | ElasticSearch       |    ElasticSearch           |
## 二、环境的准备

### 2.1 安装JDK

见[[Hadoop3.x集群安装#2.2.7 安装JDK]]
## 三、安装ElasticSearch集群

### 3.1 下载ElasticSearch安装包

下载地址：[Download Elasticsearch | Elastic](https://www.elastic.co/cn/downloads/elasticsearch)
### 3.2 安装ElasticSearch

在hadoop101机器上进行

```shell
# 1.把安装包放在/opt/software/目录下
cd /opt/software/
rz

# 2.解压elasticsearch到/opt/module 目录下
tar -zxf elasticsearch-8.10.4-linux-x86_64.tar.gz -C /opt/module/

# 3.配置Hadoop环境变量（在hadoop101-103都需要修改）
sudo vim /etc/profile.d/my_env.sh

# 添加如下内容
#ELASTICSEARCH_HOME
export ELASTICSEARCH_HOME=/opt/module/elasticsearch-8.10.4
export PATH=$PATH:$ELASTICSEARCH_HOME/bin

# 4.让环境变量生效
source /etc/profile
```

每台机器都要配置：

```shell
#查看可打开文件数量
ulimit -Hn
#查看虚拟内存的大小
sudo sysctl -p

#用户最大可创建文件数太小
sudo vim /etc/security/limits.conf

# 添加如下参数
* soft nofile 65535
* hard nofile 65535

#最大虚拟内存太小
sudo vim /etc/sysctl.conf

# 添加如下参数
vm.max_map_count=262144
```

否则启动会出现如下错误：

```txt

bootstrap check failure [1] of [2]: max file descriptors [4096] for elasticsearch process is too low, increase to at least [65535]
bootstrap check failure [2] of [2]: max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]
```

修改后记得删除日志文件

```shell
rm -rf /opt/data/elasticsearch-8.10.4/logs/
```
### 3.3 修改ElasticSearch

```shell
cd /opt/module/elasticsearch-8.10.4/config

vim elasticsearch.yml

# 修改如下内容
```

```yml
# 集群名称，通过组播的方式通信，通过名称判断属于哪个集群
cluster.name: my-es

# 节点名称，要唯一
node.name: node101

#数据存放位置
path.data: /opt/data/elasticsearch-8.10.4/data
#日志存放位置(可选)
path.logs: /opt/data/elasticsearch-8.10.4/logs

# 是否锁定内存
bootstrap.memory_lock: false

#es绑定的ip地址
network.host: hadoop101

#初始化时可进行选举的节点
discovery.seed_hosts: ["hadoop101", "hadoop102", "hadoop103"]
cluster.initial_master_nodes: ["node101", "node102", "node103"]

# 是否启动安全模式
xpack.security.enabled: false
```
### 3.4 分发脚本

```shell
# 发至hadoop102以及hadoop103，分发之后修改配置文件：
cd /opt/module

xsync elasticsearch-8.10.4/

# 修改配置文件
cd /opt/module/elasticsearch-8.10.4/config
vim elasticsearch.yml

#在hadoop102机器修改以下信息
node.name: node102
network.host: hadoop102

#在hadoop103机器修改以下信息
node.name: node103
network.host: hadoop103
```

==重启机器！！！！！==
## 四、启动集群

### 4.1 逐个启动或关闭

```shell
# 1.启动集群
# 分别在三台机器启动hadoop101-103， 也可以后台启动 elasticsearch -d
elasticsearch

或

elasticsearch -d

# 2.关闭集群 jps 查看进程号
kill 进程号

或
kill `ps -ef | grep Elasticsearch | grep -v grep | awk '{print $2}'`

或

ps -ef | grep Elasticsearch | grep -v grep | awk '{print $2}' | xargs kill
```

### 4.2 编写一键启动/关闭脚本

```shell
cd /home/hadoop/bin

# 启动方式：myes.sh start
# 关闭方式：myes.sh stop
vim myes.sh

# 添加如下内容
```

```shell
#!/bin/bash
if(($#!=1))
then
	echo 请输入单个start或stop参数！
	exit
fi

if [ $1 = start ]
then
	cmd="nohup elasticsearch > /home/hadoop/null  2>&1 &"   
elif [ $1 = stop ]
then
	cmd="ps -ef | grep Elasticsearch | grep -v grep | awk '{print \$2}' | xargs kill"
else
	echo 请输入单个start或stop参数！
fi

for i in hadoop101 hadoop102 hadoop103
do
	echo "--------------$i-----------------"
	ssh $i $cmd
	sleep 3s
done
```

```shell
# 赋予执行权限
chmod +x myes.sh
```

### 4.3 验证是否启动

#### 4.3.1 查看单台机器

```shell
curl http://hadoop101:9200

hadoop101:9200
```

![[Pasted image 20231024181225.png|300]]


![[Pasted image 20231024181316.png|400]]

#### 4.3.2 查看集群是否正常

```shell
#查看集群状态
curl -XGET 'http://hadoop101:9200/_cluster/health?pretty'
http://hadoop101:9200/_cluster/health?pretty
http://hadoop101:9200/_cat/nodes
```


如果集群有问题，可以尝试删除data和logs目录，再重启。再不行再详细看日志

status 字段指示着当前集群在总体上是否工作正常。它的三种颜色含义如下：
- green：所有的主分片和副本分片都正常运行。
- yellow：所有的主分片都正常运行，但不是所有的副本分片都正常运行。
- red：有主分片没能正常运行

![[Pasted image 20231024185946.png|400]]

![[Pasted image 20231024190006.png|400]]


![[Pasted image 20231024190045.png|400]]

## 五、安装其他辅助软件

### 5.1 安装Kibana

官网下载：[Download Kibana Free | Get Started Now | Elastic](https://www.elastic.co/cn/downloads/kibana)

Kibana是一个用于探索、可视化和分享ES数据的客户端。因此在任一节点安装即可。将kibana压缩包上传到所安装节点的指定目录。
==安装版本要跟elasticSearch保持一致。==

```shell
# 1.把安装包放在/opt/software/目录下
cd /opt/software/
rz

# 2.解压kibana到/opt/module 目录下()
tar -zxf kibana-8.10.4-linux-x86_64.tar.gz -C /opt/module/

# 修改相关配置，连接Elasticsearch
cd /opt/module/kibana-8.10.4/config

vim kibana.yml 

# 修改如下内容
server.host: "hadoop101"
elasticsearch.hosts: ["http://hadoop101:9200"]
```

启动Kibana

```shell
cd /opt/module/kibana-8.10.4
bin/kibana
```

浏览器访问：hadoop101:5601

![[Pasted image 20231024211031.png|400]]


![[Pasted image 20231024211132.png|1000]]

### 5.2 安装head插件

```shell
sudo yum install -y unzip
sudo yum install -y npm
sudo yum install -y bzip2

# 安装node
# 官网下载安装包：https://nodejs.org/en/download 点击：All download options。太高版本会报错
tar -zxf node-v16.18.1-linux-x64.tar.gz -C /opt/module/

# 3.配置Hadoop环境变量（在hadoop101-103都需要修改）
sudo vim /etc/profile.d/my_env.sh

# 添加如下内容
#NODEJS_HOME
export NODEJS_HOME=/opt/module/node-v16.18.1-linux-x64
export PATH=$PATH:$NODEJS_HOME/bin

# 4.让环境变量生效
source /etc/profile

# 查看版本号
node -v
```

下载head插件包：[GitHub - mobz/elasticsearch-head: A web front end for an elastic search cluster](https://github.com/mobz/elasticsearch-head)


```shell
# 上传解压
unzip elasticsearch-head-master.zip

# 移动
mv elasticsearch-head-master /opt/module/

cd /opt/module/elasticsearch-head-master

# 安装
npm install
```

如果出现报错：

```txt
npm ERR! Downloading https://github.com/Medium/phantomjs/releases/download/v2.1.1/phantomjs-2.1.1-linux-x86_64.tar.bz2
npm ERR! Saving to /tmp/phantomjs/phantomjs-2.1.1-linux-x86_64.tar.bz2
```

```shell
#  从https://github.com/Medium/phantomjs/releases/download/v2.1.1/phantomjs-2.1.1-linux-x86_64.tar.bz2下载
# 把包放入/tmp/phantomjs/

# 安装
npm install
```

修改配置：

```shell
vim Gruntfile.js 

# 再如下内容添加host参数

options: {
		port: 9100,
		base: '.',
		keepalive: true,
		host: '*'
}
                   
```

```shell
vim /opt/module/elasticsearch-8.10.4/config/elasticsearch.yml 

# 添加如下参数
#开启跨域支持
#允许所有人跨域访问 
http.cors.enabled: true 
http.cors.allow-origin: "*"
```

开启服务：

```shell
cd /opt/module/elasticsearch-head-master/
#运行服务
npm run start
```

网页访问：hadoop102:9100

![[Pasted image 20231025121827.png]]

### 5.3 安装IK分词器插件

下载zip包：[Releases · medcl/elasticsearch-analysis-ik (github.com)](https://github.com/medcl/elasticsearch-analysis-ik/releases)

```shell
# 上传zip包
 cd /opt/module/elasticsearch-8.10.4/plugins/
rz

# 解压
mkdir ik
mv elasticsearch-analysis-ik-8.10.4.zip ./ik/
cd ik
unzip elasticsearch-analysis-ik-8.10.4.zip

rm -rf elasticsearch-analysis-ik-8.10.4.zip 
```

```shell
# 把ik目录拷贝到其他的机器
cd ..
xsync ik
```

重启服务即可