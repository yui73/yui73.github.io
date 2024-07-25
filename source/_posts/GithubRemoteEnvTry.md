---
title: Github远端开发环境尝试
date: 2024-07-18 14:00:00
tags: Dev
---

# Github远端开发环境尝试

> 前情提要：经过前两次的Blog升级，现在已经可以从本地VSCode上传到Github，通过workflow actions在云上自动化编译并部署静态内容到相应的分支上。
> 但是这要求本地需要有一个可以安装VSCode的环境，但背个iPad实在是太方便了，谁愿意背笔记本啊。
> 由此对目前的云开发方式进行了一定的探索工作。

提示：网页版的VSCode在iPadOS16是无法成功访问的，需要升级系统版本至最新版本，既可成功访问。

## 1.VSCode Tunnels

此法就是开启VSCode的管道功能：
    - 通过安装`Remote - Tunnels`插件，打开VSCode的管道功能（这里也可以通过命令行的方式直接运行VSCode的程序打开）
    - 登陆个人的Github或者Microsoft账号
    - 打开`vscode.dev`网址
    - 通过云虚拟机中转成功通过网页访问到本地电脑的资源

缺陷：
    - 微软的Azure云服务并非免费，我用了几分钟，大概产生3刀的费用
    - `vscode.dev`的网页版本对插件安装存在限制性

## 2. 使用Github CodeSpace

这里真的要感谢Github，实在是太优秀了，至今我能想到的代码相关的管理功能，`Github`基本上都能帮我实现一二，虽然一步一步配环境会略有难度，但这是锻炼，也是乐趣。

因为之前已经实现了用不同分支去管理部署内容和环境内容，所以这里可以直接通过环境内容分支去创建`CodeSpace`（创建过程看文档非常简单）。然后，BOOM!!!，`Open in browser`就可以了。非常简单- v -

这种方法并不访问我主机上的本地资源，而是直接起虚拟机去访问远端资源。

![远端界面](/img/img_in_posts/GithubRemoteEnvTry/image.png)

缺陷：
    - 图片复制黏贴可以直接上传，但是移动到框架可以识别的位置会比较麻烦（其实自己搭图床都能解决）

优点：
    - 免费🆓
    - 无需任何环境，只要browser + Github（国内的话）
    - 可以安装VSCode插件🆗
    - 可以远端直接提交Github并触发部署Actions，一键完成✅😆
    - 可以随时随地记录论文阅读笔记了

### 2.1 收费问题

Github不像微软，基本上都有免费额度，写博客也足够了。

在个人`Settings -> Billing`会有个人版详细的免费额度计划。涉及到云开发内容的只有`Codespaces`模块下。

|module|content|billing|
|-|-|-|
|CodeSpace|Usage hours|120 core hours per month|
||Storage|15 GB per month|

使用时间是根据核心数*小时数来计算的。

这下，我带个iPad就能写博客了，不知道是不是会更多产出捏。
嘻嘻٩(๑′∀ ‵๑)۶•*¨*•.¸¸♪