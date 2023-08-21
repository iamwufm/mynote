---
author: wufm
title: Linux笔记
rating: 3
time: 2023-08-18 周五
tags:
 - Linux
 - centOS
---

## 一、Linux入门
### 1.1 概述

**Linux是一个操作系统。**
![[Pasted image 20230818132811.png]]

Linux内核最初是由芬兰人**林纳斯·托瓦兹**（Linus Torvalds）在赫尔辛基大学上学时出于个人爱好而编写的。

Linux继承了Unix以网络为核心的设计思想，是一个性能稳定的多用户网络操作系统。
![[Pasted image 20230818132253.png]]

### 1.2 Linux的解释

![[Pasted image 20230818132403.png]]

### 1.3 Linux发行版本

国内比较常用的发行版本是**centOS和ubuntu**。
![[Pasted image 20230818132502.png]]

## 二、VM与Linux安装

详情请参见[[VM与CentOS安装]]
## 三、Linux文件与目录结构

**Linux系统中一切皆文件。**
目录结构是树状结构，只有一个根。
## 四、vi/vim编辑器（重点）

vi是文本编辑器。vim编辑器是从vi发展出来的一个性能更强大的文本编辑器，可以主动的以字体颜色辨别的正确性，方便程序开发。

```shell
# 下载安装vim
yum install vim -y
```

![[Drawing 2023-08-18 19.32.23.excalidraw]]
常用一般模式

| 语法 | 功能描述             |
| ---- | -------------------- |
| yy   | 复制光标所在那一行   |
| p    | 粘贴                 |
| dd   | 删除光标所在的那一行 |
| 数字+shift+g | 移动到第几行                  |
| shift+g      | 移动到页尾                    |

常用命名模式

| 语法        | 功能描述   |
| ----------- | ---------- |
| :wq         | 保存并退出 |
| :q          | 退出       |
| :q!         | 强制退出   |
| /要查找的词 | n查找下一个，N往上查找           |
## 五、网络配置（重点）

### 5.1 查看网络ip和网关

#### 5.1.1 查看VM的网络信息
![[Pasted image 20230820163103.png]]

![[Pasted image 20230820163205.png]]

![[Pasted image 20230820163255.png]]

#### 5.1.2 查看windows环境中VMnet8网络配置

![[Pasted image 20230820163425.png]]

![[Pasted image 20230820163641.png]]

![[Pasted image 20230820163708.png]]
### 5.2 配置网络IP地址

#### 5.2.1 查看机器的ip

```shell
ifconfig
```

#### 5.2.2 修改ip地址

```shell
# 1.修改网络信息
vim /etc/sysconfig/network-scripts/ifcfg-ens33

# 修改内容如下：
BOOTPROTO=static # dhcp动态ip地址，static静态ip地址
# UUID那行删除，开机会自动分配，避免与克隆那台机器一样
ONBOOT=yes # yes开启网卡自动连接
IPADDR=修改没有冲突的ip
GATEWAY=网关


# 2.重启网络
service network restart
```

#### 5.2.3 两个机器是否互通

```shell
# 1.测试某虚拟机是否可以ping通百度
ping baidu.com

# 2.测试物理机是否可以ping通某台虚拟机
ping ip地址
```
### 5.3 配置主机名

#### 5.3.1 修改主机名称

```shell
# 查看当前服务器主机名称
hostname

# 修改主机名称
vim /etc/hostname

# 重启使主机名生效
reboot
```

#### 5.3.2 修改hosts映射文件

```shell
# 1.修改linux主机映射文件
vim /etc/hosts

# 添加以下内容
ip地址1 名称1
ip地址2 名称2

# 2.修改windows的主机映射文件
# 打开文件 C:\Windows\System32\drivers\etc\hosts

# 添加以下内容
ip地址1 名称1
ip地址2 名称2
```

## 六、系统管理

### 6.1 进程和服务
计算机中，一个正在执行的程序或命令，被叫做“进程”（process）。
启动之后一直存在、常驻内存的进程，一般被称为“服务”（service）。

#### 6.1.1 服务管理

