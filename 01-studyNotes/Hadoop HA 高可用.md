---
author: wufm
title: Hadoop HA 高可用
rating: 5
time: 2023-09-26 周二
tags:
  - Hadoop
  - Hadoop高可用
代码:
---
## 一、概述

所谓 HA（High Availablity），即高可用（**7 * 24 小时不中断服务**）。

实现高可用最关键的策略是==消除单点故障==。HA 严格来说应该分成各个组件的 HA机制：==HDFS 的 HA 和 YARN 的 HA==。

NameNode/ResourceManager主要在以下两个方面影响 HDFS/Yarn 集群：
- 机器发生意外，如宕机，集群将无法使用，直到管理员重启
- 机器需要升级，包括软件、硬件升级，此时集群也将无法使用

HDFS HA 功能通过配置多个 NameNodes(Active/Standby)实现在集群中对 NameNode 的热备来解决上述问题。如果出现故障，如机器崩溃或机器需要升级维护，这时可通过此种方式将 NameNode 很快的切换到另外一台机器。

YARN HA也是如此。
## 二、解决方案

### 2.1 hdfs高可用

![[hdfs高可用解决方案.excalidraw|600]]

### 2.2 yarn高可用

![[yarn高可用解决方案.excalidraw]]

## 三、Hadoop HA 高可用安装

[[Hadoop HA 高可用安装]]
