---
author: wufm
title: Scala安装
rating: 2
time: 2023-10-28 周六
tags:
  - Scala
代码:
---
## 一、环境准备

需要JDK1.8+，见[[Windows安装Java#一、安装JDK]]

## 二、安装Scala

### 2.1 方法一：在IDEA软件工程中的pom.xml文件指定依赖即可

```xml
<dependency>  
    <groupId>org.scala-lang</groupId>  
    <artifactId>scala-library</artifactId>  
    <version>2.12.18</version>  
</dependency>
```

### 2.2 方法二：配置环境变量

从官网下载安装包zip，解压后配置环境变量即可

官网：[The Scala Programming Language (scala-lang.org)](https://www.scala-lang.org/)

![[Pasted image 20231028125941.png|375]]

配置环境变量参考[[Windows安装Java#1.2 配置环境变量]]

配置完成后，通过`win+r`调出【运行】弹窗，输入`cmd`之后打开【命令提示符】。输入`scala`，就可以写代码了。

![[Pasted image 20231028130127.png]]
## 三、IDEA安装Scala插件

在IDEA软件中
File -> Settings -> Plugins -> 选择 Scala 插件 -> OK -> 重启 IDEA

![[Pasted image 20231028121708.png|275]]

![[Pasted image 20231028121815.png|275]]

## 四、建立Scala工程

建立一个scala目录，把scala目录设置为Soures Root

![[Pasted image 20231028122034.png|400]]

![[Pasted image 20231028122136.png|400]]

![[Pasted image 20231028122319.png|550]]

![[Pasted image 20231028122347.png|375]]


![[Pasted image 20231028122446.png|400]]