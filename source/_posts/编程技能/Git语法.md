---
title: Git语法
tags: [git]
categories: 编程技能
date: 2023-03-01 19:20:00
---
# Git语法

## 教程

[B站教程](https://www.bilibili.com/video/BV1vy4y1s7k6?p=1)

[Toc]

## 基础的语法

### 本地库

#### 设置

- git config user.name	查看用户名
- git config user.email	查看邮箱地址
- git config --global user.name “username”	修改用户名
- git config --global user.email “email”	修改邮箱地址
- git config -l    查看Git全局设置信息

#### 创建版本库

- git init	把当前目录变成Git可以管理的仓库，会在当前目录下生成.git目录如果.git目录是藏隐的，可用 ls -ah 命令显示

#### 查看仓库状态

- git status	查看当前仓库的状态
- git diff	工作区(workdict)和暂存区(stage)的比较
- git diff commit_id	工作区(workdict)和指定版本的比较
- git diff commit_id – file	工作区(workdict)和指定版本中的指定文件的比较
- git diff --cached	暂存区(stage)和分支(master)的比较

#### 添加文件

- git add file	把文件添加到仓库(暂存区
- git commit -m “comments”	把文件提交到仓库(分支)，-m后面是备注注释
- git add .	添加目录下的所有文件
- git add folder/	添加目录下的指定文件夹

#### 查看日志

- git log	查看提交历
- git log -pretty=oneline	查看提交历史，并使内容单行显示
- git reflog	查看命令历史

#### 删除文件

- rm file	删除工作区文件，不提交到暂存区
- git rm file	删除工作区文件，自动提交到暂存区

#### 版本回退

- git reset --hard commit_id	回退到指定版本，版本号没必要写全，前几位就可以了
- git reset commit_id file	指定文件回退到指定版本
- git reset	用来撤销所有暂存区域文件
- git reset – file	用来撤销最后一次git add file

### 远程库

#### 关联远程库

- git remote add origin xxx	关联远程库，远程库的名字就是origin这是Git默认的叫法，也可以改成别的
- 从远程仓库克隆
- git clone xxx	Git支持多种协议默认的git://使用ssh，也可以使用https等其他协议
- git remote rm origin     删除名字为“origin“关联远程库
- git remote    查看当前的远程仓库
- git remote show <远程仓库>    查看某个远程仓库的详细信息

#### 推送

- git push origin master	将本地库推送至远程库

#### 抓取

- git pull	抓取
##### 拉取常见的问题

- 拉起出现fatal，可能是因为远程库和本地的内容存在出入，需要将本地的内容提交（commit）以后在开始pull
#### 分支

- git branch dev	创建分支dev
- git branch -d dev	删除分支dev
- git checkout dev	切换到分支dev
- git checkout -b dev	加上-b参数表示创建并切换dev分支
- git branch	查看当前分支，会列出所有分支，当前分支前面会标示*号
- git merge dev	合并指定分支到当前分支

#### 分支策略

- 在实际开发中，我们应该按照几个基本原则进行分支管理
- master分支应该是非常稳定的，仅用来发布新版本，平时不能在上面干活
- 干活都在dev分支上，dev分支是不稳定的，到某个时候，比如1.0版本发布时，再把dev分支合并到master上，在master分支发布1.0版本
- 每个人都在dev分支上干活，每个人都有自己的分支，时不时地往dev分支上合并
![](https://img-blog.csdnimg.cn/20201216171821903.png#pic_center#id=gMnNX&originHeight=125&originWidth=498&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)

#### Bug分支

##### 为什么要用到Bug分支

当线上(master)出现Bug需要修复，而手上的工作(dev)只进行了一半不想提交,可以将此时的工作区储藏起来；跳转到master分支,通过一个新的临时分支来修复，修复后合并分支，然后将临时分支删除。然后在跳转到dev分支，将之前储藏的工作区恢复，继续工作

##### 代码

- git stash	把当前工作现场"储藏"起来，等以后恢复现场后继续工作
- git stash list    	查看"储藏"的工作现场
- git stash apply       恢复但stash内容并不删除，要用git stash drop来删除
- git stash drop     删除stash内容
- git stash pop     恢复的同时把stash内容也删了

#### Feature分支

##### 为什么要用到Feature分支

- 软件开发中，总有无穷无尽的新功能要不断添加进来。
- 在添加一个新功能的时候，你肯定不希望因为一些实验性质的代码，把主分支搞乱了，所以，每添加一个新功能，最好新建一个feature分支，在上面开发，完成后，合并，最后，删除该feature分支。

##### 代码

- git branch -D feature-vulcan	强行删除没有合并的分支
- 开发一个新feature，最好新建一个分支；
- 如果要丢弃一个没有被合并过的分支，可以通过git branch -D 强行删除。

#### 标签管理

- git tag v1.0	创建标签，默认标签是打在最新提交的commit上的即HEAD
- git tag v1.0 commit_id	给指定的commit_id创建标签
- git tag -a v1.0 -m “Comments” commit_id	创建带有说明的标签
- git tag	查询所有标签，按字母排序
- git tag show v1.0	查看标签信息
- git tag -d v1.0	删除标签
- git push origin v1.0	推送某个标签到远程
- git push origin --tags	推送全部尚未推送到远程的本地标签
- git tag -d v1.0	本地删除
- git push origin :refs/tags/v1.0	本地删除
- 如果标签已经推送到远程，需要先从本地删除，再从远程删除

#### 多人协作

- git remote	查看远程库的信息
- git remote -v	显示远程库更详细的信息
- git remote rm origin	删除关联的origin远程库
- git fetch origin	查看远程库的信息
- git diff branch origin/branch	查看远程库分支与本地分支的对比
- git diff branchA branchB	查看本地分支的对比

#### 推送分支

- git push origin master	把本地分支上的所有本地提交推送到远程库对应的远程分支上
- git push origin master -f	表示舍弃线上的文件，强制推送
- master分支是主分支，因此要时刻与远程同步
- dev分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步
- bug分支只用于在本地修复bug，就没必要推到远程了
- feature分支是否推到远程，取决于你是否和你的小伙伴合作在上面开发
- 抓取分支
- git pull	抓取分支
- git branch --set-upstream dev origin/dev	抓取必须指定本地分支与远程分支的链接
- git checkout -b dev origin/dev	创建远程origin的dev分支到本地
- 


#### 多人协作的工作模式通常是这样

- 首先，可以试图用git push origin branch-name推送自己的修改
- 如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并
- 如果合并有冲突，则解决冲突，并在本地提交
- 没有冲突或者解决掉冲突后，再用git push origin branch-name推送就能成功
- 如果git pull提示“no tracking information”，则说明本地分支和远程分支的链接关系没有创建，用命令git branch --set-upstream branch-name origin/branch-name。

#### 自定义Git

- git config --global color.ui true	让Git显示颜色，会让命令输出看起来更醒目

#### 忽略特殊文件

- touch .gitignore	创建.gitignore文件
- git add -f file	用-f可以强制添加
- git check-ignore -v file	显示忽略规则的位置

> 忽略文件的原则是：
>  
> 忽略操作系统自动生成的文件，比如缩略图等
忽略编译生成的中间文件、可执行文件等，比如Java编译产生的.class文件
忽略你自己的带有敏感信息的配置文件，比如存放口令的配置文件


#### 忽略特殊文件的书写

##### 注释

- 注释使用 # 开头，后面跟注释内容

##### 忽略文件和目录

- folderName : 表示忽略 folderName 文件和 folderName 目录，会自动搜索多级目录

##### 仅忽略文件

- 仅忽略 folderName 文件，而不忽略 folderName 目录，其中，感叹号“!”表示反向操作。

```
folderName
!folderName/
```

##### 忽略目录

模式如下所示：

```
folderName/
```

忽略 folderName 目录，包括：

- 当前目录下的foldernName，例如：folderName/；
- 多级目录下的 folderName，例如：*/*/folderName/；

##### 使用通配符

- 星号“*” ：匹配多个字符；
- 问号“?”：匹配除 ‘/’外的任意一个字符；
- 方括号“[xxxx]”：匹配多个列表中的字符；

来看一个简单的例子，本地仓库的目录结构如下所示：

```
linuxy@linuxy:~/linuxGit$ tree
.
├── src
│   ├── add.c
│   ├── add.i
│   └── add.o
├── test.c
├── test.i
└── test.o
 
1 directory, 6 files
linuxy@linuxy:~/linuxGit$
```

其中，.gitignore 文件内容如下所示：

```
linuxy@linuxy:~/linuxGit$ cat .gitignore 


*.[io]
linuxy@linuxy:~/linuxGit$
```

故在本地仓库中，test.i文件、test.o文件、src/add.o文件、src/add.i文件会被忽略，而 test.c文件和add.c 文件不会被忽略。注意：这里忽略的匹配模式是多级目录的。

##### 忽略已经纳入版本管理的文件

-  有时候在项目开发过程中，突然心血来潮想把某些目录或文件加入忽略规则，按照上述方法定义后发现并未生效，原因是.gitignore只能忽略那些原来没有被track的文件，如果某些文件已经被纳入了版本管理中，则修改.gitignore是无效的。那么解决方法就是先把本地缓存删除（改变成未track状态），然后再提交： 
-  代码实现 

```git
git rm -r --cached .
git add .
git commit -m ""
```

## 后记

### 放弃本次的修改

```
git reset --hard
```