```shell
# 基本语法
systemctl start | stop | restart | status 服务名

# 可以进入某个目录查看
cd /usr/lib/systemd/system

# 实例操作
# 查看防火墙服务的状态
systemctl status firewalld

```

#### 6.1.2 后台服务的自启设置

```shell
# 关掉指定服务的自动启动
systemctl disable service_name

# 开始指定服务的自动启动
systemctl enable service_name

# 实例
# 关掉防火墙自动启动
systemctl disable firewalld.service
```

### 6.2 关闭防火墙

```shell
# 查看防火墙的状态
systemctl status firewalld

# 临时关闭防火墙
systemctl stop firewalld

# 关掉防火墙自动启动
systemctl disable firewalld.service

# 开始防火墙自动启动
systemctl enable firewalld.service
```

### 6.3 关机重启命令

```shell
# 将数据由内存同步到硬盘中
sync

# 重启
reboot

# 关机
poweroff
```

## 七、常用基本命令（重点）

shell可以看作一个命令解释器，为我们提供了交互式的文本控制台界面。我们可以通过终端控制台来输入命令，由shell进行解释并最终交给内核执行。本章将分类介绍常用的基本shell命令。

### 7.1 帮助命令

#### 7.1.1 man获得帮助信息

```shell
man 命令或配置文件

# 如
man ls
```

#### 7.1.2 help获得shell内置命令的帮助信息

一部分基本功能的系统命令内嵌在shell中，系统加载启动之后会随着shell一起加载，常驻系统内存中。这部分命令被称为“内置命令”；相应的其他命令被称为“外部命令”。

```shell
# 如
help cd
```
#### 7.1.3 常用快捷键

| 常用快捷键 | 功能             |
| ---------- | ---------------- |
| ctrl+c     | 停止进程         |
| ctrl+l     | 清屏             |
| 善用tab键  | 提示             |
| 上下键     | 查找执行过的命令 |

### 7.2 文件目录类

```shell
# 显示当前工作目录的绝对路径
pwd

# 列出目录的内容
ll # 显示详情，等价与ls -l
ls -a # 显示隐藏文件

# 切换目录
cd 绝对路径/相对路径/../-

# 创建新目录
mkdir 目录名
mkdir -p 目录名1/目录名2 #创建多层目录

# 删除一个空的目录
rmkdir 目录名
```

```shell
# 创建空的文件
touch 文件名

# 复制文件或目录
cp 源文件或目录 新文件或目录
cp -r 源文件或目录 新文件或目录 # 递归复制

# 删除文件或目录
rm 文件或目录
rm -rf 文件或目录 # r:递归删除；f:强制执行；显示详细执行过程

# 移动文件与目录或重命名
mv 旧文件名 新文件名
mv 文件或目录 新文件或目录

# 查看文件内容
cat -n 文件 # n显示行号
more 文件 # 分屏查看
less 文件 # 与more类似，但是比more功能更加强大

# 输出内容到控制台
echo "内容"
echo -e "内容" # e:支持反斜杠控制的字符转换

# 显示文件头部内容
head -n 行数 文件 # 默认显示10行 head 文件

# 输出文件尾部内容
tail -n 行数 文件 # 默认显示10行 tail 文件
tail -f 文件 # 实时追踪该文件的所有更新
```

```shell
# > 输出重定向（覆盖写）
ls -l > 文件 # 列表的内容写入文件中（覆盖写）
cat 文件1 > 文件2 # 将文件1的内容覆盖到文件2

# >> 追加
ls -al >> 文件 # 列表的内容追加到文件的末尾
echo "内容" >> 文件 # 将内容追加到文件中
```

```shell
# 软链接也称为符号链接，类似于windows里面的快捷方式，有自己的数据块，主要存在链接其他文件的路径

# 创建一个软链接
ln -s 原文件或目录 软链接名

# 进入软链接实际物理路径
cd -P testLn/

# 删除软链接
rm -rf 软链接名 # 如果是：rm -rf 软链接名/，会把软链接对应的真实目录下的内容删掉
```

![[Pasted image 20230820191423.png]]

```shell
# 查看已经执行过的历史命令
history
```
### 7.3 时间日期类

