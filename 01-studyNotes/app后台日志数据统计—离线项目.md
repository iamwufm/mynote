
## 一、启动集群


## 二、日志上传到hdfs

每天凌晨一点把昨天数据上传到hdfs昨天目录，如20170815号凌晨一点上传昨天20170814的日志文件到hdfs的
/app-log-data/data/20170814


```shell
# 启动hdfs集群（在hadoop101上执行）
start-dfs.sh

# 在hadoop103上执行
cd /opt/module/flume-1.11.0/job

vim log-data-to-hdfs.conf

# 添加如下内容
```

```conf
#定义三大组件的名称
ag1.sources = source1
ag1.sinks = sink1
ag1.channels = channel1

# 配置source组件
ag1.sources.source1.type = spooldir
ag1.sources.source1.spoolDir = /home/hadoop/app-log-data/
ag1.sources.source1.fileSuffix=.FINISHED
# 行的最大长度，如果超过该值会进行截取5k
ag1.sources.source1.deserializer.maxLineLength=5120

# 配置sink组件
ag1.sinks.sink1.type = hdfs
ag1.sinks.sink1.hdfs.path =hdfs://hadoop101:8020/app-log-data/data/%Y%m%d/
ag1.sinks.sink1.hdfs.filePrefix = app_log
ag1.sinks.sink1.hdfs.fileSuffix = .log
# #积攒多少个 Event 才 flush 到 HDFS 一次
ag1.sinks.sink1.hdfs.batchSize= 100
ag1.sinks.sink1.hdfs.fileType = DataStream
ag1.sinks.sink1.hdfs.writeFormat =Text

## roll：滚动切换：控制写文件的切换规则
# 按文件体积（字节）来切 ,128M
ag1.sinks.sink1.hdfs.rollSize = 134217728
# 按event条数切
ag1.sinks.sink1.hdfs.rollCount = 0
# 按时间间隔切换文件,10分钟
ag1.sinks.sink1.hdfs.rollInterval = 600

## 控制生成目录的规则,24小时生成一次目录
ag1.sinks.sink1.hdfs.round = true
ag1.sinks.sink1.hdfs.roundValue = 24
ag1.sinks.sink1.hdfs.roundUnit = hour

ag1.sinks.sink1.hdfs.useLocalTimeStamp = true

# channel组件配置
ag1.channels.channel1.type = memory
# event条数
ag1.channels.channel1.capacity = 1000
# flume事务控制所需要的缓存容量100条event
ag1.channels.channel1.transactionCapacity = 100

# 绑定source、channel和sink之间的连接
ag1.sources.source1.channels = channel1
ag1.sinks.sink1.channel = channel1
```

```shell
cd ..
# 开始采集
bin/flume-ng agent --conf conf -conf-file job/log-data-to-hdfs.conf  -n ag1  -Dflume.hadoop.logger=INFO,console

# 往/home/hadoop/app-log-data/20231016目录丢文件

```

![[Pasted image 20231016173112.png]]


![[Pasted image 20231016180011.png]]

```shell
# 当前目录是20231016
hdfs dfs -mv /app-log-data/data/20231016 /app-log-data/data/20170814 
```



## 三、数据预处理

一行数据的样子：

```txt
{"events":"1473367236143\\n","header":{"cid_sn":"1501004207EE98AA","mobile_data_type":"","os_ver":"22","mac":"1c:77:f6:78:f5:75","resolution":"1080x1920","commit_time":"1502686418952","sdk_ver":"103","device_id_type":"mac","city":"江门市","device_model":"HUAWEI VNS-AL00","android_id":"867830021735040","carrier":"中国xx","promotion_channel":"1","app_ver_name":"1.4","imei":"867830021735040","app_ver_code":"4010104","pid":"pid","net_type":"3","device_id":"m.1c:77:f6:78:f5:75","app_device_id":"m.1c:77:f6:78:f5:75","release_channel":"1009","country":"CN","time_zone":"28800000","os_name":"android","manufacture":"OPPO","commit_id":"fde7ee2e48494b24bf3599771d7c2a78","account":"none","app_token":"XIAONIU_A","app_id":"com.appid.xiaoniu","language":"zh","build_num":"YVF6R16303000403"}}
```

==要求如下：==

