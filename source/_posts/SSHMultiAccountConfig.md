---
title: SSH多账户配置
date: 2023-02-24 14:38:00
tags: SSH
---

*起因：想要在虚拟机上同时使用两个Git账户管理代码，相互之间不能影响，目前已经有一个账户了。*

## 1 生成SSH-Key

目前已经有`id_rsa`和`id_rsa.pub`一对密钥了，再使用指令生成另一对。

```sh
ssh-keygen -t rsa -C "XXXX@XXX.com"
```

## 2 修改 ~/.ssh/config文件

将新生成的密钥`YYY`和原来的密钥都放到.ssh文件夹下。

修改内容如下：

```sh
# used by XXX
Host XX.XXX.XXX.XXX:XXXX
  HostName XXX
  PreferredAuthentications publickey
  IdentityFile ~/.ssh/id_rsa
  IdentitiesOnly yes
 
# used by YYY
Host YYY.github.com # 防止冲突
  HostName github.com
  PreferredAuthentications publickey
  IdentityFile ~/.ssh/YYY
  IdentitiesOnly yes
```

## 3 将新生成的密钥添加到Github账户里并测试联通性

使用如下指令测试联通性：

```sh
ssh -T git@YYY.github.com #后半部分和Config文件的Host保持一致
```

联通了就可以正常使用啦 ~ OvO

## 4 使用方法

在`git clone`时，要将后缀改成Config文件里那种形式。随手拿个仓库举个栗子~

```sh
# 原来的
git clone git@github.com:yui73/2022ECNUInfoVis.git
# 现在的
git clone git@YYY.github.com:yui73/2022ECNUInfoVis.git
```
