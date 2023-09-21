---
author: wufm
title: Shell笔记
rating: 3
time: 2023-08-21 周一
tags:
 - shell
---

## 一、Shell概述和入门

Shell是一个命令行解释器，它接收应用程序/用户命令，然后调用系统内核。

Shell还是一个功能强大地编程语言，易编写、易调试、灵活性强。

![[Pasted image 20230821210132.png]]

```shell
# 查看Linux提供地Shell解析器
cat /etc/shells

结果
/bin/sh
/bin/bash
/usr/bin/sh
/usr/bin/bash

# bash和sh的关系
cd /bin
ll | grep bash

结果
lrwxrwxrwx. 1 root root          4 8月  18 15:20 sh -> bash

# CentOS默认的解释器十bash
echo $SHELL
```

**入门脚本：**

```shell
# 创建一个helloworld.sh脚本
vim helloworld.sh

# 添加如下内容
#!/bin/bash
echo "helloworld"
A=5

# 执行脚本的方式
# 1. sh/bash 绝对路径/相对路径
sh ./helloworld.sh
# 2.直接执行，需要对文件赋执行权限
chmod +x helloworld.sh
./helloworld.sh
# 3.以上方法都是在子shell执行，脚本执行完毕，子shell关闭，回到父shell中
# 在脚本路径前加“.”或者“source”,是不开的子shell。
# 主要区别在于环境的继承环境，在子shell的变量，父shell是不可见的。
. ./helloworld.sh
source ./helloworld.sh

# 可通过查看A变量来比较
echo $A
```
## 二、变量

### 2.1 系统预定义变量

```shell
# 查看shell中所有的变量
set

# 查看系统变量的值
echo $变量名
echo $USER
```
### 2.2 自定义变量

1. 定义变量：**变量名=变量值**，注意，**=号前后不能有空格**
2. 撤销变量：uset 变量名。静态变量不能unset（声明方法：readonly 变量名=变量值）
3. 变量名称可以由字符、数字和下划线组成，但是不能以数字开头。**环境变量名建议大写**
4. 在bash中，变量默认类型都是字符串，无法直接进行数值运算
5. 变量的值如果由空格，需要使用双引号或单引号括起来

```shell
# 定义变量
A=5
echo $A

# 给变量重新赋值
A=8
echo $A

# 撤销变量
unset A
echo $A

# 声明静态变量，不能unset
readonly B=2
B=9

# 默认都是字符串类型
C=1+2
echo $C

# 变量如果有空格，用双引号或单引号括起来
D="I love you"
echo $D

# 可把变量提升为全局环境变量，可供其他shell程序使用
vim test.sh

# 添加如下内容
#!/bin/bash
echo $D

# 执行脚本
sh ./test.sh

# 把变量B变成全局变量
export D

# 执行脚本
sh ./test.sh
```

```shell
d=$(date +%y%m%d)
echo $d
```
### 2.3 特殊变量

| 特殊变量 | 功能描述                                                                                           |
| -------- | -------------------------------------------------------------------------------------------------- |
| `$n`     | n为数字，`$0`代表该脚本名称，`$1`-`$9`代表第一 到第九个参数，十以上的参数，用大括号括起，如`${10}` |
| `$#`     | 获取所有输入参数个数。常用于循环，判断参数的个数是否正确以及加强脚本的健壮性                       |
| `$*`     | 命令行中所有的参数，把所有的参数看成一个整体                                                       |
| `$@`     | 命令行中所有的参数，每个参数区分对待                                                               |
| `$?`     | 最后一次执行的命令的返回状态。如果是0，则证明上一个命令正确执行。                                  |


```shell
# $*和$@用for in 遍历结果都一样，但是如果加双引号的话，"$*"只会输出一次
# 编写一个测试脚本
vim parameter.sh

# 添加如下内容
#!/bin/bash
echo '----------$n---$0-$2-----------------'
echo $0
echo $1
echo $2

echo '------------$#------------------------'
echo $#

echo '------------$*------------------------'
echo $*

echo '------------$@----------------------'
echo $@

# 执行脚本
sh ./parameter.sh haha xixi

# 判断parameter.sh脚本是否正确执行
echo $?
```
## 三、运算符

`$((运算式))` 或 `$[运算式]`

```shell
# 计算(2+3)*2
NUM1=$(((2+3)*2))
echo $NUM1

NUM2=$[(2+3)*2]
echo $NUM2

NUM3=`expr 2 + 3`
echo $NUM3
```
## 四、条件判断

