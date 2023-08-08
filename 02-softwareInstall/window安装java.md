## 一、安装jdk

### 1.1下载jdk应用程序
官网地址：[https://www.oracle.com/](https://www.oracle.com/)
建议安装1.8，目前官网1.8收费。
下载exe应用程序，双击安装即可。
### 1.2配置环境变量
新建JAVA_HOME变量，在PATH变量最后面添加一条变量值。如下图所示。

配置完成后，通过`win+r`调出【运行】弹窗，输入`cmd`之后打开【命令提示符】。
输入`java -version`，出现版本号表示安装成功。
![[图片3.png]]

![[图片4.png]]

## 二、安装maven

### 2.1下载maven压缩包
官网下载zip包：[https://maven.apache.org/download.cgi](https://maven.apache.org/download.cgi) ^07b760

解压即安装。

### 2.2配置环境变量

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

```xml
<!-- 找到<mirrors></mirrors>标签，复制如下内容到标签里面-->
<mirror>  
     <id>alimaven</id>  
     <name>aliyun maven</name>  
     <url>http://maven.aliyun.com/nexus/content/groups/public/</url>  
     <mirrorOf>central</mirrorOf>        
</mirror>
```

## 三、安装idea

### 3.1下载idea应用程序
idea下载官网：[IntelliJ IDEA – 领先的 Java 和 Kotlin IDE (jetbrains.com.cn)](https://www.jetbrains.com.cn/idea/promo/?bd_vid=8517581315287908800)
专业版是不免费的，建议网上查找破解的专业版。
双击安装。

### 3.2idea破解
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

### 3.3配置jdk

![[图片14.png]]


![[图片15.png]]


![[图片16.png]]

### 3.4配置maven

![[图片17.png]]

![[图片18.png]]