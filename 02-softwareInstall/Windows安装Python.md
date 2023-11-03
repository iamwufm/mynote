---
author: wufm
title: Windows安装Python
rating: 2
time: 2023-10-28 周六
tags:
  - Python
代码:
---

## 一、下载python应用程序

下载地址：[https://www.python.org/](https://www.python.org/)

根据电脑操作系统下载相应的应用程序

双击安装即可。可以在安装过程勾选配置环境变量。如果没有可以参考[[Windows安装Java#1.2 配置环境变量]]的方法

## 二、更换镜像源

**方法1：（永久更换）**
通过`win+r`调出【运行】弹窗，输入`cmd`之后打开【命令提示符】。
该条语句将pip的下载源永久更改为某个镜像站，这里以清华大学开源镜像站为例：

```shell
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple/
```

**方法2：（永久更换）**
windows环境下，在用户目录中创建一个文件夹，该文件夹的命名为pip；在该pip文件夹中新建一个文件pip.ini，pip.ini的内容如下：（完整路径：C:\Users\用户名\pip\pip.ini）

```txt
[global]
index-url=https://pypi.tuna.tsinghua.edu.cn/simple
[install]  
trusted-host=pypi.tuna.tsinghua.edu.cn
disable-pip-version-check = true  
timeout = 6000
```

**方法3：（临时更换**）
在安装库的时候，临时需要用到某个镜像，这里以清华大学镜像为例下载pandas库：

```shell
pip install pandas -i https://pypi.tuna.tsinghua.edu.cn/simple/
```

国内镜像源
```txt
豆瓣：http://pypi.douban.com/simple/
阿里云：http://mirrors.aliyun.com/pypi/simple/
华为云：https://repo.huaweicloud.com/repository/pypi/simple
清华大学：https://pypi.tuna.tsinghua.edu.cn/simple
中科大：https://pypi.mirrors.ustc.edu.cn/simple/
```

## 三、安装编译器

### 3.1 安装pycharm

#### 3.1.1 下载pycharm应用程序

pycharm下载官网：[Download PyCharm: Python IDE for Professional Developers by JetBrains](https://www.jetbrains.com/pycharm/download/?section=windows)

专业版是不免费的，建议网上查找破解的方法。
双击安装。

本次下载的版本是：pycharm-professional-2020.1.3
#### 3.1.2 pycharm破解

先下载 jetbrains-agent.jar，激活的时候要用！

![[Pasted image 20230929134924.png|400]]


![[Pasted image 20230929135000.png|400]]


![[Pasted image 20230929135010.png|400]]

==如果你的 pycharm 拖不上去这个文件，可以点 File – setting – plugins，然后 install plugin from disk，再选择 jetbrains-agent.jar 安装==

![[Pasted image 20230929135118.png|400]]


![[Pasted image 20230929135137.png|400]]


![[Pasted image 20230929135147.png|400]]