```shell
# 23大于或等于22嘛
[ 23 -ge 22 ]
echo $?

# 1大于2嘛
[ 1 -gt 2 ]
echo $?

# helloworld.sh是否具有写权限
[ -w helloworld.sh ]
echo $?

# 判断目录中的文件是否存在
[ -e /root/test3.sh ]
echo $?

# 多条件判断（&&表示前一条命令执行成功时，才执行后一条命令，||表示上一个命令执行失败，才执行下一条命令）
[ hello ] && echo OK || echo notOK
[ ] && echo OK || echo notOK
```

基本语法：
[ 条件判断式 ]
注意：
- 注意条件判断式前后要有空格
- 条件非空即为true

常用判断条件
1. 两个整数之间比较
-eq等于（equal）           -ne不等于（not equal）
-lt小于（less than）        -le小于或等于（less equal）
-gt大于（greater than） -ge大于或等于（greater equal）

2. 两个字符串之间比较
用“=”判断相等，用“!=”判断不想等。

3. 按文件权限进行判断
-r 有读的权限（read）
-w 有写的权限（write）
-x 有执行的权限（execute）

4. 按文件类型判断
-e 文件存在（existence）
-f 文件存在并且时一个常规文件（file）
-d 文件存在并且时一个目录（directory）

## 五、流程控制（重点）

### 5.1 if判断

```shell
# 单分支
if [ 条件判断式 ];then
	程序
fi

# 或者
if [ 条件判断式 ]
then
	程序
fi
```

```shell
# 多分枝
if [ 条件判断式 ]
then
	程序
elif [ 条件判断式 ]
then
	程序
else
	程序
fi
```

```shell
# 如果年龄小于18岁，门票半价；如果年龄在18-55岁，门票全价；年龄大于55，门票免费
# -a表示并且，-o表示或
# 创建一个脚本
vim testIf.sh

# 添加如下内容
#!/bin/bash

if [ $1 -lt 18 ]
then
	echo "年龄小于18，门票半价"
elif [ $1 -gt 18 -a $1 -lt 55 ]
then
	echo "年龄在18-55之间，门票全价"
else
	echo "年龄大于55，门票免费"
fi

# 执行脚本
sh ./testIf.sh 19
```
### 5.2 case语句

```shell
# 双引号;;表示命令序列结束，相当与java中break
# 最后的*）表示默认模式，相当于java的default
case $变量名 in
"值1")
	程序
;;
"值2")
	程序
;;
*)
	程序
;;
esac
```

```shell
# 显示周几
# 创建一个脚本
vim testCase.sh

# 添加以下内容
#!/bin/bash

case $1 in
"1")
	echo "周一"
;;
"2")
	echo "周二"
;;
"3")
	echo "周三"
;;
"4")
	echo "周四"
;;
"5")
	echo "周五"
;;
"6")
	echo "周六"
;;
"7")
	echo "周日"
;;
*)
	echo "未知"
;;
esac

# 执行脚本
sh ./testCase.sh 2
```
### 5.3 for循环

```shell
for ((初始值;循环控制条件;变量变化))
do
	程序
done
```

```shell
for 变量 in 值1 值2 值3 ...
do
	程序
done
```

```shell
# 1-10相加等于多少
# 创建一个脚本
vim testFor1.sh

# 添加如下内容
#!/bin/bash

sum=0
for ((i=0;i<10;i++))
do
	sum=$[ $sum + $i + 1 ]
done
echo $sum

# 执行脚本
sh testFor1.sh
```

```shell
# 打印数字
# 创建一个脚本
vim testFor2.sh

# 添加以下内容
#!/bin/bash

for i in 1 2 3
do
	echo "I love $i"
done

# 执行脚本
sh ./testFor2.sh
```

### 5.4 while循环

```shell
while [ 条件判断式 ]
do
	程序
done
```

```shell
# 从1加到10
# 创建一个脚本
vim testWhile.sh

# 添加如下内容
#!/bin/bash

sum=0
i=0
while [ $i -lt 10 ]
do
	sum=$[$sum + $i + 1]
	i=$[$i + 1]
done
echo $sum

# 执行脚本
sh ./testWhile.sh
```
## 六、read读取控制台输入

`read [选项] [参数]`

选项：
- -p：指定读取值时的提示符
- -t：指定读取值时等待的时间（秒），默认时一直等待
参数
- 变量：指定读取值得变量名

```shell
# 请在7秒输入得你的姓名，并把值保存在变量name中
read -t 7 -p "enter your name in 7 seconds：" name
# 打印变量
echo $name
```

## 七、函数

### 7.1 系统函数

`basename 文件路径 [后缀]`

basename可以理解为取路径里的文件名称；
后缀会把文件名的后缀去掉

`dirname 文件绝对路径`

dirname可理解为取文件路径的绝对路径名称

