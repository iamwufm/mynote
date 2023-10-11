---
author: wufm
title: 在linux上安装mysql
rating: 5
time: 2023-09-28 周四
tags:
  - mysql
代码:
---
## 一、软件下载

mysql官网下载：[MySQL :: MySQL Product Archives](https://downloads.mysql.com/archives/)

![[Pasted image 20230928142441.png|400]]

![[Pasted image 20230928150330.png]]
## 二、mysql安装

hadoop用户操作

```shell
# 把安装包上传到以下目录（用rz命令或者alt+p上传）
cd /opt/software/
rz

# 解压
mkdir mysql
tar -xf MySQL-5.6.26-1.linux_glibc2.5.x86_64.rpm-bundle.tar -C ./mysql

cd mysql
```

```shell
# 查看mysql是否安装，如果安装了，卸载mysql
rpm -qa | grep mysql

# 卸载
rpm -e --nodeps mysql名称
```
### 2.1 安装mysql服务器

```shell
# 1.安装mysql服务器。
sudo rpm -ivh MySQL-server-5.6.26-1.linux_glibc2.5.x86_64.rpm

# 2.查看密码
sudo cat /root/.mysql_secret

# 结果
4w3rF98AzOHK5ru_

# 3.启动mysql的服务端, sudo service mysql status
sudo service mysql start
```

步骤一成功出现的结果：

```shell

A RANDOM PASSWORD HAS BEEN SET FOR THE MySQL root USER !
You will find that password in '/root/.mysql_secret'.

You must change that password on your first connect,
no other statement but 'SET PASSWORD' will be accepted.
See the manual for the semantics of the 'password expired' flag.

Also, the account for the anonymous user has been removed.

In addition, you can run:

  /usr/bin/mysql_secure_installation
```

```txt
初始密码：/root/.mysql_secret  
改密码脚本：/usr/bin/mysql_secure_installation
```
### 2.2 安装mysql客户端

```shell
sudo rpm -ivh MySQL-client-5.6.26-1.linux_glibc2.5.x86_64.rpm
```

### 2.3 在window安装mysql客户端

可不安装。

可以在windows上用Navicat。
### 2.4 修改密码（密码设置为：1234）

（1）方式一：

```shell
/usr/bin/mysql_secure_installation
```

![[Pasted image 20230928163545.png|400]]

（2）方式二：

```shell
mysql -uroot -p4w3rF98AzOHK5ru_

# 重新设置密码
SET PASSWORD=PASSWORD('1234');

# 退出
exit
```

### 2.5 让mysql可以远程登录访问

最直接测试方法：从windows上用Navicat去连接，能连，则可以，不能连，则要去mysql的机器上用命令行客户端进行授权：

在mysql的机器上,启动命令行客户端：

```shell
mysql -uroot -p1234

# grant给root使用root密码从任何主机连接到mysql服务器
grant all privileges on *.* to 'root'@'%' identified by '1234' with grant option;

# 刷新
flush privileges;
```

## 三、客户端命令行操作



## 四、安装常见错误

### 4.1 安装服务器错误

（1）包冲突
```txt
准备中...                          ################################# [100%]
        file /usr/share/mysql/charsets/README from install of MySQL-server-5.6.26-1.linux_glibc2.5.x86_64 conflicts with file from package mariadb-libs-1:5.5.68-1.el7.x86_64
        file /usr/share/mysql/czech/errmsg.sys from install of MySQL-server-5.6.26-1.linux_glibc2.5.x86_64 conflicts with file from package mariadb-libs-1:5.5.68-1.el7.x86_64
        file /usr/share/mysql/danish/errmsg.sys from install of MySQL-server-5.6.26-1.linux_glibc2.5.x86_64 conflicts with file from package mariadb-libs-1:5.5.68-1.el7.x86_64
```

解决方法：

```shell
# 查看是否安装mariadb
rpm -qa | grep mariadb
 
# 结果
mariadb-libs-5.5.68-1.el7.x86_64
 
# 2.卸载
sudo rpm -e --nodeps mariadb-libs-5.5.68-1.el7.x86_64
```

（2）依赖报错：缺perl

```shell
sudo yum install perl
```

（3）包冲突 conflict with

```shell
# 移除老版本的冲突包：mysql-libs-5.1.73-3.el6_5.x86_64
rpm -e --nodeps mysql-libs-5.1.73-3.el6_5.x86_64 
```