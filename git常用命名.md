## 1. 克隆远程仓库

```shell
# 克隆远程仓库
git clone 远程仓库地址

# 在本地文件夹中建立仓库
git init
```

## 2. 添加到本地缓存区

```shell
# 查看哪些修改了
git status

# 将文件添加到本地缓存
# 1. 所有
git add .
# 2. 指定某个文件
git add 某个文件名称
```

## 3. 提交

```mark
git commit -m "描述性文字"
或者
git commit 某个文件名称 -m "描述性文字"
```

## 4. 回滚

查看提交的版本

```markdown
git log
或者
git log -数量
```

回滚到指定版本（log版本号如下图圈红部分）

```markdown
git reset --hard log版本编码
```

![image-20230508110225766](images/image-20230508110225766.png)

## 5. 拉取或推送

```markdown
-- 把内容推送到远程仓库（分知名不写，默认主分支）
git push 远程仓库地址 分支名

-- 从远程仓库拉取内容到本地（分知名不写，默认主分支）
git pull 远程仓库地址 分支名
```

## 6. 分支管理

### 6.1 查看所有分支

```markdown
git branch -a
```

### 6.2 创建分支

```markdown
git checkout -b 分支名称
```

### 6.3 切换分支

```markdown
git checkout 分支名称
```

### 6.4 合并分支

比如我需要把某个分支合并到主分支，先切换到主分支

```markdown
git merge 分支名称
```

### 6.5 查看哪些分支已被并入当前分支

```markdown
git branch --merged
```

### 6.6 删除分支

```markdown
git branch -d 分支名称
```