```txt
date [选项] [+格式]

选项说明：
-d"时间字符串"：显示指定的“时间字符串”表示的时间，而非当前时间
 -s"日期时间"：设置系统日期时间

参数说明：
+日期时间格式：指定显示时使用的日期时间格式
```

#### 7.3.1 显示当前时间

```shell
# 显示当前时间
date

# 显示当前年份
date +%Y

# 显示当前月份
date +%m

# 显示年月日时分秒
date "+%Y-%m-%d %H:%M:%S"

```

#### 7.3.2 显示非当前时间

```shell
# 显示前一天时间
date -d'1 day ago'

# 显示明天时间
date -d'-1 day ago'

# 显示上一个月
date -d'1 month ago'
```

#### 7.3.3 设置系统时间

```shell
# 设置系统当前时间
date -s"2023-01-01 20:52:18"
```

#### 7.3.4 cal查看日历

```shell
# 查看当前月的日历
cal

# 查看2017年的日历
cal 2020
```
### 7.4 用户管理命令

```shell
# 添加新用户
useradd 用户名 # 新建用户后，在/home 目录下会出现相应的文件夹
useradd -g 组名 用户名 # 添加用户到某个组

# 设置用户密码
passwd 用户名

# 查看用户是否存在
id 用户名

# 查看创建了哪些用户
cat /etc/passwd

# 切换用户
su 用户名 # 环境没有切换
su - 用户名 # 环境也切换

# 查看环境变量
echo $PATH

# 删除用户
userdel 用户名 # 删除用户但保存用户主目录
userdel -r 用户名 # 用户和用户主目录都删除

# 查看登陆用户信息
whoami # 显示自身用户名
who am i # 显示登陆用户的用户名以及登陆时间

# 设置普通用户具有root权限
vim /etc/sudoers
# 在root下面添加一行
用户名    ALL=(ALL)       ALL
#或者采用sudo,不需要输入密码
用户名    ALL=(ALL)       NOPASSWD:ALL

```

### 7.5 用户组管理命令

每个用户都有一个用户组，系统可以对一个用户组中的所有用户进行集中管理。不同的linux系统对用户组的规定有所不同。在创建用户时会创建一个同名的用户组。用户组的管理涉及用户组的添加、删除和修改。用户组的管理实际上就是对/etc/group文件的更新。

```shell
# 新增一个用户组
groupadd 组名

# 查看创建了哪些组
cat /etc/group

# 修改一个用户组
groupmod -n 新组名 旧组名

# 删除一个用户组
groupdel 组名
```
### 7.6 文件权限类

为了保护系统的安全性，Linux系统对不同的用户访问同一个文件（包括目录文件）的权限做了不同的规定。在Linux中我们可以使用ll命令来显示一个文件的属性以及文件所属的用户和组。

![[Drawing 2023-08-21 09.11.46.excalidraw]]
链接数：
- 如果是文件，指的是硬链接个数
- 如果是文件夹，指的是子文件夹个数


从左到右的10个字符表示

| 文件类型 |  属主权限   |  属组权限  | 其他用户权限 |
|:--------:|:-----------:|:----------:|:------------:|
|    0     |   1  2  3   |  4  5  6   |   7  8  9    |
|    d     |   r  w  x   |  r  -  x   |   r  -  x    |
| 目录文件 | 读  写 执行 | 读 写 执行 |  读 写 执行  |

**r=4  w=2  x=1  rwx=4+2+1=7**

1. 0首位表示类型
	- -代表文件
	- d代表目录
	- l代表链接文件

2. 第1-3位确定属主（该文件的所有者）拥有该文件的权限。--User
3. 第4-6位确定属组（所有者的同组用户）拥有该文件的权限。-- Group
4. 第7-9位确定其他用户拥有该文件的权限。--Other

**rwx作用到文件和目录的区别**

|     |                         文件                         |                 目录                 |
|:---:|:----------------------------------------------------:|:------------------------------------:|
|  r  |                      读取、查看                      |        读取（ls查看目录内容）        |
|  w  | 修改（想删除文件，需要拥有对该文件所在目录的写权限） | 修改（目录内创建、删除和重命名目录） |
|  x  |                    可以被系统执行                    |            可以进入该目录            |

