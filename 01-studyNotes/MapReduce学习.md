---
author: wufm
title: MapReduce学习
rating: 5
time: 2023-09-05 周二
tags:
  - Hadoop
  - MapReduce
  - 分布式计算框架
代码: bigdata项目的下的hadoop-mapreduce
---
## 一、MapReduce概述

MapReduce 是一个**分布式计算框架**，是用户开发“基于Hadoop的数据分析应用”的核心框架。
### 1.1 MapReduce优缺点

#### 1.1.1 MapReduce优点

1. 易于编程：简单实现一些接口，就可以完成一个分布式程序
2. 良好的拓展性：可以通过简单的增加机器来拓展它的计算能力
3. 高容错性：当其中一台机器挂了，可以把计算任务转移到另外一个节点上运行，不至于这个任务运行失败
4. 适合PB级以上海量数据的离线处理
#### 1.1.2 MapReduce缺点

1. 不擅长实时计算：无法像 MySQL 一样，在毫秒或者秒级内返回结果
2. 不擅长流式计算：输入数据集式静态的，不能动态变化
3. 不擅长DAG(有向无环图)计算：输出结果都会写入到磁盘，会造成大量的磁盘 IO，导致性能非常的低下
### 1.2 核心思想

==分而治之==

就像清点图书馆的有多少书本一样，不是派一个人做完，而是派几个分楼层清点，再把结果汇总。

mapreduce程序应该是在很多机器上并行启动，而且先执行map task，当众多的maptask都处理完自己的数据后，还需要启动众多的reduce task，这个过程如果用用户自己手动调度不太现实，需要一个自动化的调度平台——hadoop中就为运行mapreduce之类的分布式运算程序开发了一个自动化调度平台——YARN。

![[mapreuce简单流程图.excalidraw|400]]

## 二、MapReduce框架的运行机制

### 2.1 mapreduce框架内部核心工作机制

![[mapreduce框架内部核心工作机制.excalidraw|2500]]

map reduce 编程模型：把数据运算程序分为两个阶段：
1. 阶段1：读取原始数据，形成key-value数据（map方法）
2. 阶段2：将阶段1的key-value数据按照相同的key分组聚合（reduce方法）

mapreduce编程模型的具体实现（软件）：hadoop中的mapreduce框架、spark；

hadoop中的mapreduce框架：
1. 对编程模型阶段1的实现就是：map task
2. 对编程模型阶段2的实现就是：reduce task
### 2.2 mapreduce程序在YARN上启动-运行-注销的全流程

![[mapreduce程序在YARN.excalidraw|1200]]

### 2.3 重要概念

#### 2.3.1 MapTask并行度决定机制

==数据块==：Block 是 HDFS 物理上把数据分成一块一块。数据块是 HDFS 存储数据单位。

==数据切片==：数据切片只是在逻辑上对输入进行分片，并不会在磁盘上将其切分成片进行存储。数据切片是 MapReduce 程序计算输入数据的单位，==一个切片会对应启动一个 MapTask==。

==MapTask 并行度由切片个数决定，切片个数由输入文件和切片规则决定。==

