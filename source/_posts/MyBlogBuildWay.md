---
title: 我的建站方法记录
date: 2021-05-12 19:41:14
tags: Blog
excerpt: 记录了我建站的详细过程。（小声：现在看来有很多走了弯路的地方 ﹁ ^ ﹁）
---

# 我的建站方法记录

环境：Windows10

## 1 安装git

### 1.1 安装Cygwin

>下载地址:[Cygwin官网](http://www.cygwin.com/)

![Cygwin-1]( /imgInPosts/MyBlogBuildWay/Cygwin.PNG )

>直接安装一路默认

![Cygwin-2]( /imgInPosts/MyBlogBuildWay/Cygwin2.PNG )
![Cygwin-3]( /imgInPosts/MyBlogBuildWay/Cygwin3.PNG )
![Cygwin-4]( /imgInPosts/MyBlogBuildWay/Cygwin4.PNG )
![Cygwin-5]( /imgInPosts/MyBlogBuildWay/Cygwin5.PNG )

>这里推荐使用国内的镜像网站（找不到得自己添加）

![Cygwin-6]( /imgInPosts/MyBlogBuildWay/Cygwin6.PNG )
>这里是一些必须安装的包

![Cygwin-7](/imgInPosts/MyBlogBuildWay/Cygwin7.PNG )

### 1.2 在Cygwin下使用git的SSH服务

#### 1.2.1 在Cygwin打开后，默认在~目录，使用如下命令查看实际在Windows中的路径

```shell
$ cygpath -w ~/

```

![Cygwin-8](/imgInPosts/MyBlogBuildWay/Cygwin8.PNG )

#### 1.2.2 使用ssh-keygen命令生成密钥

生成的.ssh文件夹在C:\cygwin64\home\ad662下，.ssh文件夹里面有id_rsa和id_rsa.pub两个文件，这两个就是SSH Key的秘钥对，id_rsa是私钥，id_rsa.pub是公钥。

```shell
$ ssh-keygen -t rsa -C "youremail@163.com"
```

#### 1.2.3 设置GitHub SSH Keys

登陆GitHub->Settings->“SSH Keys”，然后，点“Add SSH Key”，起个Title，在Key文本框里粘贴id_rsa.pub文件的内容，点“Add Key”。

#### 1.2.4 创建Repository

在github上创建一个仓库
注意repository的名字要与github的用户名一致。(如下形式)
`your_user_name.github.io`
其他保持默认即可。

#### 1.2.5 在本地创建一个同名仓库

回到Cygwin
使用如下命令在~目录下，创建一个名为`your_user_name.github.io`的仓库，初始化，并上传一两个文件测试能否使用。

```shell
mkdir xxx（仓库名）
git init
touch README
git add README 
git commit -m 'first commit'
git push origin master
```

## 2 安装Node.js

> 下载地址：[Node.js官网](https://nodejs.org/en/)
安装一路默认即可。

## 3 安装Hexo

### 3.1 安装

打开Cygwin，输入

```shell
$ npm install -g hexo
```

### 3.2 初始化

新建一个名为Hexo的文件夹，cd到该文件夹，输入如下命令进行初始化及初始化下载。

```shell
$ hexo init
$ npm install #会在目录中安装 node_modules。
```

### 3.3 启动服务

输入如下命令，启动服务；启动后，在浏览器中输入http://localhost:4000/，可见已经生成好了一篇模板blog。

```shell
$ hexo server
[info] Hexo is running at http://localhost:4000/. Press Ctrl+C to stop.
```

### 3.4 生成静态网页

输入如下命令，将.md文件转换为.html网页

```shell
$ hexo g
```

### 3.5 部署到Github上

#### 3.5.1 修改_config.yml文件

将下面的第一段内容修改成第二段

```sh
# Deployment
## Docs: http://hexo.io/docs/deployment.html
deploy:
  type:
```

```sh
# Deployment
## Docs: http://hexo.io/docs/deployment.html
deploy:
  type: git
  repository: git@github.com:yui73/yui73.github.io.git
  branch: master
```

#### 3.5.2 测试

在浏览器中打开(http://yui73.github.io/) ，正常显示网页，表明部署成功。

## 4 开始操作

### 4.1 部署操作

```shell
$ hexo new "postName" #新建文章(不用引号)
$ hexo new page "pageName" #新建页面
$ hexo generate #生成静态页面至public目录
$ hexo server #开启预览访问端口（默认端口4000，'ctrl + c'关闭server）
$ hexo deploy #将.deploy目录部署到GitHubhexo help  # 查看帮助hexo version  #查看Hexo的版本
```

每次更新完，都可使用三步走进行部署

```shell
$ hexo clean
$ hexo s -g
```

假设只执行`hexo g`，不执行`hexo s`的时候，则会在`hexo`文件夹下生成public文件夹，当遇到一些bug的时候，直接去看html文件有奇效。

### 4.2 更换主题-Way1

> [主题仓库](https://github.com/hexojs/hexo/wiki/Themes)

#### 4.2.1 在Cygwin下，cd到D:/hexo目录下，clone想要的主题仓库

```sh
git clone git://github.com/A-limon/pacman.git themes/pacman
#将链接中的https换成git，能避免一些clone失败的情况
```

#### 4.2.2 修改你的博客根目录/D/Hexo下的_config.yml配置文件中的theme属性，将其设置为pacman。

#### 4.2.3 更新主题

```sh
$ cd themes/pacman
$ git pull
```

### 4.3 更换主题

有些主题有自己推荐的安装方式，详情见文档。
> [Fluid](https://hexo.fluid-dev.com/docs/start/)

### 4.4 增加Live2D小人

> GitHub仓库：[Hexo-Live2D](https://github.com/EYHN/hexo-helper-live2d/blob/master/README.zh-CN.md#hexo)

#### 4.4.1 安装插件

输入

```shell
$ npm install --save hexo-helper-live2d
```

#### 4.4.2 修改配置

配置_config.yml文件（我一般都配根目录下面那个）

### 4.5 插入pdf

先安装

```shell
$ npm install --save hexo-pdf
``` 

在markdown对应的文件夹中存放你的pdf,输入：

```md
{% pdf ./Prism5forWPF.pdf %}
```

## 5 一些问题

### 5.1 图片加载不出来

#### 5.1.1 注意图片是`.png`还是`.PNG`，在本地测试无关，在Github上区分大小写。

#### 5.1.2 直接找`html`文件看路径是否正确，路径可能不支持中文。

### 5.2 hexo g 时的报错

报错内容如下：

```sh
FATAL {
  err: Template render error: (unknown path)
    Error: expected end of comment, got end of file
      at Object._prettifyError (D:\Hexo\node_modules\nunjucks\src\lib.js:36:11)
      at Template.render (D:\Hexo\node_modules\nunjucks\src\environment.js:538:21)
      at Environment.renderString (D:\Hexo\node_modules\nunjucks\src\environment.js:380:17)
      at D:\Hexo\node_modules\hexo\lib\extend\tag.js:236:16
      at tryCatcher (D:\Hexo\node_modules\bluebird\js\release\util.js:16:23)
      at Function.Promise.fromNode.Promise.fromCallback (D:\Hexo\node_modules\bluebird\js\release\promise.js:209:30)
      at Tag.render (D:\Hexo\node_modules\hexo\lib\extend\tag.js:235:20)
      at Object.onRenderEnd (D:\Hexo\node_modules\hexo\lib\hexo\post.js:297:22)
      at D:\Hexo\node_modules\hexo\lib\hexo\render.js:79:21
      at tryCatcher (D:\Hexo\node_modules\bluebird\js\release\util.js:16:23)
      at Promise._settlePromiseFromHandler (D:\Hexo\node_modules\bluebird\js\release\promise.js:547:31)
      at Promise._settlePromise (D:\Hexo\node_modules\bluebird\js\release\promise.js:604:18)
      at Promise._settlePromise0 (D:\Hexo\node_modules\bluebird\js\release\promise.js:649:10)
      at Promise._settlePromises (D:\Hexo\node_modules\bluebird\js\release\promise.js:729:18)
      at _drainQueueStep (D:\Hexo\node_modules\bluebird\js\release\async.js:93:12)
      at _drainQueue (D:\Hexo\node_modules\bluebird\js\release\async.js:86:9)
      at Async._drainQueues (D:\Hexo\node_modules\bluebird\js\release\async.js:102:5)
      at Immediate.Async.drainQueues (D:\Hexo\node_modules\bluebird\js\release\async.js:15:14)
      at processImmediate (internal/timers.js:462:21) {
    cause: Template render error: (unknown path)
      Error: expected end of comment, got end of file
        at Object._prettifyError (D:\Hexo\node_modules\nunjucks\src\lib.js:36:11)
        at Template.render (D:\Hexo\node_modules\nunjucks\src\environment.js:538:21)
        at Environment.renderString (D:\Hexo\node_modules\nunjucks\src\environment.js:380:17)
        at D:\Hexo\node_modules\hexo\lib\extend\tag.js:236:16
        at tryCatcher (D:\Hexo\node_modules\bluebird\js\release\util.js:16:23)
        at Function.Promise.fromNode.Promise.fromCallback (D:\Hexo\node_modules\bluebird\js\release\promise.js:209:30)
        at Tag.render (D:\Hexo\node_modules\hexo\lib\extend\tag.js:235:20)
        at Object.onRenderEnd (D:\Hexo\node_modules\hexo\lib\hexo\post.js:297:22)
        at D:\Hexo\node_modules\hexo\lib\hexo\render.js:79:21
        at tryCatcher (D:\Hexo\node_modules\bluebird\js\release\util.js:16:23)
        at Promise._settlePromiseFromHandler (D:\Hexo\node_modules\bluebird\js\release\promise.js:547:31)
        at Promise._settlePromise (D:\Hexo\node_modules\bluebird\js\release\promise.js:604:18)
        at Promise._settlePromise0 (D:\Hexo\node_modules\bluebird\js\release\promise.js:649:10)
        at Promise._settlePromises (D:\Hexo\node_modules\bluebird\js\release\promise.js:729:18)
        at _drainQueueStep (D:\Hexo\node_modules\bluebird\js\release\async.js:93:12)
        at _drainQueue (D:\Hexo\node_modules\bluebird\js\release\async.js:86:9)
        at Async._drainQueues (D:\Hexo\node_modules\bluebird\js\release\async.js:102:5)
        at Immediate.Async.drainQueues (D:\Hexo\node_modules\bluebird\js\release\async.js:15:14)
        at processImmediate (internal/timers.js:462:21),
    isOperational: true
  }
} Something's wrong. Maybe you can find the solution here: %s https://hexo.io/docs/troubleshooting.html
```

主要报错要看

```shell
 err: Template render error: (unknown path)
    Error: expected end of comment, got end of file
```

错误原因：Markdown文件中存在不能转义的字符
例如：

```shell
{#
# //#单独出现
```

### 5.3 希腊字母和数学符号无法在网页显示

错误原因：使用Markdown语法写的希腊字母在转义成HTML时，无法被HTML识别

解决办法：直接在Markdown里面使用HTML语法的希腊字母和数学符号即可