```shell
# u:属主 g:属组 o:其他组 a:所有人

# 修改权限

# 修改文件使其所属主具有执行权限
chmod u+x 文件名
# 去除文件所属主用户执行权限，并使其他用户具有执行权限
chmod u-x,o+x 文件名
# 设置文件属主、所属组和其他用户都具有可读可写可执行权限
chmod 777 文件名
# 修改整个文件夹里面的所有文件的所属主、所属主和其他用户都具有可读可写可执行权限
chmod -R 777 目录
```

```shell
# 修改文件所属主
chown 用户名 文件名
chgrp 用户名 文件名
# 递归改变文件所属主和所属组
chown -R 属主:属组 目录
```
### 7.7 搜索查找类

find指定将从指定目录向下递归地遍历其各个子目录，将满足条件的文件显示在终端。

```shell
# 按文件名：根据名称查找目录下的含有“.txt”文件
find 目录 -name "*.txt*"

# 按所属主：查看目录下，用户名称为某用户的文件
find 目录 -user 用户

# 按文件大小：在某目录下查找大于200m的文件（+n大于，-n小于，n等于）
find 目录 -size +204800
```

locate指令无需遍整个文件系统，查询速度较快。为了保证查询结果的准确性，管理员必须定期更新locate时刻。

```shell
# 安装mlocate包，mlocate是新型的locate,updatedb速度更快。
yum install mlocate -y

# 创建locate数据库
updatedb

# 查询文件
locate 文件名
```

grep过滤查找，管道符“|”，表示将前一个命令的处理结果输出传递给后面的命令处理。

grep 选项 查找内容 源文件

```shell
# 查看某文件再第几行。-n显示匹配行及行号
ls|grep -n test
```
### 7.8 压缩和解压类

**1. gzip/gunzip**
	- 压缩：gzip 文件名
	- 解压：gunzip 压缩名.gz

```shell
# 只能压缩文件不能压缩目录
# 不保留原来文件
# 同时多个文件会产生多个压缩包

# 压缩（只能将文件压缩为*.gz文件）
gzip 文件名

# 解压
gunzip 压缩名.gz
```

**2. zip/unzip**
	- 压缩：zip [选项] 压缩名.zip 将要压缩的内容（压缩文件和目录都可以）
	- 解压：unzip [选项] 压缩名.zip
- zip选项：
	- -r：压缩目录
- unzip选项：
	- -d<目录>：指定压缩后文件的存放目录

```shell
# zip压缩命令再windows/linux都通用，可以压缩目录且保留源文件

# 将多个文件压缩
zip 压缩名.zip 文件名1 文件名2

# 解压
unzip 压缩名.zip
unzip 压缩名.zip -d 目录
```

**3. tar**

tar [选项] 压缩名.tar.gz 将要打包的内容

| 选项 | 功能               |
| ---- | ------------------ |
| -c   | 产生.tar打包文件   |
| -v   | 显示详细信息       |
| -f   | 只当压缩后的文件名 |
| -z   | 打包同时压缩       |
| -x   | 解压.tar文件       |
| -C   | 解压到指定目录                   |

```shell
# 压缩多个文件
tar -zcvf 压缩名.tar.gz 文件名1 文件名2

# 压缩目录
tar -zcvf 压缩名.tar.gz 目录

# 解压到当前目录
tar -zxvf 压缩名.tar.gz

# 解压到指定目录
tar -zxvf 压缩名.tar.gz -C 目录
```
### 7.9 磁盘情况

#### 7.9.1 查看文件和目录占用的磁盘空间

du [选项] 目录/文件

| 选项          | 功能                                               |
| ------------- | -------------------------------------------------- |
| -h            | 以人们较易阅读的GBytes,MBytes,KBytes等格式自行显示 |
| -a            | 不仅查看子目录大小，还要包括文件                   |
| -c            | 显示所有的文件和子目录大小后，显示总和             |
| -s            | 只显示总和                                         |
| --max-depth=n | 指定统计子目录的深度为第n层                        |

