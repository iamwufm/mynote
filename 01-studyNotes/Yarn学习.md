---
author: wufm
title: Yarn学习
rating: 3
time: 2023-09-13 周三
tags:
  - Hadoop
  - Yarn
  - 分布式资源管理器
代码:
---
## 一、Yarn资源调度器

1. 如果管理集群资源？
2. 如何给任务分配资源？

Yarn 是一个资源调度平台，负责为运算程序提供服务器运算资源，相当于一个分布式的操作系统平台，而 MapReduce 等运算程序则相当于运行于操作系统之上的应用程序。
### 1.1 Yarn基础架构

YARN 主要由 ResourceManager、NodeManager、ApplicationMaster 和 Container 等组件构成。
见[[Hadoop3.x学习笔记#1.1.3 Hadoop的组成]] 的第三点。
### 1.2 Yarn工作机制

见[[MapReduce学习#2.2 mapreduce程序在YARN上启动-运行-注销的全流程]]
## 二、Yarn调度器和调度算法

目前，Hadoop 作业调度器主要有三种：FIFO、容量（Capacity Scheduler）和公平（Fair Scheduler）。Apache Hadoop3.1.3 默认的资源调度器是 Capacity Scheduler。

yarn-default.xml

```xml
<property>
<description>The class to use as the resource scheduler.</description>
<name>yarn.resourcemanager.scheduler.class</name>
<value>org.apache.hadoop.yarn.server.resourcemanager.scheduler.capacity.CapacityScheduler</value>
</property>
```
### 2.1 先进先出调度器（FIFO）

FIFO 调度器（First In First Out）：**单队列，根据提交作业的先后顺序，先来先服务。**

优点：简单易懂；

缺点：不支持多队列，生产环境很少使用；

![[FIFO调度器.excalidraw|500]]
### 2.2 容量调度器（Capacity Scheduler）

Capacity Scheduler 是 Yahoo 开发的多用户调度器。

![[容量调度器.excalidraw|500]]

1. 多队列：每个队列可配置一定的资源量，**每个队列采用FIFO调度策略**
2. 容量保证：管理员可为每个队列设置资源最低保证和资源使用上限
3. 灵活性：如果一个队列中的资源有剩余，可以暂时共享给那些需要资源的队列，而一旦该队列有新的应用程序提交，则其他队列借调的资源会归还给该队列
4. 多租户：
	1. 支持多用户共享集群和多应用程序同时运行
	2. 为了防止同一个用户的作业独占队列中的资源，该调度器会对同一用户提交的作业所占资源量进行限定

![[容器调度器资源分配算法.excalidraw]]
### 2.3 公平调度器（Fair Scheduler）

Fair Schedulere 是 Facebook 开发的多用户调度器。

![[公平调度器特点.png]]


![[Pasted image 20230915171326.png]]

有三个队列 queue1、Queue2、Queue3，每个队列中的 job 按照优先级分配资源，优先级越高分配的资源越多，但是每个 job 都会分配到资源以确保公平。**在资源有限的情况下，每个 job 理想情况下获得的计算资源与实际获得的计算资源存在一种差距，这个差距就叫做差额**。==在同一个队列中，job 的资源缺额越大，越先获得资源优先执行==。作业是按照缺额的高低来先后执行的。在 Fair调度器中，我们不需要预先占用一定的系统资源，Fair 调度器会为所有运行的 job动态的调整系统资源。

1. 与容量调度器不同点
	1. 核心调度策略不同
		- 容量调度器：优先选择**资源利用率低**的队列
		- 公平调度器：优先选择对资源的**缺额比例大**的
	2. 每个队列可以设置资源分配方式不同
		- 容量调度器：FIFO、 DRF
		- 公平调度器：FIFO、FAIR、DRF

#### 2.3.2 公平调度器队列资源分配方式

##### 2.3.2.1 FIFO策略

公平调度器每个队列资源分配策略如果选择FIFO的话，此时公平调度器相当于上面讲过的容量调度器。
##### 2.3.2.2 Fair策略

Fair 策略（默认）是一种基于**最大最小公平算法**实现的资源多路复用方式，默认情况下，每个队列内部采用该方式分配资源。这意味着，如果一个队列中有两个应用程序同时运行，则每个应用程序可得到1/2的资源；如果三个应用程序同时运行，则每个应用程序可得到1/3的资源。

公平调度器可以为所有的应用“平均公平”分配资源，当然，这种“公平”是可以配置的，称为权重，可以在分配文件中为每一个队列设置分配资源的权重，如果没有设置，默认是1（由于默认权重相同，因此，在不做配置的情况下，作业（队列）之间的资源占比相同)。

- 默认地，所有的应用程序在一段时间内平均获得相等的资源份额；
- 默认地，公平调度程序仅基于内存调度公平决策，当然，这种策略也是支持配置的。

具体资源分配流程和容量调度器一致；
- 选择队列
- 选择作业
- 选择容器

以上三步，每一步都是**按照公平策略分配资源**

![[公平策略分配资源.excalidraw]]

➢ 实际最小资源份额：mindshare = Min（资源需求量，配置的最小资源）
mindshare = Min（4，2）= 2
➢ 是否饥饿：isNeedy = 资源使用量 < mindshare
isNeedy = 1 < 2 = yes
➢ 资源分配比：minShareRatio = 资源使用量 / Max（mindshare, 1）
minShareRatio = 1 / Max（2, 1）= 0.5
➢ 资源使用权重比：useToWeightRatio = 资源使用量 / 权重

1. **队列资源分配**
需求：集群总资源100，有三个队列，对资源的需求分别是：
queueA-->20， queueA-->50， queueA-->30

第一次算：100/3=33.33
queueA：分33.33 --> 多13.33
queueB：分33.33 --> 少16.67
queueC：分33.33 --> 多3.33

第二次算：(13.33 + 3.33)/1=16.66
queueA：33.33 - 13.33 = 20
queueB：33.33 + 16.66 = 50
queueC：33.33 - 3.33 = 30

2. **作业资源分配**

![[作业资源分配.excalidraw|600]]
##### 2.3.2.3 DRF策略

DRF（Dominant Resource Fairness），我们之前说的资源，都是单一标准，例如只**考虑内存（也是Yarn默
认的情况）**。但是很多时候我们资源有很多种，例如内存，CPU，网络带宽等，这样我们很难衡量两个应用
应该分配的资源比例。

那么在YARN中，我们用DRF来决定如何调度：
假设集群一共有100 CPU和10T 内存，而应用A需要（2 CPU, 300GB），应用B需要（6 CPU，100GB）。
则两个应用分别需要A（2%CPU, 3%内存）和B（6%CPU, 1%内存）的资源，这就意味着A是内存主导的, B是
CPU主导的，针对这种情况，我们可以选择DRF策略对不同应用进行不同资源（CPU和内存）的一个不同比
例的限制
## 三、Yarn 常用命令

Yarn 状态的查询，除了可以在 hadoop102:8088 页面查看外，还可以通过命令操作。常见的命令操作如下所示：


```shell
# 列出所有 Application
yarn application -list

# Kill 掉 Application：
# yarn application -kill job_id
yarn application -kill application_1612577921195_0001

# 查询 Application 日志
yarn logs -applicationId application_1612577921195_0001

# 根据 Application 状态过滤 yarn application -list -appStates 状态
#状态 ：ALL、NEW、NEW_SAVING、SUBMITTED、ACCEPTED、RUNNING、FINISHED、FAILED、KILLED
yarn application -list -appStates FINISHED

# 查看某个Application
# yarn applicationattempt -list job_id
yarn applicationattempt -list application_1612577921195_0001

# 查看am容器
yarn applicationattempt -status appattempt_1612577921195_0001_000001

# 列出所有容器
yarn container -list appattempt_1612577921195_0001_000001

# 查看容器状态
yarn container -status container_1612577921195_0001_01_000001
```


```shell
# 列出所有节点
yarn node -list -all

# 加载队列配置
yarn rmadmin -refreshQueues

# 打印队列信息 yarn queue -status <QueueName>
yarn queue -status default
```
## 四、Yarn 生产环境核心参数

1. ResourceManager相关

```txt
yarn.resourcemanager.scheduler.class 配置调度器，默认容量
yarn.resourcemanager.scheduler.client.thread-count ResourceManager处理调度器请求的线程数量，默认50。但是不能超过 3 台 * 16 线程 = 48 线程（去除其他应用程序实际不能超过 44）
```

2. NodeManager相关

```txt
yarn.nodemanager.resource.detect-hardware-capabilities 是否让yarn自己检测硬件进行配置，默认false
yarn.nodemanager.resource.count-logical-processors-as-cores 是否将虚拟核数当作CPU核数，默认false
yarn.nodemanager.resource.pcores-vcores-multiplier虚拟核数和物理核数乘数，例如：4核8线程，该参数就应设为2，默认1.0
yarn.nodemanager.resource.memory-mb NodeManager使用内存，默认8G
yarn.nodemanager.resource.system-reserved-memory-mb NodeManager为系统保留多少内存
以上二个参数配置一个即可
yarn.nodemanager.resource.cpu-vcores NodeManager使用CPU核数，默认8个
yarn.nodemanager.pmem-check-enabled是否开启物理内存检查限制container，默认打开
yarn.nodemanager.vmem-check-enabled是否开启虚拟内存检查限制container，默认打开，建议修改为关闭
yarn.nodemanager.vmem-pmem-ratio虚拟内存物理内存比例，默认2.1
```
Contain相关

```txt
yarn.scheduler.minimum-allocation-mb 容器最最小内存，默认1G
yarn.scheduler.maximum-allocation-mb 容器最最大内存，默认8G
yarn.scheduler.minimum-allocation-vcores 容器最小CPU核数，默认1个
yarn.scheduler.maximum-allocation-vcores 容器最大CPU核数，默认4个
```

## 五、Yarn 案例实操

注：
- ==调整下列参数之前尽量拍摄 Linux 快照，否则后续的案例，还需要重写准备集群。==
- ==以下操作全部做完过后，快照回去或者手动将配置文件修改成之前的状态，因为本
身资源就不够，分成了这么多，不方便以后测试==
### 5.1 Yarn生产环境核心参数配置

每台服务器的资源：4G 内存，8核CPU，16线程。

![[Pasted image 20230914175438.png]]

```shell
# 【逻辑CPU数量和型号】
cat /proc/cpuinfo | grep name | cut -f2 -d: | uniq -c
# 【物理CPU数量】
cat /proc/cpuinfo | grep "physical id" | sort | uniq | wc -l
# 【物理CPU内核的个数】（1个物理CPU里面有几个物理内核）
cat /proc/cpuinfo | grep "cpu cores" | uniq

【逻辑CPU数量和型号】 16  AMD Ryzen 7 PRO 6850HS with Radeon Graphics
【物理CPU数量】2
【物理CPU内核的个数】cpu cores       : 8

# 查看内存
free -h
```

修改 yarn-site.xml 文件，加入以下参数：

```shell
cd /opt/module/hadoop-3.3.6/etc/hadoop
vim yarn-site.xml
```

```xml
<!-- ResourceManager 处理调度器请求的线程数量,默认 50；如果提交的任务数大于 50，可以
增加该值，但是不能超过 3 台 * 16 线程 = 48 线程（去除其他应用程序实际不能超过 44） -->
<property>
<name>yarn.resourcemanager.scheduler.client.thread-count</name>
<value>44</value>
</property>

<!-- NodeManager 使用内存数，默认 8G，修改为 4G 内存 -->
<property>
<name>yarn.nodemanager.resource.memory-mb</name>
<value>4096</value>
</property>

<!-- nodemanager 的 CPU 核数，不按照硬件环境自动设定时默认是 8 个 -->
<property>
<name>yarn.nodemanager.resource.cpu-vcores</name>
<value>8</value>
</property>

<!-- 容器最小内存，默认 1G -->
<property>
<name>>yarn.scheduler.minimum-allocation-mb</name>
<value>1024</value>
</property>

<!-- 容器最大内存，默认 8G，修改为 2G -->
<property>
<name>yarn.scheduler.maximum-allocation-mb</name>
<value>2048</value>
</property>

<!-- 容器最小 CPU 核数，默认 1 个 -->
<property>
<name>yarn.scheduler.minimum-allocation-vcores</name>
<value>1</value>
</property>

<!-- 容器最大CPU 核数，默认 1 个 -->
<property>
<name>yarn.scheduler.maximum-allocation-vcores</name>
<value>2</value>
</property>
```

分发配置（注意：如果集群的硬件资源不一致，要每个 NodeManager 单独配置）：

```shell
xsync yarn-site.xml
```

重启集群：

```shell
myhadoop.sh stop
myhadoop.sh start
```

执行wordcount

```shell
hadoop jar /opt/module/hadoop-3.3.6/share/hadoop/mapreduce/hadoop-mapreduce-examples-3.3.6.jar wordcount /input /output
```

### 5.2 容量调度器多队列提交案例

1. 在生产环境怎么创建队列？
	-  调度器默认就 1 个 default 队列，不能满足生产要求。
	- 按什么规则创建
		- 按照框架：hive /spark/ flink 每个框架的任务放入指定的队列（企业用的不是特别多）
		- 按照业务模块：登录注册、购物车、下单、业务部门 1、业务部门 2
2. 创建多队列的好处？
	- 因为担心员工不小心，写递归死循环代码，把所有资源全部耗尽。
	- 实现任务的降级使用，特殊时期保证重要的任务队列资源充足。11.11 6.18。业务部门 1（重要）=》业务部门 2（比较重要）=》下单（一般）=》购物车（一般）=》登录注册（次要）

#### 5.2.1 需求

需求 1：default 队列占总内存的 40%，最大资源容量占总资源 60%；hive 队列占总内存的 60%，最大资源容量占总资源 80%。

需求 2：配置队列优先级

#### 5.2.2 配置多队列的容量调度器

在 capacity-scheduler.xml 中配置：

```shell
cd /opt/module/hadoop-3.3.6/etc/hadoop
vim capacity-scheduler.xml
```

修改如下配置:

```xml
<!-- 指定多队列，增加 hive 队列 -->
<property>
<name>yarn.scheduler.capacity.root.queues</name>
<value>default,hive</value>
<description>
The queues at the this level (root is the root queue).
</description>
</property>

<!-- 降低 default 队列资源额定容量为 40%，默认 100% -->
<property>
<name>yarn.scheduler.capacity.root.default.capacity</name>
<value>40</value>
</property>

<!-- 降低 default 队列资源最大容量为 60%，默认 100% -->
<property>
<name>yarn.scheduler.capacity.root.default.maximum-capacity</name>
<value>60</value>
</property>
```

为新加队列添加必要属性：

```xml
<!-- 指定 hive 队列的资源额定容量 -->
<property>
<name>yarn.scheduler.capacity.root.hive.capacity</name>
<value>60</value>
</property>

<!-- 用户最多可以使用队列多少资源，1 表示 -->
<property>
<name>yarn.scheduler.capacity.root.hive.user-limit-factor</name>
<value>1</value>
</property>

<!-- 指定 hive 队列的资源最大容量 -->
<property>
<name>yarn.scheduler.capacity.root.hive.maximum-capacity</name>
<value>80</value>
</property>

<!-- 启动 hive 队列 -->
<property>
<name>yarn.scheduler.capacity.root.hive.state</name>
<value>RUNNING</value>
</property>

<!-- 哪些用户有权向队列提交作业 -->
<property>
<name>yarn.scheduler.capacity.root.hive.acl_submit_applications</name>
<value>*</value>
</property>

<!-- 哪些用户有权操作队列，管理员权限（查看/杀死） -->
<property>
<name>yarn.scheduler.capacity.root.hive.acl_administer_queue</name>
<value>*</value>
</property>

<!-- 哪些用户有权配置提交任务优先级 -->
<property>
<name>yarn.scheduler.capacity.root.hive.acl_application_max_priority</name>
<value>*</value>
</property>

<!-- 如果 application 指定了超时时间，则提交到该队列的 application 能够指定的最大超时时间不能超过该值。-->
<property>
<name>yarn.scheduler.capacity.root.hive.maximum-application-lifetime</name>
<value>-1</value>
</property>

<!-- 如果 application 没指定超时时间，则用 default-application-lifetime 作为默认值 -->
<property>
<name>yarn.scheduler.capacity.root.hive.default-application-lifetime</name>
<value>-1</value>
</property>
```

分发配置、重启 Yarn 或者执行 yarn rmadmin -refreshQueues 刷新队列，就可以看到两条队列：

```shell
xsync capacity-scheduler.xml

myhadoop.sh stop/myhadoop.sh start
# 或
yarn rmadmin -refreshQueues
```

![[Pasted image 20230915112548.png]]

#### 5.2.3 测试提交任务

```shell
hadoop jar /opt/module/hadoop-3.3.6/share/hadoop/mapreduce/hadoop-mapreduce-examples-3.3.6.jar wordcount /input /output

# 向 Hive 队列提交任务 注: -D 表示运行时改变参数值（也可以在代码中指定conf.set("mapreduce.job.queuename","hive")）
hadoop jar /opt/module/hadoop-3.3.6/share/hadoop/mapreduce/hadoop-mapreduce-examples-3.3.6.jar wordcount -D mapreduce.job.queuename=hive /input /output
```


任务优先级
- 容量调度器，支持任务优先级的配置，在资源紧张时，优先级高的任务将优先获取资源。
- 默认情况，Yarn 将所有任务的优先级限制为 0，若想使用任务的优先级功能，须开放该限制。

修改 yarn-site.xml 文件，增加以下参数

```shell
cd /opt/module/hadoop-3.3.6/etc/hadoop
vim yarn-site.xml
```

```xml
<property>
<name>yarn.cluster.max-application-priority</name>
<value>5</value>
</property>
```

分发配置，并重启 Yarn

```shell
xsync yarn-site.xml
# hadoop102执行
stop-yarn.sh
start-yarn.sh
```

模拟资源紧张环境，可连续提交以下任务，直到新提交的任务申请不到资源为止。

```shell
hadoop jar /opt/module/hadoop-3.3.6/share/hadoop/mapreduce/hadoop-mapreduce-examples-3.3.6.jar pi 5 2000000
```

再次重新提交优先级高的任务

```shell
hadoop jar /opt/module/hadoop-3.3.6/share/hadoop/mapreduce/hadoop-mapreduce-examples-3.3.6.jar pi -D mapreduce.job.priority=5 5 2000000

# 也可以通过以下命令修改正在执行的任务的优先级。
yarn application -appID application_1611133087930_0009 -updatePriority 5
```

### 5.3 公平调度器案例

#### 5.3.1 需求

创建两个队列，分别是 test 和 iamwfm（以用户所属组命名）。期望实现以下效果：若用户提交任务时指定队列，则任务提交到指定队列运行；若未指定队列，test 用户提交的任务到 root.test 队列运，iamwfm 提交的任务到 root.iamwfm 队列运行）。
#### 5.3.2 配置多队列的公平调度器

公平调度器的配置涉及到两个文件，一个是 yarn-site.xml，另一个是公平调度器队列分配文件 fair-scheduler.xml（文件名可自定义）。

（1）配置文件参考资料：
[Apache Hadoop 3.3.6 – Hadoop： Fair Scheduler](https://hadoop.apache.org/docs/stable/hadoop-yarn/hadoop-yarn-site/FairScheduler.html)

2）任务队列放置规则参考资料：
[[hadoop-2.9.2] Fair Scheduler - 简书 (jianshu.com)](https://www.jianshu.com/p/78f3e4421cf2)

修改 yarn-site.xml 文件，加入以下参数

```shell
cd /opt/module/hadoop-3.3.6/etc/hadoop
vim yarn-site.xml
```

```xml
<!--配置使用公平调度器-->
<property>
  <name>yarn.resourcemanager.scheduler.class</name>
  <value>org.apache.hadoop.yarn.server.resourcemanager.scheduler.fair.FairScheduler</value>
</property>

<!--指明公平调度器队列分配配置文件-->
<property>
<name>yarn.scheduler.fair.allocation.file</name>
<value>/opt/module/hadoop-3.3.6/etc/hadoop/fair-scheduler.xml</value>
</property>

<!--禁止队列间资源抢占-->
<property>
<name>yarn.scheduler.fair.preemption</name>
<value>false</value>
</property>
```

配置 fair-scheduler.xml，添加如下内容：

```shell
vim fair-scheduler.xml
```

```xml
<?xml version="1.0"?>
<allocations>
<!-- 单个队列中 Application Master 占用资源的最大比例,取值 0-1 ，企业一般配置 0.1-->
<queueMaxAMShareDefault>0.5</queueMaxAMShareDefault>
<!-- 单个队列最大资源的默认值 test iamwfm default -->
<queueMaxResourcesDefault>4096mb,4vcores</queueMaxResourcesDefault>

<!-- 增加一个队列 test -->
<queue name="test">
<!-- 队列最小资源 -->
<minResources>2048mb,2vcores</minResources>
<!-- 队列最大资源 -->
<maxResources>4096mb,4vcores</maxResources>
<!-- 队列中最多同时运行的应用数，默认 50，根据线程数配置 -->
<maxRunningApps>4</maxRunningApps>
<!-- 队列中 Application Master 占用资源的最大比例 -->
<maxAMShare>0.5</maxAMShare>
<!-- 该队列资源权重,默认值为 1.0 -->
<weight>1.0</weight>
<!-- 队列内部的资源分配策略 -->
<schedulingPolicy>fair</schedulingPolicy>
</queue>

<!-- 增加一个队列 iamwfm -->
<queue name="iamwfm">
<!-- 队列最小资源 -->
<minResources>2048mb,2vcores</minResources>
<!-- 队列最大资源 -->
<maxResources>4096mb,4vcores</maxResources>
<!-- 队列中最多同时运行的应用数，默认 50，根据线程数配置 -->
<maxRunningApps>4</maxRunningApps>
<!-- 队列中 Application Master 占用资源的最大比例 -->
<maxAMShare>0.5</maxAMShare>
<!-- 该队列资源权重,默认值为 1.0 -->
<weight>1.0</weight>
<!-- 队列内部的资源分配策略 -->
<schedulingPolicy>fair</schedulingPolicy>
</queue>

<!-- 任务队列分配策略,可配置多层规则,从第一个规则开始匹配,直到匹配成功 -->
<queuePlacementPolicy>
<!-- 提交任务时指定队列,如未指定提交队列,则继续匹配下一个规则; false 表示：如果指
定队列不存在,不允许自动创建-->
<rule name="specified" create="false"/>
<!-- 提交到 root.group.username 队列,若 root.group 不存在,不允许自动创建；若
root.group.user 不存在,允许自动创建 -->
<rule name="nestedUserQueue" create="true">
<rule name="primaryGroup" create="false"/>
</rule>
<!-- 最后一个规则必须为 reject 或者 default。Reject 表示拒绝创建提交失败，
default 表示把任务提交到 default 队列 -->
<!-- 提交到固定的队列iamwfm中，不存在则提交到default队列中 -->
<rule name="default" queue="iamwfm"/>
</queuePlacementPolicy>
</allocations>
```

分发配置，并重启 Yarn

```shell
xsync yarn-site.xml fair-scheduler.xml
# hadoop102执行
stop-yarn.sh
start-yarn.sh
```

![[Pasted image 20230915131604.png]]

#### 5.3.3 测试提交任务

提交任务时指定队列，按照配置规则，任务会到指定的 root.test 队列

```shell
hadoop jar /opt/module/hadoop-3.3.6/share/hadoop/mapreduce/hadoop-mapreduce-examples-3.3.6.jar pi -D mapreduce.job.queuename=test 1 1
```

提交任务时不指定队列，按照配置规则，任务会到 root.atguigu.atguigu 队列

```shell
hadoop jar /opt/module/hadoop-3.3.6/share/hadoop/mapreduce/hadoop-mapreduce-examples-3.3.6.jar pi 1 1
```

### 5.4 Yarn 的 Tool 接口案例

```shell
# -D 参数不生效
hadoop jar wc.jar com.bigdata.mr.wordCount.JobSubmitterLinuxToYarn -D mapreduce.job.queuename=test
```

==需求==：自己写的程序也可以动态修改参数。编写 Yarn 的 Tool 接口。

**代码**：com.bigdata.mr.wordCount.WordCountLinuxToYarn

```shell
hadoop jar wc.jar com.bigdata.mr.wordCount.WordCountLinuxToYarn wordcount -D mapreduce.job.queuename=test
```

==注：以上操作全部做完过后，快照回去或者手动将配置文件修改成之前的状态，因为本身资源就不够，分成了这么多，不方便以后测试==