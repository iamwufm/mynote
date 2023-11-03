---
author: wufm
title: Maven自动化构建工具
rating: 5
time: 2023-08-31 周四
tags:
 - maven
---

## 一、Maven简介

### 1.1 Maven概述

**maven是一个项目构建工具。**

maven的作用：
1. 管理依赖：jar管理、下载、版本
2. 构建项目：完成项目代码的编译、测试、打包等。可参见[[#2.5 生命周期和常用命令]]

maven解决的问题：
1. 可以管理jar文件
2. 自动下载jar和它的文档、源代码
3. 管理jar直接的依赖，比如a.jar依赖b.jar，会自动下载b.jar
4. 帮你编译程序，把java编译成class
5. 帮你测试你的代码是否正确
6. 帮你打包文件，形成jar文件或者war文件
7. 帮你部署项目

### 1.2 Maven使用方式

1. 独立使用maven：使用maven的各种命令，完成编译、测试、运行、打包、部署等
**2. 结合开发工具使用：一般在idea中使用maven（简单便捷，不需要记命令）**
### 1.3 安装Maven环境

参考[[Windows安装Java#二、安装maven]]

## 二、Maven核心概念

### 2.1 仓库

仓库：是存放东西的，存放maven使用jar和我们项目使用的jar。

仓库的分类
- 本地仓库：个人计算机上的文件夹，可以通过maven安装目录/conf/settings.xml设置。默认在${user.dir}/.m2/repository
- 远程仓库：在互联网上的，使用网络才能使用的仓库
	- 中央仓库：最权威的，所有的开发人员都共享使用的一个集中仓库（地址：[中央仓库](https://mvnrepository.com/)）
	- 中央仓库镜像：就是中央仓库的备份，如阿里云镜像
	- 私服：在公司的内部，局域网中使用，不对外使用

仓库的使用不需要人为参与

开发人员使用的jar包-->本地仓库-->私服-->镜像-->中央仓库

### 2.2 生命周期和常用命令

maven生命周期就是maven构建项目的过程，清理、编译、测试、报告、打包、安装和部署。
maven的命令：通过命令，完成maven的生命周期的执行。

| 命令             | 功能         | 详细说明                                                                                                       |
| ---------------- | ------------ | -------------------------------------------------------------------------------------------------------------- |
| mvn clean        | 清理         | 会删除原来编译和测试的目录，及target目录                                                                       |
| mvn compile      | 编译主程序   | 会在当前目录下生成一个target，里面存放编译主程序之后生成的字节码文件                                           |
| mvn test-compile | 编译测试程序 | 会在当前目录下生成一个target，里面存放编译测试程序之后生成的字节码文件                                         |
| mvn test         | 测试         | 会生成一个目录surefire-reports,保存测试文件                                                                    |
| mvn package      | 打包主程序   | 会编译、编译测试、测试、并按照pom.xml配置把主程序打包生成jar包或war包                                          |
| mvn install      | 安装主程序   | 会把本工程打包，并且按照本工程的坐标保存在本地仓库中                                                           |
| mvn deploy       | 部署主程序   | 会把本工程打包，并且按照本工程的坐标保存在本地仓库中，并且还会保存到 私服仓库中。还会自动把项目部署到web容器中 |

==注意：执行以上命令必须在命令行进入pom.xml所在的目录！==

执行某个命令，会先执行前面的命令。比如执行mvn compile会先执行mvn clean，再执行mvn compile。

### 2.3 约定目录结构

- 工程名（根目录）
	- src（源代码）
		- **main（主程序）**
			- java（程序包或包中的java文件）
			- resources（java程序要使用的配置文件）
		- test（放测试程序代码和文件的，可以没有）
			- java（测试程序包和包中的java文件）
			- resource（测试java程序要使用的配置文件）
	- **pom.xml（核心文件，必须有）**
### 2.4 pom文件（重点）

#### 2.4.1 坐标

坐标在仓库中是唯一的。由三个元素组成，**通过坐标，可以唯一定位和找到一个依赖构件。**

| 名称      | 说明                                                                                                      |
| --------- | --------------------------------------------------------------------------------------------------------- |
| groupId   | 组织id,一般是公司域名的倒写。格式为：1.域名倒写，如com.bai 2.域名倒写+项目名，如com.baidu.appolo          |
| artfactId | 项目名称，也是模块名称。对应groupId中项目的项目                                                           |
| version   | 项目的版本号。如果项目还在开发中，是不稳定版本，通常在版本后带-SNAPSHOT。version使用三位数字标识，如1.1.0 |

```xml
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.33</version>
</dependency>
```

依赖包存放路径：本地仓库目录/mysql/mysql-connector-java/8.0.33
#### 2.4.2 依赖

dependencies和dependencie，相当于java代码中的import。

项目中要使用的各种资源说明，比如需要mysql驱动

```xml
<dependencies>
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.33</version>
</dependency>
</dependencies>
```

#### 2.4.3 常用插件

maven插件：maven命令执行时，真正完成功能的插件，就是一些jar文件

**后续再研究**

插件是在 pom.xml 中使用 plugins 元素定义的。

```xml
<!-- java编译插件 -->
<plugin>
	<groupId>org.apache.maven.plugins</groupId>
	<artifactId>maven-compiler-plugin</artifactId>
	<version>3.6.0</version>
	<configuration>
		<source>1.8</source>
		<target>1.8</target>
		<encoding>UTF-8</encoding>
	</configuration>
</plugin>
```