```shell
# 显示当前目录占用磁盘空间大小
du -sh
```

#### 7.9.2 查看磁盘空间使用情况

```shell
df -h
```
### 7.10 进程管理类

进程是正在执行的一个程序或命令，每一个进程都是一个运行的实体，都有自己的地址空间，并占用一定的系统资源。

#### 7.10.1 查看当前系统进程状态

```shell
# 查看进程的cpu占用率和内存占用率
ps aux

# 查看进程的父进程ID
ps -ef
```

![[Drawing 2023-08-21 11.25.14.excalidraw]]

![[Drawing 2023-08-21 11.46.01.excalidraw]]
#### 7.10.2 终止进程

kill [选项] 进程号
killall 进程名称
选项：-9表示强迫进程立即停止

```shell
# 根据进程号杀死进程
kill -9 进程号

# 根据进程名称杀死进程
killall firefox
```
#### 7.10.3 查看进程树

```shell
# 安装包
yum install psmisc -y

# 显示进程id
pstree -p

# 显示进程所属用户
pstree -u
```

#### 7.10.4 实时监控系统进程状态

```shell
# -d 几秒更新，默认三秒
top -d 1
# -i不显示任何闲置或僵死进程
top -i
# -p通过监控进程ID来仅仅监控某个进程的状态
top -p 2575
```

| 操作 | 功能                            |
| ---- | ------------------------------- |
| P    | 以CPU使用率排序，默认就是此选项 |
| M    | 以内存使用率排序                |
| N    | 以PID排序                       |
| q    | 退出top                                |

#### 7.10.5 显示网络状态和端口占用信息

```shell
# 查看该进程网络信息
netstat -anp | grep 进程号

# 查看网络端口号占用情况
netstat -tunlp | grep 端口号

# 查看某进程监控的端口号
netstat -ntlp | grep 进程号
```



### 7.11 crontab系统定时任务

格式：
**分 时 天 月 周** 执行的任务

```shell
# 编辑定时任务
crontab -e

# 每隔一分钟想某文件添加一个11数字
*/1 * * * * /bin/echo "11" >> 文件名

# 查询定时任务
crontab -l

# 删除当前用户所有的定时任务
crontab -r
```

| 项目   | 含义                   | 范围 |
| ------ | ---------------------- | ---- |
| 第一个 | 一个小时当中的第几分钟 | 0-59 |
| 第二个 | 一天当中的第几个小时   | 0-23 |
| 第三个 | 一个月当中的第几天     | 1-31 |
| 第四个 | 一年当中的第几月       | 1-12 |
| 第五个 | 一周当中的星期几       | 0-7（0和7都代表星期日）     |


| 特殊符号 | 含义       | 例子                                                         |
| -------- | ---------- | ------------------------------------------------------------ |
| *        | 任何时间   | 比如第一个`*`就代表一个小时中每分钟都执行一次的意思          |
| ,        | 不连续时间 | `0 8,12,16 * * *` 代表每天8点0分、12点0分和16点0分都执行一次 |
| -        | 连续的范围 | `0 5 * * 1-6` 代表周一到周六的凌晨5点0分执行                 |
| `*/n`    | 每隔多久   | `*/10 * * * *`代表每个10分钟执行一次                         |

例子实操：
 
| 时间           | 含义                                |
| -------------- | ----------------------------------- |
| `45 22 * * *`  | 每天22点45分执行                    |
| `0 17 * * 1`   | 每周一17点0分执行                   |
| `0 5 1,15 * *` | 每月1号和15号的5点0分执行           |
| `40 4 * * 1-5` | 每周一到周五的4点40分执行           |
| `*/10 4 * * *` | 每天4点每隔10分钟执行一次           |
| `0 0 1,15 * 1` | 每月1号和15号，每周一0点0分执行一次 |

## 八、软件包管理

RPM(RedHat Package Manager)，RedHat软件包管理工具，类似windows里面的setup.exe。是Linux系列操作系统里面的打包安装工具。

