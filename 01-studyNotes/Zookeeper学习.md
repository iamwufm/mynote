---
author: wufm
title: Zookeeper学习
rating: 2
time: 2023-09-23 周六
tags:
  - Zookeeper
代码: bigdata项目的下的zookeeper
---

## 一、Zookeeper入门

### 1.1 概述

Zookeeper官网：[Apache ZooKeeper](https://zookeeper.apache.org/)

Zookeeper 是一个开源的分布式的，为分布式框架提供协调服务的 Apache 项目。

==Zookeeper=文件系统+通知机制==：
- 可以为客户端管理==少量数据==（一个节点做多是1M）
- 可以为客户端监听指定数据节点的状态，并在数据节点发生变化时，通知客户端
### 1.2 特点

![[zookeeper特点.excalidraw]]

1）一个领导者（Leader），多个跟随者（Follower）组成的集群。
2）集群中只要有半数以上节点存活，Zookeeper集群就能正常服务。所以Zookeeper适合安装奇数台服务器。
3）==全局数据一致==：==每个Server保存一份相同的数据副本，Client无论连接到哪个Server，数据都是一致的==。
4）更新请求顺序执行，来自同一个Client的更新请求按其发送顺序依次执行。
5）数据更新原子性，一次数据更新要么成功，要么失败。
6）实时性，在一定时间范围内，Client能读到最新数据。
### 1.3 数据结构和节点类型

ZooKeeper 数据模型的结构与 Unix 文件系统很类似，整体上可以看作是一棵树，每个节点称做一个 ZNode。==每一个 ZNode 默认能够存储 1MB 的数据==，每个 ZNode 都可以通过其路径唯一标识。

![[zookeeper数据结构.excalidraw|600]]
### 1.4 应用场景

提供的服务包括：统一命名服务、统一配置管理、统一集群管理、服务器节点动态上下线、软负载均衡等。
#### 1.4.1 服务器上下线的动态感知

客户端能实时洞察到服务器上下线的变化

![[服务器上下线的动态感知.excalidraw]]

#### 1.4.2 统一配置管理

分布式环境下，配置文件同步非常常见。配置管理可交由ZooKeeper实现。

![[统一配置管理.excalidraw|400]]

## 二、Zookeeper安装

参见[[Zookeeper安装]]
## 三、Zookeeper集群操作

### 3.1 客户端命令行操作（shell）

先启动客户端，再进行操作。可以使用help查看有哪些命令

```shell
# 1.启动客户端
# 启动本地客户端
zkCli.sh
# 启动客户端，指定连接的服务器
zkCli.sh -server hadoop102:2181
```


```shell
# 查看所有操作命令
help
```

（1）查看：

```shell
# 1.查看
# 查看节点信息
ls /
# 查看节点详细信息
ls -s /
```

```txt
czxid：创建节点的事务 zxid。每次修改 ZooKeeper 状态都会产生一个 ZooKeeper 事务 ID
ctime：znode 被创建的毫秒数（从 1970 年开始）
mzxid：znode 最后更新的事务 zxid
mtime：znode 最后修改的毫秒数（从 1970 年开始）
pZxid：znode 最后更新的子节点 zxid
cversion：znode 子节点变化号，znode 子节点修改次数
dataversion：znode 数据变化号
aclVersion：znode 访问控制列表的变化号
ephemeralOwner：如果是临时节点，这个是 znode 拥有者的 session id。如果不是临时节点则是 0。
dataLength：znode 的数据长度
numChildren：znode 子节点数量
```

（2）创建节点

```shell
# 带内容 永久节点（临时节点加 -e） + 不带序号 (带序号加 -s)
create /boy "wulei"

# 不带内容
create /girl
create /sanguo

# 设置内容或修改内容
set /girl "yanmi"

# 获取节点内容（获取详情加 -s）
get /girl

# 创建子节点
create -e -s /boy/handsome

create -s /boy/ugly

# 关闭客户端
quit

# 再次启动，已经看不到/boy/handsome
ls /boy
```

（3）删除节点

```shell
deleteall /boy
delete /girl
```

（4）监听节点的值变化

```shell
# 在hadoop103上注册监听
get -w /sanguo

# 修改/sanguo内容（在hadoop101执行）
set /sanguo "wuguo"
```

在hadoop101再多次修改/sanguo的值，hadoop103上不会再收到监听。因为注册一次，只能监听一次。想再次监听，需要再次注册

（5）监听节点的子节点变化（路径变化）

```shell
# 在hadoop103上注册监听
ls -w /sanguo

# 增加一个字节点（在hadoop101执行）
create /sanguo/weiguo

delete /sanguo/weiguo/liu
```
### 3.2 客户端API操作（java）

zookeeper的增删查改：com.bigdata.zk.demo.ZookeeperClientDemo

监听节点的值变化/监听节点的子节点变化（路径变化）：com.bigdata.zk.demo.ZookeeperWatchDemo

## 四、服务器上下线动态感知案例

![[服务器上下线动态感知案例.excalidraw|500]]

需求：启动多个服务器，为客户端提供时间查询。客户端随机请求某一台服务器查询时间，某台服务器挂了也不影响客户端请求需求。

代码：
- 服务器：com.bigdata.zk.distributesystem.TimeQueryServer
- 客户端：com.bigdata.zk.distributesystem.TimeQueryClient
## 五、ZooKeeper分布式锁案例

什么叫做分布式锁呢？

比如说"进程 1"在使用该资源的时候，会先去获得锁，"进程 1"获得锁以后会对该资源保持独占，这样其他进程就无法访问该资源，"进程 1"用完该资源以后就将锁释放掉，让其他进程来获得锁，那么通过这个锁机制，我们就能保证了分布式系统中多个进程能够有序的访问该临界资源。那么我们把这个分布式环境下的这个锁叫作分布式锁。

![[ZooKeeper分布式锁案例.excalidraw|600]]


需求：客户端们有顺序的执行，一个客户端处理完业务后再到另外一个客户端。

代码：com.bigdata.zk.distributeLock.DistributeLockTestClient

## 六、企业面试真题（面试重点）

### 6.1 选举机制

选择一个领导者（Leader），超过半数的投票通过。

1）第一次启动选举规则：

投票过半数时，服务器id大的胜出（myid文件的值）

2）第二次启动选举规则：

（1）EPOCH大的直接胜出

（2）EPOCH相同，事务id大的胜出

（3）事务id相同，服务器id大的胜出
### 6.2 生产集群安装多少 zk 合适？

安装奇数台。

生产经验：

10 台服务器：3 台 zk；

20 台服务器：5 台 zk；

100 台服务器：11 台 zk；

200 台服务器：11 台 zk

服务器台数多：好处，提高可靠性；坏处：提高通信延时
### 6.2 常用命令

ls、get、create、delete