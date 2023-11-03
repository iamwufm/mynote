---
author: wufm
title: Windows安装Java
rating: 4
time: 2023-10-28 周六
tags:
  - java
代码:
---

## 一、安装JDK

### 1.1 下载JDK应用程序

官网地址：[https://www.oracle.com/](https://www.oracle.com/)

建议安装1.8，目前官网1.8收费。

下载exe应用程序，双击安装即可。
### 1.2 配置环境变量

新建JAVA_HOME变量，在PATH变量最后面添加一条变量值。如下图所示。

配置完成后，通过`win+r`调出【运行】弹窗，输入`cmd`之后打开【命令提示符】。输入`java -version`，出现版本号表示安装成功。

![[图片3.png]]

![[图片4.png]]

## 二、安装maven

### 2.1 下载maven压缩包
官网下载zip包：[https://maven.apache.org/download.cgi](https://maven.apache.org/download.cgi) ^07b760

jdk1.8建议使用maven的版本是3.3.9
解压即安装。

从官方获取jar包：[中央仓库](https://mvnrepository.com/)

### 2.2 配置环境变量

跟 [jdk环境变量](#1.2配置环境变量)一样。
配置完成后，通过`win+r`调出【运行】弹窗，输入`cmd`之后打开【命令提示符】。
输入`mvn -v`，出现版本号表示安装成功。

### 2.3配置仓库地址和镜像

1.修改仓库地址
在电脑建一个文件，命名为repository。
进入路径为../apache-maven-3.9.0/conf，然后打开settings.xml文件。

```xml
<!-- 找到如下标签，并复制一行，并修改为自己的仓库地址 -->
 <localRepository>/path/to/local/repo</localRepository> 
```

2.修改镜像文件

把远程仓库改为阿里仓库镜像

（1）方法一：配置单一库

```xml
<!-- 找到<mirrors></mirrors>标签，复制如下内容到标签里面-->
<!-- 单一库，用这个就行了。 这个虽然也可以配置多个，但是，它是第一个镜像挂了，才会找第二个。 不是多仓库的意思 -->
<mirror>
  <id>nexus</id>
  <mirrorOf>central</mirrorOf>
  <url>https://maven.aliyun.com/repository/public</url>
</mirror>

<mirror>
  <id>huaweiyun</id>
  <mirrorOf>central</mirrorOf>
  <url>hhttps://repo.huaweicloud.com/repository/maven</url>
</mirror>
```

（2）配置多仓库

```xml
<!-- 找到<profiles></profiles>标签，复制如下内容到标签里面,mirror内容可以注释掉--> 
    <!-- maven配置多仓库。使用顺序是倒序的，所以最流畅的写在最下面。 -->
    <!-- repo1仓库 -->
    <profile>
      <id>repo1</id>
      <repositories>
        <repository>
          <id>repo1</id>
          <url>https://repo1.maven.org/maven2</url>
           <!-- 能下载正式版本 -->
          <releases><enabled>true</enabled></releases>
          <!-- 能下载快照版本 -->
          <snapshots><enabled>true</enabled></snapshots>
        </repository>
      </repositories>
    </profile>
    <!-- 清华大学 -->
    <profile>
      <id>repo</id>
      <repositories>
        <repository>
          <id>repo</id>
          <url>https://repo.maven.apache.org/maven2/</url>
          <releases><enabled>true</enabled></releases>
          <snapshots><enabled>true</enabled><updatePolicy>always</updatePolicy></snapshots>
        </repository>
      </repositories>
    </profile>
     <!-- 华为云 -->
    <profile>
      <id>huaweicloud</id>
      <repositories>
        <repository>
          <id>huaweicloud</id>
          <url>https://repo.huaweicloud.com/repository/maven/</url>
          <releases><enabled>true</enabled></releases>
          <snapshots><enabled>true</enabled><updatePolicy>always</updatePolicy></snapshots>
        </repository>
      </repositories>
    </profile>
    <!-- 阿里云 -->
    <profile>
      <id>aliyun</id>
      <repositories>
        <repository>
          <id>aliyun</id>
          <url>https://maven.aliyun.com/repository/public</url>
          <releases><enabled>true</enabled></releases>
          <snapshots><enabled>true</enabled><updatePolicy>always</updatePolicy></snapshots>
        </repository>
      </repositories>
    </profile>
  </profiles>
```

```xml
<!-- 激活仓库 -->
<activeProfiles>
    <activeProfile>repo1</activeProfile>
    <activeProfile>repo</activeProfile>
    <activeProfile>huaweicloud</activeProfile>
    <activeProfile>aliyun</activeProfile>
</activeProfiles>
```

国内镜像仓库

```txt
阿里云：https://maven.aliyun.com/repository/public
华为云：https://repo.huaweicloud.com/repository/maven/
网易：http://maven.netease.com/repository/public/
tencent：https://mirrors.cloud.tencent.com/repository/maven/
中国科学院开源协会：http://maven.opencas.cn/maven/
清华大学：https://repo.maven.apache.org/maven2/
中国科技大学：http://mirrors.ustc.edu.cn/maven/maven2/
东软信息学院：https://mirrors.neusoft.edu.cn/maven2/
中央仓库：https://repo1.maven.org/maven2/
```
## 三、安装编译器

Eclipse和IDEA是目前比较流行的编辑器。Eclipse是免费的，idea是收费的。

### 3.1 安装idea2018
#### 3.1.1 下载idea应用程序

idea下载官网：[IntelliJ IDEA – 领先的 Java 和 Kotlin IDE (jetbrains.com.cn)](https://www.jetbrains.com.cn/idea/promo/?bd_vid=8517581315287908800)
专业版是不免费的，建议网上查找破解的方法。
双击安装。

#### 3.1.2 idea破解
把JetbrainsCrack-release-enc.jar放在idea的bin目录下，然后在bin目录下两个文件（idea.exe.vmoptions、idea64.exe.vmoptions）追加以下一句

```txt
-javaagent:C:\ruanjian\idea\IDEA2018.2.5\bin\JetbrainsCrack-release-enc.jar
```

打开idea，复制下面的代码到Activation code里面

```txt
ThisCrackLicenseId-{
“licenseId”:“ThisCrackLicenseId”,
“licenseeName”:“username”,
“assigneeName”:"",
“assigneeEmail”:“XXXX@163.com”,
“licenseRestriction”:“For This Crack, Only Test! Please support genuine!!!”,
“checkConcurrentUse”:false,
“products”:[
{“code”:“II”,“paidUpTo”:“2019-12-31”},
{“code”:“DM”,“paidUpTo”:“2019-12-31”},
{“code”:“AC”,“paidUpTo”:“2019-12-31”},
{“code”:“RS0”,“paidUpTo”:“2019-12-31”},
{“code”:“WS”,“paidUpTo”:“2019-12-31”},
{“code”:“DPN”,“paidUpTo”:“2019-12-31”},
{“code”:“RC”,“paidUpTo”:“2019-12-31”},
{“code”:“PS”,“paidUpTo”:“2019-12-31”},
{“code”:“DC”,“paidUpTo”:“2019-12-31”},
{“code”:“RM”,“paidUpTo”:“2019-12-31”},
{“code”:“CL”,“paidUpTo”:“2019-12-31”},
{“code”:“PC”,“paidUpTo”:“2019-12-31”}
],
“hash”:“2911276/0”,
“gracePeriodDays”:7,
“autoProlongated”:false}
```

#### 3.1.3配置jdk

![[图片14.png]]


![[图片15.png]]


![[Pasted image 20230907092416.png]]

![[Pasted image 20230907092518.png]]

![[Pasted image 20230907092255.png]]
#### 3.1.4配置maven

![[图片17.png]]

![[图片18.png]]

### 3.2 安装Eclipse

#### 3.2.1 下载Eclipse包

官网地址：[Eclipse Downloads | The Eclipse Foundation](https://www.eclipse.org/downloads/)

#### 3.2.2 安装Eclipse

解压后，将eclipse中的执行程序 点右键  发送到桌面快捷方式
然后双击快捷方式启动即可

#### 3.2.3 配置jdk

![[Pasted image 20230907091255.png]]


![[Pasted image 20230907090930.png]]

![[Pasted image 20230907090950.png]]

![[Pasted image 20230907091035.png]]

![[Pasted image 20230907091110.png]]

![[Pasted image 20230907091141.png]]
#### 3.2.4 安装maven

![[Pasted image 20230907091328.png]]

![[Pasted image 20230907091353.png]]

![[Pasted image 20230907091452.png]]

![[Pasted image 20230907091407.png]]

### 3.3 安装idea2020

见[[Windows安装Python#3.1.2 pycharm破解]]