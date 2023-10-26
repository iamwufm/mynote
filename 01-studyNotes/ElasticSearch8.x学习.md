---
author: wufm
title: ElasticSearch8.x学习
rating: 4
time: 2023-10-24 周二
tags:
  - elasticSearch
  - Lucene
  - 搜索
  - 倒排索引
代码: bigdata项目的下的elasticsearch
---
## 一、ElasticSearch简介

### 1.1 ElasticSearch概述

官网：[使用 Elastic 解决方案推动重要结果的交付 | Elastic](https://www.elastic.co/cn/products/)

ElasticSearch是一个基于JSON的==实时分布式搜索和分析引擎==。它用于全文搜索、结构化搜索、分析。

数据结构就是==倒排索引==

![[Pasted image 20231025192912.png|500]]
### 1.2 ElasticSearch核心概念

- 索引Index：类似MySql中的表
- 类型Type(7.0版本Type名称固定且默认为_doc)：类似MySql数据库表Table
- 文档id：相当于数据库表中记录的主键，是唯一的。
- 文档Document(JSON格式)：类似MySql数据库数据行Row
- 字段Field：类似MySql数据库数据列Column
- 映射Mapping：类似MySql数据库约束 Schema
### 1.2 ElasticSearch特点

-  **天然分片**：ES把数据分成多个shard，下图中的P0-P2，多个shard可以组成一份完整的数据，这些shard可以分布在集群中的各个机器节点中。随着数据的不断增加，集群可以增加多个分片，把多个分片放到多个机子上，已达到负载均衡，横向扩展。

![[Pasted image 20231024153425.png|400]]

- **天然集群**：一台ES实例也可以组成一个集群，方便扩容。在扩容时，只需要在其他节点安装ES，直接启动，新节点会基于配置自动在网段中寻找ES集群，自动申请加入集群。
- **天然索引**：mysql和其他的数据库，需要手动创建索引。 ES在插入数据后自动创建索引。
-  **近实时**：从写入数据到数据可以被搜索到有一个小延迟（大概1秒）
## 二、ElasticSearch安装

见[[ElasticSearch8.x安装]]

## 三、数据类型

ES中的数据类型十分丰富，具体可参考：[Painless Scripting Language [8.10] | Elastic](https://www.elastic.co/guide/en/elasticsearch/painless/current/index.html)

这里仅仅列出核心数据类型：
- 字符串型：text(分词)、keyword(不分词)
- 数值型：long、integer、short、byte、double、float、half_float、scaled_float
- 日期类型：date
- 布尔类型：boolean
## 四、ElasticSearch快速入门

在kibana上执行可以省略不少信息。

比如在kibana上可以用（[Console - Dev Tools - Elastic](http://hadoop101:5601/app/dev_tools#/console)）

```shell
POST /index3/_doc/1
{
  "name":"jack",
  "age": 20
}
```

在服务器上用：

```shell
curl -XPOST "http://hadoop101:9200/index3/_doc/2" -H "Content-Type: application/json" -d '
{
  "name":"jack",
  "age": 20
}'
```

其中，-X选项指定HTTP请求的方法，-H选项指定HTTP请求的头部信息，-d选项指定HTTP请求的数据体。

![[Pasted image 20231025123848.png]]

```shell
#查看节点状况
GET /_cat/nodes?v

#查看健康状况
GET /_cat/health?v

#查看能查看什么
GET /_cat

#查看所有的index
GET /_cat/indices
```

以下操作均可以通过head插件查看是否成功：[elasticsearch-head](http://hadoop102:9100/)

###  4.1 index操作

#### 4.1.1 创建index

手动创建Index 。需要在创建index时指定mapping信息

```shell
curl -XPUT "http://hadoop101:9200/stu" -H "Content-Type: application/json" -d '
{
  "mappings": {
    "properties": {
      "id": {
        "type": "keyword"
      },
      "name": {
        "type": "text"
      },
      "hobby": {
        "properties": {
          "name": {
            "type": "text"
          },
          "years":{
            "type": "integer"
          }
        }
      }
    }
  }
}'
```

自动创建 。直接向一个不存在的Index插入数据，在插入数据时，系统根据数据的类型，自动推断mapping，自动创建mapping

```shell
curl -XPUT "http://hadoop101:9200/index3/_doc/1" -H "Content-Type: application/json" -d '
{
  "name":"jack",
  "age": 20
}'
```
#### 4.1.2 查看index

```shell
#查看所有的index
curl -XGET "http://hadoop101:9200/_cat/indices"

#查看某个index的信息
curl -XGET "http://hadoop101:9200/_cat/indices/index3"

#查看某个index的元数据信息
curl -XGET "http://hadoop101:9200/index3"

#查看某个index的表结构
curl -XGET "http://hadoop101:9200/index3/_mapping"

#查看某个index的别名
curl -XGET "http://hadoop101:9200/index3/_alias"
```
#### 4.1.3 修改index

目前仅仅支持向Index添加新的字段映射，如下:

```shell

curl -XPUT "http://hadoop101:9200/stu/_mapping" -H "Content-Type: application/json" -d '
{
  "properties" : {
    "sex":{
      "type":"keyword"
     }
   }
}'
```
#### 4.1.4 删除index

```shell
#删除index

DELETE /index1

curl -XDELETE "http://hadoop101:9200/stu" 
```

### 4.2 新增/修改操作

路径 `megacorp/_doc/1`包含了三部分的信息：
- megacorp：索引名称，相当于数据库中的表
- `_doc`：固定
- 1：文档id，相当于表中的主键

相同文档id会先删除再新增

明确指定id新增

```shell
curl -XPUT "http://hadoop101:9200/megacorp/_doc/1?pretty" -H 'Content-Type:application/json' -d'
{
"first_name":"John",
"last_name":"Smith",
"age":25,
"about":"I love to go rock climbing",
"interests":["sports","music1"]
}'

curl -XPUT "http://hadoop101:9200/megacorp/_doc/2?pretty" -H 'Content-Type:application/json' -d'
{
"first_name":"Jane",
"last_name":"Smith",
"age":32,
"about":"I like to collect rock albums",
"interests":["music1"]
}'
```

随机指定id

```shell
curl -XPOST "http://hadoop101:9200/megacorp/_doc?pretty" -H 'Content-Type:application/json' -d'
{
"first_name":"Douglas",
"last_name":"Fir",
"age":35,
"about":"I like to build cabinets",
"interests":["forestry"]
}'
```

批量导入（导入library表，`_bluk`固定写法）

```shell
curl -XPUT "http://hadoop101:9200/library/_bulk?refresh&pretty" -H 'Content-Type: application/json' -d'
{"index":{"_id": "Leviathan Wakes"}}
{"name": "Leviathan Wakes", "author": "James S.A. Corey", "release_date": "2011-06-02", "page_count": 561}
{"index":{"_id": "Hyperion"}}
{"name": "Hyperion", "author": "Dan Simmons", "release_date": "1989-05-26", "page_count": 482}
{"index":{"_id": "Dune"}}
{"name": "Dune", "author": "Frank Herbert", "release_date": "1965-06-01", "page_count": 604}'
```
### 4.3 查询操作

#### 4.3.1 简单查询

```shell
# 根据文档id查询  ?pretty 可以去掉。加上格式更加好看
curl -XGET "http://hadoop101:9200/megacorp/_doc/1?pretty"

# 查询所有
curl -XGET "localhost:9200/megacorp/_search?pretty"

# 按字段查询 查询名字为Smith的数据 select * from megacorp where last_name = 'Smith'
curl -XGET "localhost:9200/megacorp/_search?q=last_name:Smith&pretty"
```

#### 4.3.2 复杂查询

```txt
must: 条件必须满足，相当于 and
should: 条件可以满足也可以不满足，相当于 or
must_not: 条件不需要满足，相当于 not

gt :  > 大于
lt :  < 小于
gte :  >= 大于等于
lte :  <= 小于等于
```

```shell
# 查询last_name为smith，age大于30的数据
curl -XGET "localhost:9200/megacorp/_search?pretty" -H 'Content-Type: application/json' -d'
{
    "query" : {
        "bool": {
            "must": {
                "match" : {
                    "last_name" : "smith" 
                }
            },
            "filter": {
                "range" : {
                    "age" : { "gt" : 30 } 
                }
            }
        }
    }
}'
```

```shell
# 搜索下所有喜欢攀岩（rock climbing）的员工
curl -XGET "localhost:9200/megacorp/_search?pretty" -H 'Content-Type: application/json' -d'
{
    "query" : {
        "match" : {
            "about" : "rock climbing"
        }
    }
}'
```

![[Pasted image 20231025183808.png|250]]

`_score` : 1.4167401, 相关性得分

Elasticsearch 默认按照相关性得分排序，即每个文档跟查询的匹配程度。第一个最高得分的结果很明显：John Smith 的 about 属性清楚地写着 "rock climbing" 。

但为什么 Jane Smith 也作为结果返回了呢？原因是她的 about 属性里提到了 "rock" 。因为只有 "rock" 而没有 "climbing" ，所以她的相关性得分低于 John 的。

因为默认分词器是按照单词分组的。

```shell
curl -XGET "localhost:9200/_analyze?pretty" -H 'Content-Type: application/json' -d'
{
  "analyzer": "standard",
  "text": "rock climbing"
}'
```

![[Pasted image 20231025192956.png]]

```shell
# 短语查询 match_phrase。只查询出一个结果
curl -XGET "localhost:9200/megacorp/_search?pretty" -H 'Content-Type: application/json' -d'
{
    "query" : {
        "match_phrase" : {
            "about" : "rock climbing"
        }
    }
}'

# 高亮查询
curl -XGET "localhost:9200/megacorp/_search?pretty" -H 'Content-Type: application/json' -d'
{
    "query" : {
        "match_phrase" : {
            "about" : "rock climbing"
        }
    },
    "highlight": {
        "fields" : {
            "about" : {}
        }
    }
}'
```

![[Pasted image 20231025184910.png|225]]

#### 4.3.3 SQL查询

```sql
curl -XPOST "localhost:9200/_sql?format=txt&pretty" -H 'Content-Type: application/json' -d'
{
  "query": "SELECT * FROM library WHERE release_date < '2000-01-01'"
}'
```

![[Pasted image 20231025191510.png]]

响应数据格式：

|**格式**|**`Accept`HTTP头**|**描述**|
|---|---|---|
|`csv`|`text/csv`|[逗号分隔的值](https://en.wikipedia.org/wiki/Comma-separated_values)|
|`json`|`application/json`|[JSON](https://www.json.org/)（JavaScript对象表示法）|
|`tsv`|`text/tab-separated-values`|[标签分隔的值](https://en.wikipedia.org/wiki/Tab-separated_values)|
|`txt`|`text/plain`|类似CLI的表示形式|
|`yaml`|`application/yaml`|[YAML](https://en.wikipedia.org/wiki/YAML)（YAML Ain’t Markup Language）人类可读格式|

### 4.4 SQL客户端

```shell
elasticsearch-sql-cli
```

```sql
SELECT * FROM library WHERE release_date < '2000-01-01'
```

![[Pasted image 20231025192254.png]]
## 五、分词操作

### 5.1 默认分词器

```shell
# text(允许分词)   keyword(不允许分词)
# 默认的分词器，用来进行英文分词，按照空格分

# standard 那一行可以不写，默认的
curl -XGET "localhost:9200/_analyze?pretty" -H 'Content-Type: application/json' -d'
{
  "analyzer": "standard",
  "text": "rock climbing"
}'

# 汉语按照字切分
curl -XGET "localhost:9200/_analyze?pretty" -H 'Content-Type: application/json' -d'
{
  "text": "我是中国人"
}'
```

ES自带的分词器，对中文的分词效果不明显，因此使用IK分词器。

![[Pasted image 20231025193006.png]]

![[Pasted image 20231025193120.png|475]]

### 5.2 IK分词器

IK分词器安装见[[ElasticSearch8.x安装#5.3 安装IK分词器插件]]

IK提供了两个分词算法ik_smart 和 ik_max_word。推荐用ik_smart

```shell
#ik_smart：智能分词。切分的所有单词的总字数等于词的总字数，即输入总字数=输出总字数
curl -XGET "localhost:9200/_analyze?pretty" -H 'Content-Type: application/json' -d'
{
  "analyzer": "ik_smart",
  "text": "我是中国人"
}'

#ik_max_word：最大化分词。 输入总字数 <= 输出总字数
#没有NLP(自然语言处理，没有人的情感，听不懂人话)功能
curl -XGET "localhost:9200/_analyze?pretty" -H 'Content-Type: application/json' -d'
{
  "analyzer": "ik_max_word",
  "text": "我是中国人"
}'
```

![[Pasted image 20231025212733.png|450]]

![[Pasted image 20231025212750.png|475]]


扩展词库：

```shell
cd /opt/module/elasticsearch-8.10.4/plugins/ik/config

vim IKAnalyzer.cfg.xml
```

根据需要选择，记得自己在文件（如mydict.dic）中添加自己定义的词语，之后保存

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE properties SYSTEM "http://java.sun.com/dtd/properties.dtd">
<properties>
	<comment>IK Analyzer 扩展配置</comment>
	<!--用户可以在这里配置自己的扩展字典 -->
	<entry key="ext_dict">custom/mydict.dic;custom/single_word_low_freq.dic</entry>
	 <!--用户可以在这里配置自己的扩展停止词字典-->
	<entry key="ext_stopwords">custom/ext_stopword.dic</entry>
 	<!--用户可以在这里配置远程扩展字典 -->
	<entry key="remote_ext_dict">location</entry>
 	<!--用户可以在这里配置远程扩展停止词字典-->
	<entry key="remote_ext_stopwords">http://xxx.com/xxx.dic</entry>
</properties>
```

之后分发刚刚的词库文件和配置文件，并重启ES。

### 5.3 IK分词器测试

创建索引，指定用ik分词器

```shell
# 创建索引的时候指定建立索引用什么分词器，查询时用什么分词器
curl -XPUT http://localhost:9200/iktest -H 'Content-Type:application/json' -d'
{
  "mappings": {
    "properties": {
       "content": {
          "type": "text",
          "analyzer": "ik_smart",
          "search_analyzer": "ik_smart"
       }
    }
  }
}'

```

新增数据：

```shell
# 新增数据
curl -XPOST http://localhost:9200/iktest/_create/1 -H 'Content-Type:application/json' -d'
{"content":"美国留给伊拉克的是个烂摊子吗"}'

curl -XPOST http://localhost:9200/iktest/_create/2 -H 'Content-Type:application/json' -d'
{"content":"公安部：各地校车将享最高路权"}'

curl -XPOST http://localhost:9200/iktest/_create/3 -H 'Content-Type:application/json' -d'
{"content":"中韩渔警冲突调查：韩警平均每天扣1艘中国渔船"}'

curl -XPOST http://localhost:9200/iktest/_create/4 -H 'Content-Type:application/json' -d'
{"content":"中国驻洛杉矶领事馆遭亚裔男子枪击 嫌犯已自首"}'
```

查询

```shell
# 查询
curl -XPOST http://localhost:9200/iktest/_search?pretty  -H 'Content-Type:application/json' -d'
{
    "query" : { "match" : { "content" : "中国" }},
    "highlight" : {
        "pre_tags" : ["<font color='red'>", "<tag2>"],
        "post_tags" : ["</font>", "</tag2>"],
        "fields" : {
            "content" : {}
        }
    }
}'

# 可以查看怎么分词的
curl -XGET "localhost:9200/_analyze?pretty" -H 'Content-Type: application/json' -d'
{
  "analyzer": "ik_smart",
  "text": "韩警平均每天扣1艘中国渔船"
}'

```

![[Pasted image 20231025221139.png|400]]

## 六、ElasticSearchDSL查询

### 6.1 索引信息


```shell
# 查看索引信息
curl -XGET http://localhost:9200/iktest?pretty
```

![[Pasted image 20231026092940.png|350]]

默认一个一个分片和一个副本，如下图所示：

![[Pasted image 20231026093400.png|300]]
### 6.2 数据准备

（1）建索引

```txt
分片:2 副本：2
默认分词器(搜索也会用指定默认的分词器)：ik_smart（也可以在elasticsearch.yml文件配置）
empid:员工id-->long 不分词
age:年龄-->integer 不分词 
balance:收入-->float 不建索引
name:姓名-->text 分词
gender:性别-->keyword 不分词
hobby:兴趣爱好-->text 分词
tag:标签 -->text 分词 不存储 用标准分词器

可以在_source 指定存储哪些列，不存储哪些列 excludes  includes
（1）_source和_all其实都是两个字段，只不过区别是_source是存储的结构化的原始文档，而_all是存储的是一个所有field字段拼接而成的字符串，两者是有区别的。  
（2）store是作用于field(字段)上的属性，决定该field是否单独存储一份文档（默认是false，不建议修改），该属性可以作用于_all字段上，但与_source字段重复。
```

```shell
curl -XPUT "http://hadoop101:9200/mytest" -H "Content-Type: application/json" -d '
{
    "settings": {
        "number_of_shards": 2,
        "number_of_replicas": 2,
        "index": {
            "analysis.analyzer.default.type" : "ik_smart"
        }
    },
    "mappings": {
             "_source":{
                 "excludes":["tag"]
              },
            "properties": {
                "empid": {
                    "type": "long"
                },
                "age": {
                    "type": "integer"
                },
                "balance": {
                    "type": "float",
                    "index": false
                },
                "name": {
                    "type": "text"
                },
                "gender": {
                    "type": "keyword"
                },
                "hobby": {
                    "type": "text"
                },
                "tag": {
                    "type": "text",
                    "analyzer": "standard"
                }
            }
    }
}'
```

![[Pasted image 20231026102526.png|250]]

（2）新增数据

```shell
curl -XPUT "http://hadoop101:9200/mytest/_bulk?refresh&pretty" -H 'Content-Type: application/json' -d'
{"index":{"_id":"1"}}
{"empid":1001,"age":20,"balance":2000,"name":"李三","gender":"男","hobby":"吃饭睡觉","tag":"吃饭睡觉"}
{"index":{"_id":"2"}}
{"empid":1002,"age":30,"balance":2600,"name":"李小三","gender":"男","hobby":"吃粑粑睡觉","tag":"吃粑粑睡觉"}
{"index":{"_id":"3"}}
{"empid":1003,"age":35,"balance":2900,"name":"张伟","gender":"女","hobby":"吃,睡觉","tag":"吃,睡觉"}
{"index":{"_id":"4"}}
{"empid":1004,"age":40,"balance":2600,"name":"张伟大","gender":"男","hobby":"打篮球睡觉","tag":"打篮球睡觉"}
{"index":{"_id":"5"}}
{"empid":1005,"age":23,"balance":2900,"name":"大张伟","gender":"女","hobby":"打乒乓球睡觉","tag":"打乒乓球睡觉"}
{"index":{"_id":"6"}}
{"empid":1006,"age":26,"balance":2700,"name":"张大喂","gender":"男","hobby":"打排球睡觉","tag":"打排球睡觉"}
{"index":{"_id":"7"}}
{"empid":1007,"age":29,"balance":3000,"name":"王五","gender":"女","hobby":"打牌睡觉","tag":"打牌睡觉"}
{"index":{"_id":"8"}}
{"empid":1008,"age":28,"balance":3000,"name":"王武","gender":"男","hobby":"打桥牌","tag":"打桥牌"}
{"index":{"_id":"9"}}
{"empid":1009,"age":32,"balance":32000,"name":"王小五","gender":"男","hobby":"喝酒,吃烧烤","tag":"喝酒,吃烧烤"}
{"index":{"_id":"10"}}
{"empid":1010,"age":37,"balance":3600,"name":"赵六","gender":"男","hobby":"吃饭喝酒","tag":"吃饭喝酒"}
{"index":{"_id":"11"}}
{"empid":1011,"age":39,"balance":3500,"name":"张小燕","gender":"女","hobby":"逛街,购物,买","tag":"逛街,购物,买"}
{"index":{"_id":"12"}}
{"empid":1012,"age":42,"balance":3500,"name":"李三","gender":"男","hobby":"逛酒吧,购物","tag":"逛酒吧,购物"}
{"index":{"_id":"13"}}
{"empid":1013,"age":42,"balance":3400,"name":"李球","gender":"男","hobby":"体育场,购物","tag":"体育场,购物"}
{"index":{"_id":"14"}}
{"empid":1014,"age":22,"balance":3400,"name":"李健身","gender":"男","hobby":"体育场,购物","tag":"体育场,购物"}
{"index":{"_id":"15"}}
{"empid":1015,"age":22,"balance":3400,"name":"Nick","gender":"男","hobby":"坐飞机,购物","tag":"坐飞机,购物"}
'
```
### 6.3 DSL语法介绍

| **关键字**   | **含义**                                               | **类比SQL**                                      |
| ------------ | ------------------------------------------------------ | ------------------------------------------------ |
| query        | 查询                                                   | select                                           |
| bool         | 将多个条件进行组合                                     | selext xxx from xxx where age=20 and gender=male |
| must         | 必须符合                                               | =       条件必须满足，相当于 and                                         |
| must_not     | 必须不符合                                             | !=        条件不需要满足，相当于 not                                       |
| should       | 最好符合，如果满足条件，可以加分                       |  条件可以满足也可以不满足，相当于 or                                            |
| filter       | 过滤条件                                               | where                                            |
| term         | 术语匹配                                               | gender=male                                      |
| match        | 全文检索                                               |                                                  |
| fuzzy        | 模糊音匹配                                             |                                                  |
| from         | 从哪一条开始查                                         |                                                  |
| size         | 查询数据的数量                                         | limit x                                          |
| `_source`      | 指定查询的字段                                         | select 字段                                      |
| multi_match  | 匹配多个字段中的内容                                   |                                                  |
| match_phrase | 短语匹配，将输入的查询内容整个作为整体进行查询，不切词 |                                                  |
### 6.3 DSL查询

#### 6.3.1 查询的两种方式

==需求：查询所有员工信息，并且按照年龄降序排序==

第一种:：REST

```shell
# GET  /index/_search?参数1=值1&参数2=值2
curl -XGET "http://hadoop101:9200/mytest/_search?pretty&sort=age:desc"
```

第二种:：DSL(特定领域语言)

```shell
curl -XGET "localhost:9200/mytest/_search?pretty" -H 'Content-Type: application/json' -d'
{
  "query": {
    "match_all": {}
  },
  "sort": [
    {
      "age": {
        "order": "desc"
      }
    }
  ]
}'
```
#### 6.3.2 基本查询

==需求：全表查询，按照年龄降序排序，再按照工资降序排序，只取前5条记录的empid，age，balance==

```shell
curl -XGET "localhost:9200/mytest/_search?pretty" -H 'Content-Type: application/json' -d'
{
  "query": {
    "match_all": {}
  },
  "sort": [
    {
      "age": {
        "order": "desc"
      }
    },
    {
      "balance": {
        "order": "desc"
      }
    }
  ],
  "from": 0,
  "size": 5,
  "_source": ["empid","age","balance"]
}'

# 可以通过sql转为DSL语句
curl -XPOST "localhost:9200/_sql/translate?pretty" -H 'Content-Type: application/json' -d'
{
  "query":"select empid,age,balance from mytest order by age desc ,balance desc limit 5"
}'
```
#### 6.3.3 全文检索

==需求：搜索hobby含有吃饭睡觉的员工==

```shell
# 分为吃饭和睡觉两个词
curl -XGET "localhost:9200/_analyze?pretty" -H 'Content-Type: application/json' -d'
{
  "analyzer": "ik_smart",
  "text": "吃饭睡觉"
}'

# 结果hobby含有吃饭或睡觉就可以
curl -XGET "localhost:9200/mytest/_search?pretty" -H 'Content-Type: application/json' -d'
{
  "query": {
    "match": {
      "hobby": "吃饭睡觉"
    }
  }
}'
```

部分结果显示：

![[Pasted image 20231026115026.png|175]]

```shell
# 建索引指定用标准分词器，搜索也会用标准分词器，主要有吃或饭或睡或觉都会被搜索出来
curl -XGET "localhost:9200/mytest/_search?pretty" -H 'Content-Type: application/json' -d'
{
  "query": {
    "match": {
      "tag": "吃饭睡觉"
    }
  }
}'
```

部分结果显示：

![[Pasted image 20231026120035.png|175]]

==需求：搜索工资是2000的员工==

```shell
curl -XGET "localhost:9200/mytest/_search?pretty" -H 'Content-Type: application/json' -d'
{
  "query": {
    "match": {
      "balance": 2000
    }
  }
}'

curl -XGET "localhost:9200/mytest/_search?pretty" -H 'Content-Type: application/json' -d'
{
  "query": {
    "term": {
      "balance": 2000
    }
  }
}'
```

==需求：搜索hobby是“吃饭睡觉”的员工==

```shell
curl -XGET "localhost:9200/mytest/_search?pretty" -H 'Content-Type: application/json' -d'
{
  "query": {
    "match_phrase": {
      "hobby": "吃饭睡觉"
    }
  }
}'
```

![[Pasted image 20231026124354.png|200]]
#### 6.3.4 多字段匹配

==需求：搜索name或hobby中带球的员工==

```shell
curl -XGET "localhost:9200/mytest/_search?pretty" -H 'Content-Type: application/json' -d'
{
  "query": {
   "multi_match": {
     "query": "球",
     "fields": ["name","hobby"]
   }
  }
}'
```

![[Pasted image 20231026124644.png|175]]
#### 6.3.5 多条件匹配

==需求：搜索男性中喜欢购物的员工==

```shell
curl -XGET "localhost:9200/mytest/_search?pretty" -H 'Content-Type: application/json' -d'
{
  "query": {
    "bool": {
      "must": [
        {
          "term": {
            "gender": {
              "value": "男"
            }
          }
        },
        {
          "match": {
            "hobby": "购物"
          }
        }
      ]
    }
  }
}'
```

==需求：搜索男性中喜欢购物，还不能爱去酒吧的员工==

```shell
curl -XGET "localhost:9200/mytest/_search?pretty" -H 'Content-Type: application/json' -d'
{
  "query": {
    "bool": {
      "must": [
        {
          "term": {
            "gender": {
              "value": "男"
            }
          }
        },
        {
          "match": {
            "hobby": "购物"
          }
        }
      ],
      "must_not": [
        {
          "match": {
            "hobby": "酒吧"
          }
        }
      ]
    }
  }
}'
```

==需求：搜索男性中喜欢购物，还不能爱去酒吧的员工，年龄最好在20-30之间==

```shell
curl -XGET "localhost:9200/mytest/_search?pretty" -H 'Content-Type: application/json' -d'
{
  "query": {
    "bool": {
      "must": [
        {
          "term": {
            "gender": {
              "value": "男"
            }
          }
        },
        {
          "match": {
            "hobby": "购物"
          }
        }
      ],
      "must_not": [
        {
          "match": {
            "hobby": "酒吧"
          }
        }
      ],
      "should": [
        {
          "range": {
            "age": {
              "gte": 20,
              "lte": 30
            }
          }
        }
      ]
    }
  }
}
```

![[Pasted image 20231026125817.png|275]]

==需求：搜索男性中喜欢购物，还不能爱去酒吧的员工，最好在20-30之间，不要40岁以上的==

```shell
curl -XGET "localhost:9200/mytest/_search?pretty" -H 'Content-Type: application/json' -d'
{
  "query": {
    "bool": {
      "must": [
        {
          "term": {
            "gender": {
              "value": "男"
            }
          }
        },
        {
          "match": {
            "hobby": "购物"
          }
        }
      ],
      "must_not": [
        {
          "match": {
            "hobby": "酒吧"
          }
        }
      ],
      "should": [
        {
          "range": {
            "age": {
              "gte": 20,
              "lte": 30
            }
          }
        }
      ],
      "filter": {
        "range": {
          "age": {
            "lt": 40
          }
        }
      }
    }
  }
}
```

![[Pasted image 20231026125935.png|175]]

==需求：搜索男性中喜欢购物 或 年龄超过39岁的人==

```shell
curl -XGET "localhost:9200/mytest/_search?pretty" -H 'Content-Type: application/json' -d'
{
  "query": {
    "bool": {
      "should": [
        {
           "bool": {
               "must": [
                   {
                       "term": {
                           "gender": "男"
                       }
                   },
                   {
                       "match": {
                           "hobby": "购物"
                       }
                   }
               ]
           }
        },
        {
          "range": {
            "age": {
            "gt": 39
            }
          }
        }
      ]
    }
  }
}'
```
#### 6.3.6 模糊音匹配

==需求：搜索Nick==

```shell
curl -XGET "localhost:9200/mytest/_search?pretty" -H 'Content-Type: application/json' -d'
{
  "query": {
    "fuzzy": {
      "name": "Dick"
    }
  }
}'
```

![[Pasted image 20231026133210.png|225]]
### 6.4 聚合

text类型的字段是无法聚合的，需要使用keyword类型代替。

==需求：统计男女员工各多少人==

```shell
curl -XGET "localhost:9200/mytest/_search?pretty" -H 'Content-Type: application/json' -d'
{
  "aggs": {
    "gendercount": {
      "terms": {
        "field": "gender",
        "size": 2
      }
    }
  }
}'
```

==需求：统计喜欢购物的男女员工各多少人==

```shell
curl -XGET "localhost:9200/mytest/_search?pretty" -H 'Content-Type: application/json' -d'
{
  "query": {
    "match": {
      "hobby": "购物"
    }
  }, 
  "aggs": {
    "gendercount": {
      "terms": {
        "field": "gender",
        "size": 2
      }
    }
  }
}'
```

==需求：统计喜欢购物的男女员工各多少人，及这些人总体的平均年龄==

```shell
curl -XGET "localhost:9200/mytest/_search?pretty" -H 'Content-Type: application/json' -d'
{
  "query": {
    "match": {
      "hobby": "购物"
    }
  }, 
  "aggs": {
    "gendercount": {
      "terms": {
        "field": "gender",
        "size": 2
      }
    },
    "avgage":{
      "avg": {
        "field": "age"
      }
    }
  }
}'
```

![[Pasted image 20231026134143.png|225]]

==统计喜欢购物的男女员工各多少人，及这些人不同性别的平均年龄==

```shell
curl -XGET "localhost:9200/mytest/_search?pretty" -H 'Content-Type: application/json' -d'
{
  "query": {
    "match": {
      "hobby": "购物"
    }
  },
  "aggs": {
    "gendercount": {
      "terms": {
        "field": "gender",
        "size": 2
      },
      "aggs": {
        "avgage": {
          "avg": {
            "field": "age"
          }
        }
      }
    }
  }
}'
```

![[Pasted image 20231026134346.png|225]]

## 七、API操作（java）

代码：com.bigdata.elasticSearch.EsQueryMain