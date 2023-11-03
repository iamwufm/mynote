---
author: wufm
title: obsidian笔记
rating: 0
time: 2023-09-21 周四
tags:
  - obsidian
  - markdown
代码:
---

**obsidian**:基于==**markdown**==语法和==**双链功能**==的深度知识管理工具
## 一、常用markdown语法

### 1.1标题
最多支持6层标题。用#加空格。#号个数代码主题级别。如一个#是一级主题。

```txt
# 我是一级主题
```

### 1.2加粗或高亮

加粗默认快捷键是ctrl+b。

```txt
**加粗**
==高亮==
==**又粗又高亮**==
**==又粗又高亮==**
```

**加粗**
==高亮==
==**又粗又高亮**==
**==又粗又高亮==**

### 1.3斜体、删除线、下划线

```txt
*斜体*
~~删除线~~
<u>下划线</u>
```

*斜体*
~~删除线~~
<u>下划线</u>

### 1.4上下标

```txt
m<sup>2</sup>
H<sub>2</sub>O
```

m<sup>2</sup>
H<sub>2</sub>O

### 1.5超链接与图片

```txt
[百度](baidu.com)
[回到二级标题-常用markdown语法](#一、常用markdown语法)
![[图片3.png]]
![环境变量配置图片](imgs/图片3.png)
```

[百度](baidu.com)
[回到二级标题-常用markdown语法](#一、常用markdown语法)
![[图片3.png]]

![环境变量配置图片](imgs/图片3.png)
### 1.6列表
tab键控制缩进。
数字+点+空格
-/+/* + 空格
```
1. 学习
	1. 学python
	2. 学java
2. 实践

- 第一点（推荐）
+ 第二点
* 第三点
```

1. 学习
	1. 学python
	2. 学java
2. 实践

- 第一点（推荐）
+ 第二点
* 第三点
### 1.7表格

|线分隔单元格
```txt
|name|age|
|--|--|
|wfm|18|
|zhangsan|28|
```

可以安装advanced tables插件来快速创建表。tab下一个单元格，enter下一行。

| name     | age |
| -------- | --- |
| wfm      | 18  |
| zhangsan | 28  |

### 1.8代码和代码块

代码：`java -version`
代码块：
```java
public class MyConstant {
    // main入口（快捷键psvm）,必须有这个方法，程序才能执行
    public static void main(String[] args) {
        // 字符串常量（快捷键sout）
        System.out.println("hello world!");
    }
}
```

### 1.9任务列表

```txt
- [ ] java
- [x] python
```

- [ ] java
- [x] python

### 1.10分割线

三个-线
```txt
---
```

---

### 1.11引用
```txt
>我没有说过这句话--鲁迅
```

>我没有说过这句话--鲁迅


## ==二、双链==

两对中框号`[[]]`
`[[ 某文件]]`
`[[ 某文件#某标题]]`
`[[ 某文件#^某段话]]`
`[[ 某文件#^某段话|别名]]`
``![[ 某文件#^某段话|别名]]``
```txt
[[window安装java]]
[[Windows安装Java#1.1 下载JDK应用程序]]
[[window安装java#^07b760]]
[[window安装java#^07b760|maven官网地址]]
![[window安装java#^07b760|maven官网地址]]
```
[[Windows安装Java]]

[[Windows安装Java#1.1 下载JDK应用程序]]

[[Windows安装Java#^07b760]]

[[Windows安装Java#^07b760|maven官网地址]]

![[Windows安装Java#^07b760|maven官网地址]]

## 三、常用第三方插件

### 3.1同步插件
主要是解决pc端到手机端的同步问题。手机端主要是查看笔记，一般没有修改或新增笔记的需求。

目前了解的插件有obsidian git插件和remotely save插件。

remotely save插件可以配合cos使用，cos是收费，但是价格不高，可以考虑，网上有一堆配置资料。

本人使用的是obsidian git插件。只需要在手机端的obsidian软件安装该插件，然后在命令面板输入clone关键词，克隆github中的数据仓库。建议数据仓库忽略.obsidian目录，不然目录过大，clone和pull时间过久。可以在该插件中设置多久pull一次，也可以手动执行命名pull。

手机端git插件的设置（除了pull功能，其他都关闭）和克隆拉取的操作如下：
1. **自动pull的时间间隔**
![[Pasted image 20230807171547.png]]

![[Pasted image 20230807171657.png]]

**2. 仓库的目录路径**
![[Pasted image 20230807171737.png]]

**3. 克隆或拉取的命令**

![[Pasted image 20230807171912.png]]

![[Pasted image 20230807171933.png]]


### 3.2表格插件

Advanced Table插件。先输入|，然后输入列明，按tab到下一个单元格，enter到一行。也可以点击左侧功能区的表格图标，右侧会出现相应的功能，可以按需选择对表格进行操作。


| name | age | sex |
| ---- | --- | --- |
| 张三 | 18  | 男  |
| 李四 | 20  | 男  |
| 王五 | 17  | 女    |

如果没有出现如下图，可以在github下载旧的版本解决此问题。
![[Pasted image 20230807172558.png]]
### 3.3搜索插件
dataview插件：搜索和查询功能
操作手册见官方文档：[数据视图 (blacksmithgu.github.io)](https://blacksmithgu.github.io/obsidian-dataview/)

1. 使用逻辑运算符：
	- `OR`：或
	- `-`：非
	- 加上括号设置复杂逻辑： `(obsidian OR ob) - Obsidian`

![[Pasted image 20230921162017.png|225]]

查询00-common目录下所有的文档

```dataview
list
from "00-common"
```

### 3.4脑图插件

Obsidian markmind插件：使用见[[markmind]]

Mind Map插件：可以把标题或列表转为思维导图
![[Pasted image 20230807180137.png|350]]
### 3.5流程图插件
Excalidraw插件

### 3.6 日记插件

日历插件：Calendar
任务管理插件：Tasks
灵感插件：Obsidian Memos

在核心插件开启日记功能。Calendar插件开启后会在右上角右一个日期图案，点击相应的日期，可以帮助我们快速打开或者创建日记，还能开启周记功能。

想做专业的任务的管理，建议用滴答清单。开启tasks插件后，可以在命令面板中输入task,然后选择create...。创建完一个任务如图所示
![[Pasted image 20230807190717.png]]


还可以使用tasks插件语法筛选你想要的信息(具体用法参照官方文档)。如下所示：
筛选出你**整个笔记库内所有未完成的任务**

```tasks
not done
```


开启Memos插件后，点击左侧灯泡按钮，记录自己的灵感，记录的内容会显示在日记里。


![[Pasted image 20230807201341.png]]