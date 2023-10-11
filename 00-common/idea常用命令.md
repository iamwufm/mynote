---
author: wufm
title: idea常用命令
rating: 6
time: 2023-09-21 周四
tags:
  - idea
  - maven
  - debug
代码:
---

## 一、查找

### 1.1 本页内

在本页查找：Ctrl + F

在本页替换：Ctrl + R

### 1.2 在整个项目内

查找：Ctrl + Shift+F

替换：Ctrl + Shift+R

### 1.3 全局（包括依赖）

按文件名搜索文件：Ctrl + Shift+N

**查看继承关系：Ctrl + H**

查找类或方法在哪里被使用：Alt+F7

搜索任何东西：Shift+Shift

按名称搜索类：Ctrl +N

## 二、重写方法

重写父类方法：Ctrl +O

生成构造器、get和set等：Alt+Insert

代码异常：Alt+Enter

格式化代码：Ctrl +Alt+L

优化导入的类和包：Ctrl +Alt+L

	查看接口实现类 或 跳转到方法实现处：Ctrl +Alt+B
## 三、Maven学习

参见 [[Maven自动化构建工具]]


## 四、调试debug

==F8（step Over）==：程序往下执行一行，如果这一行有方法不会进入方法。[[#^506b58]]

==F7（step into）==：程序往下执行一行，如果这一行有方法会进入方法内（自定义方法或三方库方法，JDK方法无法进入）。[[#^7a4aa8]]

Alt+Shift+F7（Force step into）：强制进入方法内，一般Step into进不去时可以使用

==Shift+F8（Step out）==:退出方法，跟F7（step into）配合使用

==F9（Resume Program）==：运行到下一个断点的地方。[[#^40f4f6]]

Alt+F9（Run to Cursor）运行到光标处，你可以将光标定位到你需要查看的那一行，然后使用这个功能，代码会运行至光标行，而不需要打断点。[[#^31dd21]]

![[Pasted image 20230921112837.png|500]]

==F8操作：== ^506b58

![[Pasted image 20230921104523.png|375]]

==F7操作：== ^7a4aa8

![[Pasted image 20230921105226.png|375]]

==F9操作：== ^40f4f6

![[Pasted image 20230921111634.png|375]]

==Alt+F9操作：== ^31dd21

![[Pasted image 20230921112227.png|375]]