RPM包的名称格式：
Apache-1.3.23-11.i386.rpm
- "Apache"软件名称
- ”1.3.23-11“软件的版本号
- ”i386“是软件所运行的硬件平台，Intel32位处理器的统称
- ”rpm“文件拓展名，代表RPM包

YUM是一个在Fedora和RedHat以及CentOS中Shell前端软件包管理器。基于RPM包管理，能够从指定的服务器自动下载RPM包并且安
装，可以自动处理依赖性关系，并且一次安装所有依赖的软件包，无需繁琐地一次次下载、安装。

YUM类似我们java开发中地maven工具，可以从镜像网站上下载应用程序，并直接安装。

```shellc
# 安装命令
rpm -ivh RPM包全名
# 安装firefox
rpm -ivh firefox-45.0.1-1.el6.centos.x86_64.rpm

# 查询所安装地所有RPM软件包
rpm -qa
rpm -qa | grep firefox

# 卸载软件包
rpm -e RPM软件包
# 卸载软件包，不检查依赖。这样的话，那些使用该软件包地软件在此之后可能就不能正常工作
rpm -e --nodeps RPM软件包
```

**`yum [选项] [参数]`**

```shell
# 采用yum方式安装firefox
yum install firefox -y
```

| 选项 | 说明                |
| ---- | ------------------- |
| -y   | 对所有问题都回答yes |

| 参数         | 功能                          |
| ------------ | ----------------------------- |
| install      | 安装RPM软件包                 |
| update       | 更新RPM软件包                 |
| check-update | 检查是否有可用地更新RPM软件包 |
| remove       | 删除指定地RPM软件包           |
| list         | 显示软件包信息                |
| clean        | 清理yum过期缓存               |
| deplist      | 显示yum软件包地所有依赖关系   |

修改网络YUM源
默认地系统YUM源，需连接国外apache网站，网速比较慢，可以修改关联地网络YUM源为国内镜像地网站，比如网易，aliyun

```shell]
# 1.安装wget，wget用来指定地URL下载文件
yum install wget

# 2.进入某目录，备份文件
cd /etc/yum.repos.d/
cp CentOS-Base.repo CentOS-Base.repo.backup

# 3. 下载网易163或者aliyun地repos文件，人选其一
wget
http://mirrors.aliyun.com/repo/Centos-7.repo

或
wget
http://mirrors.163.com/.help/CentOS7-Base-163.repo

# 4.使用下载好地repos文件替换成默认地repos文件
mv CentOS7-Base-163.repo CentOS-Base.repo

# 5.清理旧缓存数据，缓存新数据
yum clean all
yum makecache

# 测试
yum list | grep firefox
yum install firebox -y
```


## 九、克隆虚拟机

参见：[[VM与CentOS安装#四、克隆机器|完整克隆虚拟机]]

## 十、常见错误及解决方案

### 10.1 解析域名出错

报错信息：

```txt
[root@iamwufm network-scripts]# ping baidu.com
ping: baidu.com: 未知的名称或服务

或者出现

"Could not resolve host: ftp.sjtu.edu.cn; Unknown error"
```

解决方法：

```shell
# 1.resolv.conf添加dns
vi /etc/resolv.conf

# 添加如下内容
nameserver 8.8.8.8

# 2. 检查网卡配置有没有配置网关
vi /etc/sysconfig/network-scripts/ifcfg-ens33

# 添加如下内容
GATEWAY=192.168.204.2

# 3.重启网络服务
systemctl restart network
```

### 10.2 修改IP地址后可能会遇到的问题

1. 物理机能ping通虚拟机，但是虚拟机ping不通物理机，一般都是因为物理机的防火墙问题，把防火墙关闭即可。
2. 虚拟机能ping通物理机，但是虚拟机ping不通外网，一般都是因为DNS的设置有问题。
3. 虚拟机ping外网显示域名未知等信息，一般查看GATWAY和DNS设置是否正确。

## 十一、企业面试题

```shell
# 查看内存
top

# 查看磁盘存储情况
df -h

# 查看磁盘IO读写情况
yum install iotop
iotop
iotop -o # 查看输出比较高地磁盘读写程序

# 查看端口占用情况
netstat -tunlp | grep 端口号

# 查看进程
ps -aux
```