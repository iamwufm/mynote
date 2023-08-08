
---
author: wufm
title: git常用命名
rating: 5
time: 2023-08-08 周二
tags:
 - git
 - 数据仓库
---



```dataview
list
from #git
```

## 一、克隆远程仓库

```shell
# 克隆远程仓库（分支名不写，默认主分支）
git clone 远程仓库地址 分支名

# 在本地文件夹中建立仓库
git init
```

## 二、添加到本地缓存区

```shell
# 查看哪些修改了
git status
# 将文件添加到本地缓存
# 1. 所有
git add .
# 2. 指定某个文件
git add 某个文件名称
```

## 三、提交

```shell
git commit -m "描述性文字"
# 或者
git commit 某个文件名称 -m "描述性文字"
```

## 四、回滚

查看提交的版本

```shell
git log
# 或者
git log -数量
```

回滚到指定版本

```shell
git reset --hard log版本编码
```


## 五、 拉取或推送

```shell
# 把内容推送到远程仓库（分支名不写，默认主分支）
git push 远程仓库地址 分支名

# 从远程仓库拉取内容到本地（分支名不写，默认主分支）
git pull 远程仓库地址 分支名
```

## 六、分支管理

### 6.1查看所有分支

```shell
git branch -a
```

### 6.2创建分支

```shell
git checkout -b 分支名称
```

### 6.3切换分支

```shell
git checkout 分支名称
```

### 6.4合并分支

比如需要把某个分支合并到主分支，先切换到主分支

```shell
git merge 分支名称
```

### 6.5分支有哪些并入分支

```shell
git branch --merged
```

### 6.6删除分支

```shell
git branch -d 分支名称
```