```shell
# 提取文件名testCase.sh
basename /root/testCase.sh

# 提取testCase
basename /root/testCase.sh .sh

# 提取文件的绝对路径
dirname /root/testCase.sh
```

### 7.2 自定义函数

```shell
# 求和函数
# 创建一个脚本
vim testFun1.sh

# 添加如下内容
#!/bin/bash

function sum(){
	s=$[$1+$2]
	return $s
}

# 调用函数
sum $1 $2

# 执行脚本
sh ./testFun1.sh 1 2
echo $?
```

```shell
# 求和函数
# 创建一个脚本
vim testFun2.sh

# 添加如下内容
#!/bin/bash

function sum(){
	s=$[$1+$2]
}

# 调用函数
sum $1 $2

# 执行脚本
sh ./testFun2.sh 1 2
echo $?
```

## 八、正则表达式入门

在linux中，grep，sed，awk等文本处理工具都支持通过正则表达式进行模式匹配。

[[常用正则表达式]]

```shell
# 查找含atguigu的行
cat /etc/passwd | grep zhangsan

# 匹配以a开头的行
cat /etc/passwd | grep ^a

# 匹配以t结尾的行
cat /etc/passwd | grep t$

# 匹配一个任意的字符
cat /etc/passwd | grep r..t

# 匹配某个字符零次或多次
cat /etc/passwd | grep ro*t

# 匹配范围内的字符
cat /etc/passwd | grep r[a-c]*t

# 匹配含a$b的行。注意要用单引号引起来
cat /etc/passwd | grep 'a\$b'
```
## 九、文本处理工具

### 9.1 cut

`cut [选项参数] 文件`

cut的工作就是“剪”，具体的说就是根据某个分隔符分列，然后输出所需数据。默认的分隔符是制表符。

```shell
# 数据准备
vim cut.txt

# 添加如下数据
dong shen
guan zhen
wo shi
lai  pi
ni  hao

# 1.按空格切割，取第一列 -d指定分隔符，-f指定取第几列
cut -d " " -f 1 cut.txt

# 2.按空格切割，取第二和第三列
cut -d " " -f 2,3 cut.txt

# 3.取出guan
cat cut.txt | grep guan | cut -d " " -f 1
```

```shell
# 选取系统PATH变量值，第2个“:”开始后的所有路径
echo $PATH

# 结果
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/root/bin

echo $PATH | cut -d ":" -f 3-

# 结果
/usr/sbin:/usr/bin:/root/bin
```

### 9.2 awk

`awk [选项参数] '/pattern1/{action1} /pattern2/{action2}...' filename`

一个强大的文本分析工具，把文件逐行的读入，默认以空格分隔每行切片，切开后的部分再进行分析处理。
pattern:匹配模式
action:在找到匹配内容时所执行的一系列命令
选项参数说明：
- -F：指定输入文件分隔符
- -v：赋值一个用户定义变量

```shell
# 数据准备
cp /etc/passwd ./

cat passwd
# 部分数据
# 用户名：密码（加密过后的）：用户id：组id：注释：用户家目录：shell解析器
root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin
```

```shell
# 1.搜索passwd文件以root开头的所有行，并输出该行的第7列
awk -F : '/^root/{print $7}' passwd

# 结果
/bin/bash

# 2.搜索passwd文件以root开头的所有行，并输出该行的第1列和第7列，中间用逗号隔开
awk -F : '/^root/{print $1","$7}' passwd

# 结果
root,/bin/bash

# 3.只显示passwd的第一列和第七列，以逗号隔开。且添加列名为user和shell。在最后一行添加“ceshi,bin/badboy”
# BEGIN在所有数据读取行之前执行；END在所有数据执行之后执行
awk -F : 'BEGIN{print "user,shell"}{print $1","$7}END{print "ceshi,bin/badboy"}' passwd

# 结果
user,shell
root,/bin/bash
...
ceshi,bin/badboy

# 4.将passwd文件的用户id增加数值1并输出
awk -F : -v i=1 '{print $3+i}' passwd

# 部分结果
1
2
3
4

```

awk的内置变量

| 变量     | 说明                                   |
| -------- | -------------------------------------- |
| FILENAME | 文件名                                 |
| NR       | 已读的记录数                           |
| NF       | 浏览记录的域的个数（切割后，列的个数） |

```shell
# 统计passwd文件名，每行的行号，每行的列数
awk -F : '{print "filename:"FILENAME ",linenum:"NR ",col:"NF}' passwd

# 部分结果
filename:passwd,linenum:1,col:7
filename:passwd,linenum:2,col:7
...

# 查询ifconfig命令输出结果中的空行所在的行号
ifconfig | awk '/^$/{print NR}'

# 结果
9
18
```