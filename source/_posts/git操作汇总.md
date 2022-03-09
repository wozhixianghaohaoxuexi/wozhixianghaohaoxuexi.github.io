---
title: git操作汇总
date: 2022-01-26 14:59:10
categories:
  - git
tags:
  - git
---

### 一、命令汇总

#### 1. git init
```powershell
# 初始化仓库
git init
```

#### 2. git pull
```powershell
# 从远程仓库拉取代码并合并到本地，可简写为 git pull 等同于 git fetch && git merge 
git pull <远程主机名> <远程分支名>:<本地分支名>
```

#### 3. git fetch

* git fetch 与 git pull 不同在于，git fetch 只是获取远程仓库的更改，不会自动进行合并操作

```powershell
# 获取远程仓库指定分支的更新
git fetch <远程主机名> <分支名>
# 获取远程仓库所有分支的更新
git fetch --all
```

#### 4. git add
```powershell
# 添加一个或多个文件修改文件至暂存区
git add xxx
# 不加参数默认为将修改操作的文件和未跟踪新添加的文件添加到git系统的暂存区，注意不包括删除
git add .
# -u 表示将已跟踪文件中的修改和删除的文件添加到暂存区，不包括新增加的文件，注意这些被删除的文件被加入到暂存区再被提交并推送到服务器的版本库之后这个文件就会从git系统中消失了
git add -u .
# -A 表示将所有的已跟踪的文件的修改与删除和新增的未跟踪的文件都添加到暂存区
git add -A .
```

#### 5. git commit
```powershell
# -m 参数表示可以直接输入后面的“message”，如果不加 -m参数，那么是不能直接输入message的，而是会调用一个编辑器一般是vim来让你输入这个message，message即是我们用来简要说明这次提交的语句
git commit -m "xxx"
# -a 参数可以将所有已跟踪文件中的执行修改或删除操作的文件都提交到本地仓库，即使它们没有经过git add添加到暂存区，注意: 新加的文件（即没有被git系统管理的文件）是不能被提交到本地仓库的，-am等同于-a -m
git commit -am "xxx"
```

#### 6. git push
```powershell
# 如果远程分支被省略，如上则表示将本地分支推送到与之存在追踪关系的远程分支（通常两者同名），如果该远程分支不存在，则会被新建
git push <远程主机名> <分支名>
# 如果当前分支与远程分支存在追踪关系，则本地分支和远程分支都可以省略，将当前分支推送到origin主机的对应分支
git push <远程主机名>
# 如果当前分支只有一个远程分支，那么主机名都可以省略，形如 git push，可以使用git branch -r ，查看远程的分支名
git push
```

#### 7. git branch
```powershell
# 新建本地分支，但不切换
git branch <分支名> 
# 查看本地分支
git branch
# 查看远程分支
git branch -r
# 查看本地和远程分支
git branch -a
# 删除本地分支
git branch -D <分支名>
# 重新命名分支
git branch -m <旧分支名> <新分支名>
```

#### 8. git tag
**V1.0为tag版本号**
```powershell
# 创建本地tag
git tag -a V1.0 -m '附加信息'
# 将本地tag同步到远程仓库
git push origin --tags
# 删除本地tag
git tag -d V1.0
# 同步删除线上tag
git push origin :refs/tags/V1.0  
# 查看tag
git tag  
# 查看tag详情
git show V1.0  
```

#### 9. git remote
```powershell
# 显示关联的远程仓库别名
git remote
# 显示关联的远程仓库信息，仓库别名 + 远程仓库 url
git remote -v
# 添加远程仓库关联，可关联多个远程（设置不同别名）
git remote add <仓库别名> <远程仓库 url>
# 删除远程仓库与本地仓库关联
git remote rm <仓库别名>
# 修改远程仓库名别名
git remote rename <旧仓库别名> <新仓库别名>
```

### 二、报错汇总
#### 1、Reinitialized existing Git repository

* 将当前目录下的.git文件夹删除后重新执行`git init`

#### 2、The current branch master has no upstream branch

* 没有将本地仓库分支与远程仓库分支关联
* 方法一：使用`git push --set-upstream origin master`命令（[最好不用](https://careerkarma.com/blog/git-no-upstream-branch/)）
* 方法二：使用`git push -u origin master`命令

#### 3、failed to push some refs to

* 远程仓库与本地仓库代码不一致，使用`git pull --rebase origin master`将远程仓库合并到本地仓库中
* --rebase的作用是取消本地仓库刚提交的commit，并将其合并至新的本地仓库中
* 再使用`git push -u origin master`将本地仓库提交至远程仓库

### 三、生成ssh公钥
执行`ssh-keygen -t rsa -C "xxxxx@xxxxx.com"`命令后三次回车，在**~/.ssh（windows为对应用户文件夹下的.ssh目录）**目录下会生成`id_rsa`（ssh私钥）和`id_rsa.pub`（ssh公钥）两个文件

> 注意：这里的 xxxxx@xxxxx.com 只是生成的 sshkey 的名称，并不约束或要求具体命名为某个邮箱。现网的大部分教程均讲解的使用邮箱生成，其一开始的初衷仅仅是为了便于辨识所以使用了邮箱。

* 补充：将ssh公钥添加至github/gitee后，可以使用`ssh -T git@github.com`或`ssh -T git@gitee.com`来测试ssh是否添加成功。首次使用需要确认并添加主机到本机SSH可信列表。若返回 Hi XXX! You've successfully authenticated, but Gitee.com does not provide shell access. 内容，则证明添加成功。


> 参考文档：
> [掘金：我在工作中是如何使用 git 的](https://juejin.cn/post/6974184935804534815)