（1）必选字段不能为空（null或去掉空格后是空字符串）。必须字段见[[#4.1 建外部表映射预处理数据]] net_type 字段（含）以上都是必选字段

（2）为每条日志添加一个用户唯一标识字段：user_id

规则：如果是os_name="android" and android_id不为空， user_id = android_id，否则为 user_id = device_id

（3）将event字段抛弃，将header中的各字段解析成普通文本行

（4）需要将清洗后的结果数据，分ios和android两种类别，输出到两个不同的文件夹

==代码实现==：com.bigdata.log.mr.AppLogDataCleanMain

==运行==：

```shell
# hadoop jar 包名.jar  参数1 参数2(在pom文件指定了主类。可以不用写类名))
hadoop jar appLogDataClean-jar-with-dependencies.jar /app-log-data/data/20170814 /app-log-data/clean/20170814
```

![[Pasted image 20231016183148.png]]

![[Pasted image 20231016183200.png]]

![[Pasted image 20231016183216.png]]

可以编写预处理启动脚本
```shell
vim 预处理启动脚本.bash
```

```shell
#!/bin/bash
day_str=`date +'%Y%m%d'`

inpath=/app-log-data/data/$day_str
outpath=/app-log-data/clean/$day_str

echo "准备清洗$day_str 的数据......"
hadoop jar appLogDataClean-jar-with-dependencies.jar $inpath $inpath $outpath
```
## 四、数据入库

1. 在hive中建表ods_app_log（外部表、分区表）映射预处理阶段生成的清洗数据
2.  每天定时将预处理之后的天数据 导入到 ods_app_log的天分区中

见[[Hive3.x安装#4.1 交互式命令]]

```shell
# hadoop102上执行
hive
```
### 4.1 建外部表映射预处理数据

SecureCRT打不出来指标符

```sql
drop table if exists ods_app_log;

create external table ods_app_log (
 user_id                        string          comment '用户id，如果os_name是android且android_id不为空，就取android_id，否则取device_id'
,sdk_ver                        string          comment '采集SDK版本号'
,time_zone                      string          comment '所有时间字段都是UTC时间，需要使用时区转换为当地时间，8：东8区， -8：西8区'
,commit_id                      string          comment '标识每次提交的唯一性'
,commit_time                    string          comment '提交时间,提交本地采集数据到服务器的时间'
,pid                            string          comment 'process id, 程序一次运行的唯一id'
,app_token                      string          comment '应用在采集系统中的唯一标识，由服务器生成，同一个app_token可以包括多个app_id'
,app_id                         string          comment '应用的标识，android下是包名'
,device_id                      string          comment '唯一的设备标识'
,device_id_type                 string          comment 'enum ["imei", "soc", "mac", "uuid", "android_id", "build", "unknown"]'
,release_channel                string          comment '应用发行渠道（应用宝，豌豆荚，360手机助手，小米商城，appstore）'
,app_ver_name                   string          comment '应用的版本名称'
,app_ver_code                   string          comment '应用的版本号'
,os_name                        string          comment 'enum ["android", "ios", "windows", "winphone"]'
,os_ver                         string          comment '操作系统版本'
,language                       string          comment '语言zh，en, de,...'
,country                        string          comment 'CN，US, DE,...'
,manufacture                    string          comment '设备制造商(Lenovo)'
,device_model                   string          comment '设备型号'
,resolution                     string          comment '屏幕分辨率, width*height(480*800)'
,net_type                       string          comment 'enum [0-Unknown; 1-Offline; 2-Wifi; 3-MobileData]'
,account                        string          comment '用户在帐号系统注册的帐号，暂时不使用'
,app_device_id                  string          comment '是传输时使用的设备ID'
,mac                            string          comment '设备的mac地址，可以保证稳定唯一，但是，手机开机后如果一直不打开wifi，是不能获取mac地址的'
,android_id                     string          comment 'android id, 刷系统后，会导致android id改变'
,imei                           string          comment '设备的imei号，有些手机的imei号相同(中国市场的山寨机)'
,cid_sn                         string          comment '内置存储CID(customer identity)序列号'
,build_num                      string          comment '固件版本号'
,mobile_data_type               string          comment 'enum[0-Unknown; 1-2G; 2-3G; 3-4G]'
,promotion_channel              string          comment '统计三方应用在Google Play的推广'
,carrier                        string          comment '运营商'
,city                           string          comment '城市'
) comment 'ods层_app应用_日志表'
partitioned by (stat_dt_p string comment '统计日期分区',os_name_p string comment '操作系统名称分区')
row format delimited fields terminated by '\001' location '/app-log-data/clean';
```

### 4.2 将数据导入到分区表

```sql
alter table ods_app_log add partition (stat_dt_p='20170814',os_name_p='android') location '/app-log-data/clean/20170814/android';
alter table ods_app_log add partition (stat_dt_p='20170814',os_name_p='ios') location '/app-log-data/clean/20170814/ios';
```


## 五、数据分析

### 5.1 新增用户分析

新增用户的定义：

比如，在2017-08-28日出现了一些以前从没出现过的用户，则这些用户就是2017-08-28日的新增用户
   
==需求：==将每日的新增用户从ods_app_log表中抽取出来，存入一个新用户信息表：dw_new_user_day的日分区中

==实现：==

（1）建表

```sql
-- 历史用户表
drop table if exists etl_user_history;

create table etl_user_history (
 user_id                        string          comment '用户id'
)comment '用户历史表'
partitioned by (stat_dt_p string comment '统计日期分区')
;
```

```sql
-- 新用户信息表
drop table if exists etl_user_new_day;

create table etl_user_new_day (
 user_id                        string          comment '用户id，如果os_name是android且android_id不为空，就取android_id，否则取device_id'
,sdk_ver                        string          comment '采集SDK版本号'
,time_zone                      string          comment '所有时间字段都是UTC时间，需要使用时区转换为当地时间，8：东8区， -8：西8区'
,commit_id                      string          comment '标识每次提交的唯一性'
,commit_time                    string          comment '提交时间,提交本地采集数据到服务器的时间'
,pid                            string          comment 'process id, 程序一次运行的唯一id'
,app_token                      string          comment '应用在采集系统中的唯一标识，由服务器生成，同一个app_token可以包括多个app_id'
,app_id                         string          comment '应用的标识，android下是包名'
,device_id                      string          comment '唯一的设备标识'
,device_id_type                 string          comment 'enum ["imei", "soc", "mac", "uuid", "android_id", "build", "unknown"]'
,release_channel                string          comment '应用发行渠道（应用宝，豌豆荚，360手机助手，小米商城，appstore）'
,app_ver_name                   string          comment '应用的版本名称'
,app_ver_code                   string          comment '应用的版本号'
,os_name                        string          comment 'enum ["android", "ios", "windows", "winphone"]'
,os_ver                         string          comment '操作系统版本'
,language                       string          comment '语言zh，en, de,...'
,country                        string          comment 'CN，US, DE,...'
,manufacture                    string          comment '设备制造商(Lenovo)'
,device_model                   string          comment '设备型号'
,resolution                     string          comment '屏幕分辨率, width*height(480*800)'
,net_type                       string          comment 'enum [0-Unknown; 1-Offline; 2-Wifi; 3-MobileData]'
,account                        string          comment '用户在帐号系统注册的帐号，暂时不使用'
,app_device_id                  string          comment '是传输时使用的设备ID'
,mac                            string          comment '设备的mac地址，可以保证稳定唯一，但是，手机开机后如果一直不打开wifi，是不能获取mac地址的'
,android_id                     string          comment 'android id, 刷系统后，会导致android id改变'
,imei                           string          comment '设备的imei号，有些手机的imei号相同(中国市场的山寨机)'
,cid_sn                         string          comment '内置存储CID(customer identity)序列号'
,build_num                      string          comment '固件版本号'
,mobile_data_type               string          comment 'enum[0-Unknown; 1-2G; 2-3G; 3-4G]'
,promotion_channel              string          comment '统计三方应用在Google Play的推广'
,carrier                        string          comment '运营商'
,city                           string          comment '城市'
) comment '新用户信息表'
partitioned by (stat_dt_p string comment '统计日期分区')
;
```

（2）实现逻辑

将每日的新增用户从ods_app_log表中抽取出来，存入一个新用户信息表

```sql
insert overwrite table etl_user_new_day partition(stat_dt_p='20170814')
select
t1.user_id
,t1.sdk_ver
,t1.time_zone
,t1.commit_id
,t1.commit_time
,t1.pid
,t1.app_token
,t1.app_id
,t1.device_id
,t1.device_id_type
,t1.release_channel
,t1.app_ver_name
,t1.app_ver_code
,t1.os_name
,t1.os_ver
,t1.language
,t1.country
,t1.manufacture
,t1.device_model
,t1.resolution
,t1.net_type
,t1.account
,t1.app_device_id
,t1.mac
,t1.android_id
,t1.imei
,t1.cid_sn
,t1.build_num
,t1.mobile_data_type
,t1.promotion_channel
,t1.carrier
,t1.city
from ods_app_log t1 -- 日志文件表
left join etl_user_history t2 -- 昨日的历史用户表
on t1.user_id = t2.user_id and t2.stat_dt_p<='20170813' 
where t1.stat_dt_p='20170814' and t2.user_id is null
;
```

将当日新增用户的user_id追加到历史表

```sql
insert overwrite table etl_user_history partition(stat_dt_p='20170814')
select
t1.user_id
from etl_user_new_day t1 -- 新用户信息表
where t1.stat_dt_p='20170814'
;
```
### 5.2 活跃用户（日活）

概念：某一天使用过app的用户就是活跃用户

取user_id去重值即可

==实现：==

（1）建表

```sql
drop table if exists etl_user_active_day;
-- 为了方便，直接copy新增用户表的建表语句
create table etl_user_active_day like etl_user_new_day;
```

（2）实现逻辑

将user_id去重即可

```sql
insert overwrite table etl_user_active_day partition(stat_dt_p='20170814')
select
t2.user_id
,t2.sdk_ver
,t2.time_zone
,t2.commit_id
,t2.commit_time
,t2.pid
,t2.app_token
,t2.app_id
,t2.device_id
,t2.device_id_type
,t2.release_channel
,t2.app_ver_name
,t2.app_ver_code
,t2.os_name
,t2.os_ver
,t2.language
,t2.country
,t2.manufacture
,t2.device_model
,t2.resolution
,t2.net_type
,t2.account
,t2.app_device_id
,t2.mac
,t2.android_id
,t2.imei
,t2.cid_sn
,t2.build_num
,t2.mobile_data_type
,t2.promotion_channel
,t2.carrier
,t2.city
from
(select
row_number() over(partition by t1.user_id order by t1.commit_time desc) as rn
,t1.user_id
,t1.sdk_ver
,t1.time_zone
,t1.commit_id
,t1.commit_time
,t1.pid
,t1.app_token
,t1.app_id
,t1.device_id
,t1.device_id_type
,t1.release_channel
,t1.app_ver_name
,t1.app_ver_code
,t1.os_name
,t1.os_ver
,t1.language
,t1.country
,t1.manufacture
,t1.device_model
,t1.resolution
,t1.net_type
,t1.account
,t1.app_device_id
,t1.mac
,t1.android_id
,t1.imei
,t1.cid_sn
,t1.build_num
,t1.mobile_data_type
,t1.promotion_channel
,t1.carrier
,t1.city
from ods_app_log t1 -- 日志文件表
where t1.stat_dt_p='20170814'
)t2
where t2.rn = 1
;
```

### 5.3 留存用户统计

概念：

比如，15号的新增用户，在16号又活跃了，这些用户就是次日留存用户;

比如，12号的新增用户，在15号又活跃了，这些用户就是3日留存用户;

==需求==：做次日留存的分析

==实现==：

（1）建表

建次日留存etl信息表

```sql
drop table if exists etl_user_keepalive_nextday;
-- 为了方便，直接copy新增用户表的建表语句
create table etl_user_keepalive_nextday like etl_user_new_day;
```

（2）实现逻辑

```sql
insert overwrite table etl_user_keepalive_nextday partition(stat_dt_p='20170814')
select
 t1.user_id
,t1.sdk_ver
,t1.time_zone
,t1.commit_id
,t1.commit_time
,t1.pid
,t1.app_token
,t1.app_id
,t1.device_id
,t1.device_id_type
,t1.release_channel
,t1.app_ver_name
,t1.app_ver_code
,t1.os_name
,t1.os_ver
,t1.language
,t1.country
,t1.manufacture
,t1.device_model
,t1.resolution
,t1.net_type
,t1.account
,t1.app_device_id
,t1.mac
,t1.android_id
,t1.imei
,t1.cid_sn
,t1.build_num
,t1.mobile_data_type
,t1.promotion_channel
,t1.carrier
,t1.city
from etl_user_active_day t1 -- 活跃用户（日活）
join etl_user_new_day t2 -- 昨日新增用户表
on t1.user_id = t2.user_id and t2.stat_dt_p = '20170813'
where t1.stat_dt_p='20170814'
;
```

### 5.4 沉默用户分析

概念：一段时间内（连续7天）没有使用过app的用户

比如，现在运算的是14号的报表，用07号的新增用户  left join 活跃用户表的（08-14号分区），取右表join后结果为null的用户

实现：

（1）建表

```sql
-- 沉默用户表
drop table if exists etl_silent_user;

create table etl_silent_user (
 user_id                        string          comment '用户id'
)comment '沉默用户表'
partitioned by (stat_dt_p string comment '统计日期分区')
;
```


（2）实现逻辑

```sql
insert overwrite table etl_silent_user partition(stat_dt_p='20170814')
select t1.user_id
from etl_user_new_day t1 -- 取前第7天
left join etl_user_active_day t2 -- 取前7天的活跃用户表
on t1.user_id = t2.user_id and t2.stat_dt_p >= '20170808' and t2.stat_dt_p <= '20170814'
where t1.stat_dt_p = '20170807' and t2.user_id is null
;
```

