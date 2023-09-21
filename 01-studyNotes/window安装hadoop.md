
## 一、安装包下载

1. hadoop安装包
hadoop安装包跟linux上安装包一样。可以从官网或镜像文件下载安装。
Hadoop包下载地址：[Apache Hadoop官网](https://hadoop.apache.org/releases.html)
阿里云镜像下载：[阿里云镜像](https://mirrors.aliyun.com/apache/hadoop/core/)

2. hadoop的window依赖包
从github上下载windows依赖文件。[hadoop for windows](https://github.com/cdarlint/winutils)

## 二、安装

解压hadoop安装包即可。把接近或者同版本hadoop的window依赖包的bin目录下的文件拷贝到hadoop解压后的bin目录下。

![[Pasted image 20230901164148.png]]


把bin目录下的hadoop.ddl拷贝到C:\Windows\System32

## 三、环境变量配置

### 3.1 配置hadoop环境变量

![[Pasted image 20230901165013.png]]

### 3.2 配置jdk路径

进去{hadoop安装目录}/etc/hadoop

修改hadoop-env.sh

```sh
# 添加jdk路径
export JAVA_HOME=C:\ruanjian\jdk1.8.0_231
```
