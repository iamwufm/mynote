---
author: wufm
title: windows上安装hive执行可视化工具
rating: 1
time: 2023-10-11 周三
tags:
  - hive执行计划可视化工具
代码:
---
## 一、windows安装npm

要在Windows上安装npm，按照以下步骤操作：

首先，确保您已经在计算机上安装了Node.js。可以从Node.js官方网站（[Node.js (nodejs.org)](https://nodejs.org/en)）下载并安装Node.js。

双击安装Node.js后，打开命令提示符（Command Prompt）或者PowerShell。

cmd后输入以下命令来验证Node.js和npm的安装情况：

```shell
#如果正确安装了Node.js和npm,会看到它们的版本号。
node -v
npm -v
```

![[Pasted image 20231011215318.png|275]]

## 二、下载可视化工具 hive-query-plan-viz

上gitte获取拉取项目：[https://gitee.com/itlbo/hive-query-plan-viz](https://gitee.com/itlbo/hive-query-plan-viz)

解压文件后进入目录输入cmd，然后按回车
![[Pasted image 20231011220044.png|375]]

```shell
# 下载依赖包
npm install
```

![[Pasted image 20231011215748.png]]

## 三、启动服务

进入安装目录，输入cmd，按回车

```shell
set NODE_OPTIONS=--openssl-legacy-provider
# 启动服务
npm run serve
```

![[Pasted image 20231011220348.png|600]]

网页访问：localhost:8080

explain formatted ‘sql语句’；  
把结果复制到框里面

```sql
explain formatted
select od.user_id,od.product_id,od.province_id,product.product_name,province.province_name
from order_detail od
join product_info product on od.product_id = product.id
join province_info province on od.province_id = province.id;
```


![[Pasted image 20231011220714.png]]