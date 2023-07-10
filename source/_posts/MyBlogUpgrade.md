---
title: 博客升级踩坑记录
date: 2023-02-08 13:33:00
updated: 2023-07-10 13:47:00
tags: Blog
excerpt: 记录了我给博客升级一系列踩坑的地方。
---

# 博客升级踩坑记录

## 1 增加环境分支

同时在虚拟机和自己的电脑上写博客，每次更改环境，同步博客都是很令人头疼的问题，因此想到通过git实现环境的管理和同步，在网上搜索之后发现可以在pages的仓库里再新建一个分支用于管理环境。

Hexo的文件夹下已经默认有一个`.gitignore`文件，里面将一些无需上传git的文件已经忽略了。内容如下：

```gitignore
.DS_Store
Thumbs.db
db.json
*.log
node_modules/
public/
.deploy*/
_multiconfig.yml
```

大体方法如下：

- 新建分支XXX

- 下载仓库到本地

- 删除除了`.git`以外的所有东西

- 提交并`git push origin XXX(新的分支名)`清空分支上的所有内容

- 将`.git`复制到Hexo目录下

- 再提交并`git push origin XXX(新的分支名)`将环境内容提交到环境分支上

*但还是有需要修改源码的时候，我这里是用markdown做一个记录，在升级node_modules或者theme后，要重新复制修改后覆盖进去。*

## 2 添加Letax渲染

使用的Hexo环境如下：

|内容|版本|
|-|-|
|hexo|6.3.0|
|theme|fluid@latest|

### 2.1 选择渲染引擎

>网上说的引擎都试了一遍，最后只有这一个可以用的

- 卸载原有的渲染引擎`npm uninstall hexo-renderer-marked --save`

- 安装kramed引擎`npm install hexo-renderer-kramed --save`

- 修改转义规则

  - 打开目录`/node_modules/kramed/lib/rules/inline.js`
  
  - 修改11行为``escape: /^\\([`*\[\]()#$+\-.!_>])/,``

  - 修改21行为``em: /^\*((?:\*\*|[\s\S])+?)\*(?!\*)/,``

### 2.2 选择CDN

>处理完上述问题后，Mathjax报错`Mixed Content:blahblah http访问不到`

一开始以为是CDN链接的问题，改了好多链接：

```yml
# mathjax: https://lib.baomitu.com/mathjax/3.2.0/
# 上面这个是默认的 缺少字体 渲染不成功
# mathjax: https://cdn.jsdelivr.net/npm/mathjax/MathJax.js?config=TeX-AMS-MML_HTMLorMML
# 这个是网上的 啥也加载不出来 还没找原因
# mathjax: https://cdn.jsdelivr.net/npm/mathjax@3.1.2/es5/
# 这个是上一个版本的 原来的 成功了 可能有点慢

#被拦截 mathjax: https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML
#出错 mathjax: https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.1/MathJax.js?config=TeX-AMS-MML_HTMLorMML
# 采用的
mathjax: https://cdn.jsdelivr.net/npm/mathjax@3.2.2/es5/
```

后来发现真正解决需要修改主题的Layout

### 2.3 修改Layout

发现`Mixed Content`报错可以在html的head里添加标签解决（自动将http请求转化为https请求）

```html
<meta http-equiv="Content-Security-Policy" content="upgrade-insecure-requests">
```

这就需要修改主题的模板文件，因此不太能使用npm来安装主题，需要下载主题的压缩包并解压到`\Hexo\themes\fluid`里面

修改`\Hexo\themes\fluid\layout\_partials\head.ejs`

在head标签下添加上述内容即可。

{% note info %}
发现官方支持了直接在主题里开启强制https,在主题配置文件里开启，force_https: true 即可，目前先不改了。
{% endnote %}

### 2.4 渲染出错

*本来以为问题解决了，结果一刷新就乱码，搜了好多好多好多多多博客才找到是页面懒加载的问题。*

**参考博客**：[博客地址](https://cloud.tencent.com/developer/article/2067298)

公式懒加载之后就无法再次嵌入网页中，因此需要取消对公式的懒加载。

因此需要删除`\Hexo\themes\fluid\layout\_partials\math.ejs`中的

```ejs
loader : {
  ${ lazy ? 'load: \[\'ui/lazy\'\]' : '' }
},
```

## 3 图片加载问题

更新主题后，图片放置的位置也出现了变化，不可以再放在`_posts`文件夹下，需要放到`img`文件夹下，再引用。

后续考虑加图床或者修改一下源码吧，不然写的时候渲染不出来不太方便。

想学习的博客：[基于fluid主题的基础样式修改](http://www.mmyosotis.cn/post/3433766775/)

*为了给博客加Letax渲染真的是踩坑无数，之前加的Mermaid渲染可能又要重新再配一遍，配的时候再记录吧。这次配下来的感想就是主题还是直接源码装比较好，便于修改。*

## 4 自动化部署

> 没有自动化部署，每次更新文章都要本地有环境先编译再上传到仓库里，有点麻烦考虑用自动化部署简化流程。

### 4.1 巨硬家的DevOps

（要钱并且要申请，看到一篇非常详细的[博客](https://www.imaegoo.com/2019/hexo-ci/)讲了如何配置）

### 4.2 Github Actions

配置简单，容易上手，将我从配环境的苦海当中拯救出来（*＾-＾*）

参考：[知乎](https://zhuanlan.zhihu.com/p/137867759)

- Step 1: 获取Token

- Step 2: 配置Secret变量`GIT_EMAIL/GH_TOKEN`

- Step 3: 在`.github`文件夹下，新建`workflows`文件夹，新增`deployment.yml`

- Step 4: 添加工作流配置到`deployment.yml`中

```yml
# 文件路径 .github/workflows/deployment.yml
name: Deployment

on:
  push:
    branches: [hexo] # only push events on source branch trigger deployment

jobs:
  hexo-deployment:
    runs-on: ubuntu-latest
    env:
      TZ: Asia/Shanghai

    steps:
    - name: Checkout source
      uses: actions/checkout@v2
      with:
        submodules: true

    - name: Setup Node.js
      uses: actions/setup-node@v1
      with:
        node-version: '12.x'

    - name: Install dependencies & Generate static files
      run: |
        node -v
        npm i -g hexo-cli
        npm i
        hexo clean
        hexo g
    - name: Deploy to Github Pages
      env:
        GIT_NAME: WillCAI2020
        GIT_EMAIL: ${{ secrets.GIT_EMAIL }}
        REPO: github.com/WillCAI2020/WillCAI2020.github.io
        GH_TOKEN: ${{ secrets.GH_TOKEN }}
      run: |
        cd ./public && git init && git add .
        git config --global user.name $GIT_NAME
        git config --global user.email $GIT_EMAIL
        git commit -m "Site deployed by GitHub Actions"
        git push --force --quiet "https://$GH_TOKEN@$REPO" master:master
```

- Step 5: 提交修改到同步仓库，进入Actions页面查看运行结果
