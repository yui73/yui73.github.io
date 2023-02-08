---
title: GitHub用法集合 + Git学习
date: 2021-07-13 10:39:00
tags: Git
---

# GitHub 用法集合 + Git学习

文章更新顺序：




> 对GitHub一些常规操作做一些记录，关联电脑与GituHub，生成SSH密钥，已经在之前建站中完成了。
> 后续更新了Git的一些概念学习

### 1 验证是否连接成功

输入：

```
$ ssh -T git@github.com
```

出现`Hi yui73! You've successfully authenticated, but GitHub does not provide shell access.`表示成功连接。

`-T` 参数表示：不显示终端，只显示连接成功信息。

### 2 上传Code

- cd到目标文件夹内
- 输入以下指令

```
git init
git add
git commit -m "Commit 备注"
git remote add origin git@github.com:... //你仓库的SSH地址
git push -u origin master
```

### 3 GitHub大文件限制

#### 3.1 报错形式

![Error](/imgInPosts/GitHubUsage/error.png)

#### 3.2 解决方法 Git-lfs

- 去[官网](https://git-lfs.github.com/)下载安装Git-lfs
- 重启Cygwim
- 输入`ssh -T git@github.com`
- cd到目标文件夹
- 输入`git lfs install`
  ![lfs-install](/imgInPosts/GitHubUsage/lfs-install.PNG)
- Track你的大文件
  ![Track](/imgInPosts/GitHubUsage/track.PNG)
- 重新`add commit push`

### 4 Git理论学习

> 之前对于Git只是单纯的使用，对其原理并未有足够的了解导致时隔半年再度使用Git时又是一脸懵逼，因此决定重新学习一些原理
> 官方文档:[Git](https://git-scm.com/about)

#### 4.1 版本控制系统

版本控制系统分为如下三大类：

- 本地版本控制
- 集中式版本控制
- 分布式版本控制
  Git属于这种，一个命令行工具；Github为一个使用Git版本管理工具的代码托管云服务网站。

#### 4.2 Git基础知识

Git每次保存的的都是文件的完整快照。
Git工程的工作区域：

- Workplace（工作目录）
- Index（暂存区域）
- Local repository（本地仓库）
- Remote repository （远程仓库）
下图为其工作流程：
![GitUsage](/imgInPosts/GitHubUsage/Git.png)
文件的三种状态：
- 已提交
- 已修改
- 已暂存

### 5 常用Git指令

> 学习视频：[玩转Git三剑客](https://time.geekbang.org/course/detail/100021601-71676)

#### 5.1 使用Git的最小配置

||功能|
|-|-|
|user.name|目标用户名|
|user.email|目标邮箱|

#### 5.2 给文件夹重命名

|1|2|3|功能|
|-|-|-|-|
|git mv|XXX(前)|XXX(后)|给文件夹重命名|

#### 5.3 查看版本历史

|1|2|3|功能|
|-|-|-|-|
|git log| --oneline||简洁|
||-nx||最近的x条记录|
||--all||所有分支|
|||--graph|分支图形化|

#### 5.4 切换分支

|1|2|3|功能|
|-|-|-|-|
|git|checkout|XXX|切换到XXX分支|

#### 5.5 git文件解释

|文件名|解释|
|-|-|
|head|存放当前工作分支|
|config|当前git的配置|
|refs|存放tags和heads|
|objects|存放对象，文件夹名以哈希值前2位字符命名，每个object由40位字符组成，前2位为文件夹，后38位为文件。文件类型：commit/tree/blob。|

文件夹逻辑树如下：

**整棵树的叶子结点为blob。**

#### 5.6 显示版本库对象的内容(-p)、类型(-t)、大小(-s)

|1|2|功能|
|-|-|-|
|git cat-file| -p/-t/-s|显示版本库对象的内容(-p)、类型(-t)、大小(-s)|

#### 5.7 分离头指针

当Head指向某个commit的时候处于分离头指针的状态，此时没和任何分支挂钩，在切换分支时会丢失数据。

#### 5.8 删除分支

|1|2|3|解释|
|-|-|-|-|
|git branch|-d|XXX|会检查merge|
|git branch|-D|XXX|强制删除|

#### 5.9 修改Message

|1|2|3|解释|
|-|-|-|-|
|git commit|--amend||修改最新的commit信息|
|git rebase|-i|XXX|交互，此时Head哈希发生变化，blob未变化。|

**commit合并也使用rebase交互实现。**

#### 5.10 比较差异

|1|2|3|功能|
|-|-|-|-|
|git diff|--cached||暂存区 VS Head|
|git diff|||暂存区 VS 工作区|
|git diff|XXX(分支) YYY(分支)|-- XX(文件)|查看不同提交的指定文件的差异|

#### 5.11 恢复操作

|1|2|3|功能|
|-|-|-|-|
|git reset|HEAD|--(空格)XXX|将暂存区恢复成HEAD|
|git reset|--hard|XXX|消除最近几次提交|
|git chechout|--(空格)XXX||工作区恢复成暂存区|

#### 5.12 删除文件

|1|2|功能|
|-|-|-|
|git rm|XXX|更改直接写入暂存区|

#### 5.13 加塞任务 - 暂存功能

|1|2|功能|
|-|-|-|
|git stash|list|查看暂存列表|
||pop|释放暂存后，会删掉stash|
||apply|释放暂存后，不会删掉stash|

#### 5.14 拉取代码

|1|功能|
|-|-|
|git fetch|只拉取代码|
|git pull|fetch + merge|

**当远端分支和本地分支不是fastforward的关系，则需要merge或者rebase，rebase则项目结构更为线性。**
> 小技巧：当Git中远端的Hash值和本地的Hash值一致则指向一致。

#### 5.15 团队仓库

**团队合作禁止：**

- `push -f`操作
- 向集成分支执行变更历史的操作
- `rebase`操作

#### 5.16 淘Github项目

|1|2|功能|
|-|-|-|
|in:|XXX|在XXX文件中搜索|
|stars:|>XXX|搜索星数大于XXX|
|filename:|XXX|搜索文件名为XXX|