1. 一个job的map阶段并行度由客户端在提交job时的切片数决定（有多个切片可以通过日志搜索number of splits来查看）
2. 切片只是在逻辑上对输入进行分片
3. 每一个split切片分配一个maptask并行实例处理
4. 切片时不考虑数据集整体，而是逐个针对每一个文件（实际是块）单独切片
5. 默认情况下，切片大小=blockSize
6. 数据输入的组件FileInputFormat，常见实现类见[[#4.1.1 读数据：InputFormat(输入数据接口)]]

```java
Math.max(minSize, Math.min(maxSize, blockSize))=blockSize
mapreduce.input.fileinputformat.split.minsize=1
mapreduce.input.fileinputformat.split.maxsize=Long.MAXValue
```

![[mapreduce切片.excalidraw|500]]
#### 2.3.2 排序概念

mapreduce程序内置了一个排序机制：

==map task 和reduce task ，都会对数据按照key的大小来排序，所以最终的输出结果中，一定是按照key有顺序的结果。==该操作属于hadoop默认行为。任何应用程序中的数据均会被排序，而不管逻辑上是否需要。

==默认排序是按照字典顺序排序==，且实现该排序的方法是快速排序。

**对于MapTask**，它会将处理的结果暂时放到环形缓冲区中，当环形缓冲区使用率达到一定阈值后，再对缓冲区中的数据进行一次快速排序，并将这些有序数据溢写到磁盘上，而当数据处理完毕后，它会对磁盘上所有文件进行归并排序。

**对于ReduceTask**，它从每个MapTask上远程拷贝相应的数据文件，如果文件大小超过一定阈值，则溢写磁盘上，否则存储在内存中。如果内存中文件大小或者数目超过一定阈值，则进行一次合并后将数据溢写到磁盘上。如果磁盘上文件数目达到一定阈值，则进行一次归并排序以生成一个更大文件；当所有数据拷贝完毕后，ReduceTask统一对内存和磁盘上的所有数据进行一次归并排序。

==自定义排序需要实现Comparable接口或者使用TreeMap（内部创建new Comparator（））==

具体可参考[[#4.1.4 排序：Comparable]]
#### 2.3.3 Combiner合并概念

（1）Combiner是MR程序中Mapper和Reducer之外的一种组件

（2） Combiner组件的父类就是Reducer

（3）Combiner和Reducer的区别在于运行的位置。Combiner是在每一个MapTask所在的节点运行

（4） **Combiner的意义就是对每一个MapTask的输出进行局部汇总，以减小网络传输量**

（5） Combiner能够应用的前提是不能影响最终的业务逻辑，而且，**Combiner的输出kv应该跟Reducer的输入一致**

具体参见[[#4.3 编程案例——Combiner合并]]
![[Combiner.excalidraw|400]]

![[Combiner前后.excalidraw|600]]
#### 2.3.4 ReduceTask并行度决定机制

ReduceTask 数量的决定是可以直接手动设置

```java
// 默认值是 1，手动设置为 4
job.setNumReduceTasks(4);
```
## 三、MapReduce快速上手

### 3.1 快速理解yarn

yarn理解参见[[Hadoop3.x学习笔记#^eebb41]]

yarn默认的逻辑内存和核数分别是8G和8核。可以根据自己的机器重新设置。如果集群的资源比较差，也可以不修改默认的配置，因为运行一份mapreduce程序，比如一个map task，不一定用到1G。设置里面的1G的意思是默认最大用到1G内存。

默认配置yarn-default.xml

```txt
yarn.scheduler.minimum-allocation-mb  默认值：1024  // yarn分配一个容器时最低内存
yarn.scheduler.maximum-allocation-mb  默认值：8192  // yarn分配一个容器时最大内存
yarn.scheduler.minimum-allocation-vcores  默认值：1  // yarn分配一个容器时最少cpu核数
yarn.scheduler.maximum-allocation-vcores  默认值：32 // yarn分配一个容器时最多cpu核数
// 1个nodemanager拥有的总内存资源（逻辑的）
yarn.nodemanager.resource.memory-mb  默认值：8192  
// 1个nodemanager拥有的总cpu资源（逻辑的）
yarn.nodemanager.resource.cpu-vcores   默认值：8
```

最大内存和核数只是逻辑上的，并非实际的

```shell
# 查看内存
free -h

# 查看核数
cat /proc/cpuinfo |grep "cpu cores"|uniq
```

执行一个map task 或者reduce task 默认需要1核和1G内存

默认配置mapred-default.xml

```txt
// ApplicationMaster（AM）：单个任务运行的老大所需内存和核数
yarn.app.mapreduce.am.resource.mb 默认值：1536
yarn.app.mapreduce.am.resource.cpu-vcores 默认值：1

//map task所需内存和核数
mapreduce.map.memory.mb 默认值：1024
mapreduce.map.cpu.vcores 默认值：1

// reduce task所需内存和核数
mapreduce.reduce.memory.mb 默认值：1024
mapreduce.reduce.cpu.vcores 默认值：1
```
### 3.2 wordcount示例开发

#### 3.2.1 需求和思路

==需求==：统计单词总数

```txt
1.输入数据
hello hadoop
hello java

2.输出数据
hello 2
hadoop 1
java 1
```

**思路**：
1. map阶段： 将每一行文本数据变成<单词,1>这样的kv数据
2. reduce阶段：将相同单词的一组kv数据进行聚合：累加所有的v

**注意点**：mapreduce程序中
1. map阶段的进、出数据，
2. reduce阶段的进、出数据，
3. 类型都应该是实现了HADOOP序列化框架的类型，如：
	- String对应Text
	- Integer对应IntWritable
	- Long对应LongWritable
#### 3.2.2 编码实现

- WordcountMapper类开发
- WordcountReducer类开发
- JobSubmitter客户端类开发

三种方式运行（不管哪种方式，都需要在执行代码的地方有hadoop的客户端）：
1. ==在windows系统上执行（连接hadoop集群，以yarn的方式执行）==（com.bigdata.mr.wordCount.JobSubmitterWindowsToYarn）
2. ==在linux系统上执行（连接hadoop集群，以yarn的方式执行）==（com.bigdata.mr.wordCount.JobSubmitterLinuxToYarn）
3. ==在windows系统上执行（以本地方式执行）==
  （com.bigdata.mr.wordCount.JobSubmitterWindowsLocal）

为了方便调试，建议采用《**在windows系统上执行（以本地方式执行）**》的方式写代码，调试没有问题后在打包到集群上（无需做过多修改）

==详细代码==：[[wordcount单词统计开发]]
## 四、 MapReduce开发总结

### 4.1 map阶段

#### 4.1.1 读数据：InputFormat(输入数据接口)

（1）默认使用的实现类是==TextInputFormat==（功能逻辑是：一次读一行文本，key：这行起始字节偏移量（LongWritable类型）；value：这行的内容（Text类型））。代码：com.bigdata.mr.wordCount.JobSubmitterWindowsLocal

（2）==SequenceFileInputFormat==：读Sequence文件（key：存储的key的数据；value：存储的value的数据）。代码：com.bigdata.mr.index.sequence.IndexStepTwoSeqMain

（3）==CombineTextInputFormat==：用于小文件过多的场景，它可以将多个小文件从逻辑上规划到一个切片中，这样，多个小文件就可以交给一个 MapTask 处理。（key：这行起始字节偏移量（LongWritable类型）；value：这行的内容（Text类型））。代码：com.bigdata.mr.wordCount.WordCountCombineTextMain

（4）DBInputFormat：读数据库

（5）KeyValueTextInputFormat：（key：这行的内容（Text类型）；value：空（Text类型））。代码：com.bigdata.mr.wordCount.WordCountKeyVlaueTextMain

（7）NLineInputFormat：按行切片（key：这行的内容（Text类型）；value：空（Text类型））。代码：com.bigdata.mr.wordCount.WordCountNLineMain

参见[[#5.6 编程案例——控制输入、输出格式]]

```java
// 设置输入格式 ，输入文件格式为Sequence。默认为TextInputFormat
job.setInputFormatClass(SequenceFileInputFormat.class);

// -----可以把多个小文件合并成一个切片处理，提高处理效率。----
// 如果不设置InputFormat，它默认用的是TextInputFormat.class
job.setInputFormatClass(CombineTextInputFormat.class);
// 虚拟存储切片最大值设置4m
CombineTextInputFormat.setMaxInputSplitSize(job, 4194304);
```

```java
// 根据文件类型获取切片信息
FileSplit inputSplit = (FileSplit) context.getInputSplit();
// 获取切片的文件名称
String fileName = inputSplit.getPath().getName();
```
#### 4.1.2 处理数据：Mapper(逻辑处理接口)

用户根据业务需求重写其中三个方法：setup() map()  cleanup ()

setup() 在执行map()前执行一次，可以做一些初始化的工作。如[[#5.4 编程案例——倒排索引创建]]、[[#6.1 编程案例——join算法]]中的mapjoin

==map()方法实现对数据的处理==

cleanup()在map()后执行一次

```java
job.setMapperClass(WordcountMapper.class);
```
#### 4.1.3 分区：Partitioner

将map阶段产生的key-value数据，分发给若干个reduce task来分担负载，maptask调用Partition类的getPartition()方法来决定如何划分数据给不同的reduce task

有默认实现 HashPartitioner，逻辑是根据 key 的哈希值和 numReduces取模来返回一个分区号；(key.hashCode() & Integer.MAX_VALUE) % numReduceTasks

可以自定义分区，参见[[#5.1 编程案例——自定义类型和分区]]

```java
// 设置参数：maptask在做数据分区时，用哪个分区逻辑类  （如果不指定，它会用默认的HashPartitioner）
 job.setPartitionerClass(ProvincePartitioner.class);
```
#### 4.1.4 排序：Comparable 

对key-value数据做排序：调用key.compareTo()方法来实现对key-value数据排序

当我们需要对自定义的对象的进行排序时，就必须要实现 Comparable 接口，重写其中的 compareTo()方法。所有的数据类型默认是按照字典顺序排序的。

自定义排序参见[[#5.2 编程案例——求TopN和全局排序]]

```java
public class PageBean implements Comparable<PageBean> {
	@Override  
	public int compareTo(PageCount o) {  
		// 按访问次数降序排序，相同访问次数按网页升序排序  
		if (o.count - this.count == 0) {  
			return this.page.compareTo(o.page);  
		}  
		return o.count - this.count;  
	}
}
```
#### 4.1.5 合并：Combiner

Combiner 合并可以提高程序执行效率，减少 IO 传输。但是使用时必须不能影响原有的业务处理结果。

参见[[#5.3 编程案例——Combiner合并]]

```java
// 设置maptask端的局部聚合逻辑类
job.setCombinerClass(WordcountReducer.class);
```
### 4.2 reduce阶段

#### 4.2.1 读数据

通过http方式从maptask产生的数据文件中下载属于自己的“区”的数据到本地磁盘，然后将多个“同区文件”做合并（归并排序）
#### 4.2.2 分组：GroupingComparator

通过调用GroupingComparator的compare()方法来判断文件中的哪些key-value属于同一组，然后将这一组数据传给Reduce类的reduce()方法聚合一次

参见[[#5.5 编程案例——自定义GroupingComparator]]

```java
job.setGroupingComparatorClass(OrderIdGroupingComparator.class);
```

#### 4.2.3 处理数据 ：Reducer(逻辑处理接口)

用户根据业务需求实现其中三个方法：setup() reduce()  cleanup ()

setup() 在执行reduce()前执行一次，可以做一些初始化的工作。

==reduce()方法实现对数据的处理==

cleanup()在reduce()后执行一次

参见[[#5.2 编程案例——求TopN和全局排序]]

```java
job.setReducerClass(WordcountReducer.class);
```
#### 4.2.4 输出结果：OutputFormat(输出数据接口)

（1）默认实现类是TextOutputFormat（功能逻辑是：将每一个 KV 对，向目标文本文件输出一行）

（2）SequenceFileOutputFormat 写Sequence文件（直接将key-value对象序列化到文件中）

（3）DBOutputFormat 写数据库

参见[[#5.6 编程案例——控制输入、输出格式]]

```java
// 设置输出格式，输出文件格式为Sequence。默认为TextInputFormat
job.setOutputFormatClass(SequenceFileOutputFormat.class);
```
## 五、MapReduce编程案例（一）

### 5.1 编程案例——自定义类型和分区

#### 5.1.1 序列化概述

[[序列化概述]]
#### 5.1.2 需求和思路

==需求一==：统计每一个手机号耗费的总上行流量、总下行流量、总流量

```txt
1. 输入数据
id   手机号码   ... 上行流量 下行流量 网络状态码
1	18320173382	...	9531	2412	200

2. 输出数据
手机号码  上行流量 下行流量 总流量
18320173382 9531  2412    11943
```

**思路**：
1. map阶段：输出<手机号码，自定义bean对象>（bean对象包含手机号，上下行流量和总流量信息）
2. reduce阶段：输出<自定义bean对象，null>

**代码**：com.bigdata.mr.flow.FlowSumMain

==需求二==：在需求一的基础上，按照手机归属地，将统计结果输出在不同的文件中

**思路**：
- 默认分区是根据key的hashCode对ReduceTasks个数取模得到的
- 想办法让map端再将数据分区时，按照我们需要的按归属地划分

**代码**：com.bigdata.mr.flow.FlowSumPartitionsMain
### 5.2 编程案例——求TopN和全局排序

==需求一==：求出每个网站被访问次数

```txt
1.输入数据
2017/07/28 qq.com/a
2017/07/28 qq.com/bx
2017/07/28 qq.com/a

2.输出数据
qq.com/a 2
qq.com/bx 1
```

**思路**：就是一个wordcount

**代码**：com.bigdata.mr.page.PageCountMain

==需求二==：求出每个网站被访问次数最多的top3个url

**思路**：
1. map阶段：输出<url,1>
2. reduce阶段：把<url对象,null>放入hashmap中，最后从hashmap中取次数最多的前三个
	1. 只能有一个reduce task
	2. reduce task在处理完自己的所有数据后，调用一次cleanup方法，取前三个在cleanup中进行
	3. 对key排序的方法
		1. 对TreeMap可以对key进行排序（需要重写Comparator类中的compare方法）（com.bigdata.mr.page.PageTopThreeMain）
		2. 在自定义类型中实现Comparable类，重写compareTo方法（基于需求一的结果，map阶段输出<url对象，null>，reduce方法取前三个key写入文件）

**代码**：com.bigdata.mr.page.PageTop3Main

==需求三==：求出每个网站被访问次数最多的topn个url

**思路**：
1. 同需求二
2. topN怎么传参（com.bigdata.mr.page.PageTopNTest）
	1. 通过代码设置参数：conf.setInt("top.n", 3); 或者 conf.setInt("top.n", Integer.parseInt(args[0]));
	2. 通过在core-site.xml 配置文件设置
	3. 加载某个配置文件，如conf.addResource("xx-oo.xml");

```xml
<configuration>  
    <property>  
        <name>topn</name>  
        <value>3</value>  
    </property>  
</configuration>
```

**代码**：com.bigdata.mr.page.PageTopNMain

==需求四==：按访问次数降序排序

**思路**：
1. map阶段输出<url对象，null>
2. reduce阶段输出<url对象，null>
3. 基于需求一的结果，url类实现WritableComparable接口

**代码**：com.bigdata.mr.page.PageCountSortMain
	
### 5.3 编程案例——Combiner合并

==需求==：单词统计

**代码**：com.bigdata.mr.wordCount.JobSubmitterWindowsLocal

只需要在客户端类中加入这么一行。CombinerClass类输入是map的输出，输出是reduced的输入

```java
// 设置maptask端的局部聚合逻辑类
job.setCombinerClass(WordcountReducer.class);
```
### 5.4 编程案例——倒排索引创建

==需求==：统计单词在每个文件出现的次数

```txt
1.输入数据
# a.txt
hello tom
hello java

# b.txt
hello jerry

2.输出数据
hello  a.txt-->2 b.txt-->1
```

**思路**：
1. 第一个mapreduce程序：统计出每个单词在每个文件中的总次数
	1. map阶段：输出<单词-文件名，1>
	2. reduce阶段：统计单词-文件名中的次数，输出<单词,文件名-->次数>
2. 第二个mapreduce程序
	1. map阶段：输出<单词，文件名-->次数>
	2. reduce阶段：输出<单词，文件名-->次数拼接>

```java
// 从输入切片信息中获取当前正在处理的一行数据所属的文件
FileSplit inputSplit = (FileSplit) context.getInputSplit();
String fileName = inputSplit.getPath().getName();
```

```txt
worker在正式处理数据之前，会先调用一次setup方法，所以，常利用这个机制来做一些初始化操作
```

**代码**：
1. 第一个mapreduce程序：com.bigdata.mr.index.IndexStepOneMain
2. 第二个mapreduce程序：com.bigdata.mr.index.IndexStepTwoMain

### 5.5 编程案例——自定义GroupingComparator

==需求==：展示每个订单商品销售额前三的订单详情

```txt
1.输入数据
order001,u001,小米6,1999.9,2
order001,u001,雀巢咖啡,99.0,2
order001,u001,安慕希,250.0,2
order001,u001,经典红双喜,200.0,4
order001,u001,防水电脑包,400.0,2

2.输出数据
order001,u001,小米6,1999.9,2
order001,u001,防水电脑包,400.0,2
order001,u001,安慕希,250.0,2
```

**思路**：
1. map阶段：输出<bean对象,null>
	1. 自定义分区，按订单id的hash取模
2. reduce阶段：输出<bean对象,null>
	1. 自定义GroupingComparator，把相同订单id分为一组

```java
// 自定义分区和GroupingComparator  
job.setPartitionerClass(OrderIdPartitioner.class);  
job.setGroupingComparatorClass(OrderIdGroupingComparator.class);
```

**代码**：com.bigdata.mr.order.grouping.OrderTopnMain
### 5.6 编程案例——控制输入、输出格式

==需求== ：把[[#5.4 编程案例——倒排索引创建]] 中第一个mapreduce程序输出为Sequence文件

```java
// job.setInputFormatClass(TextInputFormat.class); 默认的输入组件
job.setInputFormatClass(SequenceFileInputFormat.class);

// job.setOutputFormatClass(TextOutputFormat.class);  // 这是默认的输出组件
job.setOutputFormatClass(SequenceFileOutputFormat.class);
```

**代码**：
com.bigdata.mr.index.sequence.IndexStepOneSeqMain
com.bigdata.mr.index.sequence.IndexStepTwoSeqMain
## 六、MapReduce编程案例（二）

### 6.1 编程案例——join算法

==需求==：订单数据关联用户数据。类似的sql：`select a.*,b.* from a join b on a.uid=b.uid;`

```txt
1.输入数据
# 订单数据
order001,u001
order002,u001
order003,u005

#用户数据
u001,senge,18,angelababy
u002,laozhao,48,ruhua
u003,xiaoxu,16,chunge

2.输出数据
order001,u001,senge,18,angelababy
order002,u001,laozhao,48,ruhua
order003,u003,xiaoxu,16,chunge
```

**思路1**：这是很low的实现方式（com.bigdata.mr.join.OrderJoinUserMain）
1. map阶段：输出<用户id,bean对象>
	1. bean类用一个字段标识这个对象来自哪个表
2. reduce阶段：输出<bean对象,null>
	1. 遍历一组key时，如果是订单表对象，把对象放在订单对象集合中，如果是用户数据，把对象放在用户对象中，然后复制对象属性到订单对象数组中。（集合可能会很大，不停地new 对象，也会增加资源地消耗）
	
**思路二**：比较高效的方式（com.bigdata.mr.join.grouping.UserGroupingMain）
1. map阶段：输出<bean对象，null>
	1. 自定义分区：按userid的hash取模分区
	2. 类实现比较类，相同userid，按标识表的字段排序，user排第一
2. reduce阶段：输出<bean对象,null>
	1. 自定义groupingComparator，把相同userid分为一组
	2. 遍历values值，标识表的字段为user表放在user对象中，其他边遍历边写
	
**思路三**：比较高效的方式（适用于一张表十分小、一张表很大的场景。）（com.bigdata.mr.join.mapjoin.MapJoinMain）
1. map阶段：输出<bean对象,null>
	1. 在setup 阶段，将小文件读取到缓存集合中
2. reduce阶段：无

```java
//缓存普通文件到 Task 运行节点。
job.addCacheFile(new URI("file:///e:/cache/user.txt"));

//如果是集群运行,需要设置 HDFS 路径
job.addCacheFile(new URI("hdfs://hadoop102:8020/cache/user.txt"));
```

### 6.2 编程案例——数据倾斜场景

==需求==：单词统计，如果某个单词量特别多，比如hello，负责处理hello这个单词数据的reduce task就会很累（负载不均衡，过大）。如何处理？会让整个数据处理过程中，数据倾斜的状况得到缓解

**思路**：
1. 第一个mapreduce程序（在map端统计单词次数，然后再单词附加一个随机数，打乱分散在reduce端）
	1. map阶段：输出<单词-随机数,1>
		- 进行combine
	2. reduce阶段：输出<单词-随机数,次数> 
1. 第二个mapreduce程序（统计单词出现次数）
	1. map阶段：输出<单词,次数>
		- 进行combine
	2. reduce阶段：输出<单词,次数>

**代码**：
com.bigdata.mr.wordCount.skew.SkewWordcount1Main
com.bigdata.mr.wordCount.skew.SkewWordcount2Main

### 6.3 编程案例——共同好友

==需求==：求共同好友

```txt
1.输入数据
用户id:好友
A:B,C,D,F,E,O
B:A,C,E,K

2.输出数据
A-B C,D
```

**思路**：
1. 第一个mapreduce程序：
	1. map阶段：输出<好友,用户>
	2. reduce阶段：输出<"用户-用户",共同好友>
		1. <好友，用户迭代器>，遍历用户迭代器。放入集合中。然后排序。
		2. 遍历集合，输出<"用户-用户",共同好友>
2. 第二个mapreduce程序：
	1. map阶段：输出<"用户-用户",共同好友>
	2. reduce阶段：输出<"用户-用户",“共同好友,共同好友....”>

**代码**：
com.bigdata.mr.friends.CommonFriends1Main
com.bigdata.mr.friends.CommonFriends2Main

## 七、Hadoop数据压缩

### 7.1 概述

1. 压缩的好处和坏处
	- 压缩的优点：以减少磁盘 IO、减少磁盘存储空间。
	- 压缩的缺点：增加 CPU 开销。
2. 压缩原则
	- ==运算密集型的 Job，少用压缩==
	- ==IO 密集型的 Job，多用压缩==

### 7.2 MR支持的压缩编码

压缩算法和压缩性能对比介绍：

| 压缩格式 | 自带？ | 算法    | 文件拓展名 | 可切片？ | 原程序需改？ | 原始文件 | 压缩后文件 | 压缩速度 | 解压速度 |
| -------- | ------ | ------- | ---------- | -------- | ------------ | -------- | ---------- | -------- | -------- |
| deflate  | 是     | deflate | .deflate   | 否       | 否     |          |            |          |          |
| Gzip     | 是     | deflate | .gz        | 否       | 否       | 8.3GB    | 1.8GB      | 17.5MB/s | 58MB/s   |
| bzip2    | 是     | bzip2   | .bz2       | 是       | 否       | 8.3GB    | 1.1GB      | 2.4MB/s  | 9.MB/s   |
| LZO      | 否     | LZO     | .lzo       | 是       | 是         | 8.3GB    | 2.9GB      | 49.5MB/s | 74.6MB/s |
| Snappy   | 是     | Snappy  | .snappy    | 否       | 否       |          |            |          |          |

[snappy | A fast compressor/decompressor (google.github.io)](http://google.github.io/snappy/)

```txt
Snappy is a compression/decompression library. It does not aim for maximum compression, or compatibility with any other compression library; instead, it aims for very high speeds and reasonable compression. For instance, compared to the fastest mode of zlib, Snappy is an order of magnitude faster for most inputs, but the resulting compressed files are anywhere from 20% to 100% bigger. On a single core of a Core i7 processor in 64-bit mode, Snappy compresses at about 250 MB/sec or more and decompresses at about 500 MB/sec or more.

Snappy：它不以最大压缩为目标，它的目标是非常高的速度和合理的压缩。例如，Snappy比zlib 更快，但文件相对要大 20% 到 100%。在64位模式的酷睿i7处理器的单核上，Snappy的压缩速度约为250 MB/秒或更高，而解压缩速度约为500 MB/秒或更高
```
### 7.3 压缩方式选择

![[mapreduce数据压缩.excalidraw|600]]

| 压缩格式 | 对应的编码/解码器                          |
| -------- | ------------------------------------------ |
| deflate  | org.apache.hadoop.io.compress.DefaultCodec |
| gizp     | org.apache.hadoop.io.compress.GzipCodec    |
| bzip2    | org.apache.hadoop.io.compress.BZip2Codec   |
| LZO      | com.hadoop.compression.lzo.LzopCodec       |
| Snappy   | org.apache.hadoop.io.compress.SnappyCodec                                           |

要在 Hadoop 中启用压缩，可以配置如下参数

core-default.xml

```xml
<!--输入压缩,Hadoop 使用文件扩展名判断是否支持某种编解码器。默认空，无须修改-->
<property>
<name>io.compression.codecs</name>
<value></value>
</property>
```

mapred-dufault.xml

```xml
<!--mapper输出。这个参数设为true启用压缩-->
<property>
<name>mapreduce.map.output.compress</name>
<value>false</value>
</property>

<!--mapper输出。企业多使用 LZO 或Snappy 编解码器在此阶段压缩数据-->
<property>
<name>mapreduce.map.output.compress.codec</name>
<value>org.apache.hadoop.io.compress.DefaultCodec</value>
</property>

<!--reducer输出。这个参数设为true启用压缩-->
<property>
<name>mapreduce.output.fileoutputformat.compress</name>
<value>false</value>
</property>

<!--reducer输出。使用标准工具或者编解码器，如 gzip 和bzip2-->
<property>
<name>mapreduce.output.fileoutputformat.compress.codec</name>
<value>org.apache.hadoop.io.compress.DefaultCodec</value>
</property>
```

### 7.4 压缩实操案例

即使你的 MapReduce 的输入输出文件都是未压缩的文件，你仍然可以对 Map 任务的中间结果输出做压缩，因为它要写在硬盘并且通过网络传输到 Reduce 节点，对其压缩可以提高很多性能，这些工作只要设置两个属性即可，我们来看下代码怎么设置。

**代码**：com.bigdata.mr.wordCount.WordcountCompressionMain

```java
//-------------------------开启map端输出压缩-------------------------  
conf.setBoolean("mapreduce.map.output.compress", true);  
conf.setClass("mapreduce.map.output.compress.codec", SnappyCodec.class, CompressionCodec.class);  
  
//-------------------------开启reduce端输出压缩-------------------------  
FileOutputFormat.setCompressOutput(job, true);  
FileOutputFormat.setOutputCompressorClass(job, BZip2Codec.class);
```