---
title: 在WIN10环境安装MySqlClient的一些问题
date: 2024-12-26 09:43:00
tags: Dev
---

最近正在搭建毕业设计的后端系统，使用FastAPI对接MySql数据库，FastAPI框架依赖MySqlClient包，然而由于使用了conda虚拟环境，因此在安装这个依赖时走了不少弯路，简以记录。

## 1 使用`conda install`直接安装

报错：在仓库中找不到可用版本。进入PYPI官网查询Python3.8+Win64对应可以使用的版本。
![Pypi](/img/img_in_posts/InstallMySqlClientInWin10/1.png)

进入Conda官网搜索，也显示有2以上版本的包。
![Conda](/img/img_in_posts/InstallMySqlClientInWin10/2.png)

使用指令进行指定版本下载，报错：找不到该版本的包。

无论是国内镜像还是国外官网都找不到，`conda search`也显示没有该版本，最高版本到`1.3.14`。

不死心，进入仓库直接搜索，确实是最高版本到`1.3.14`，死心了。

## 2 使用压缩包文件直接安装

步骤如下：

1. 去官网下载对应版本的压缩包
![zip](/img/img_in_posts/InstallMySqlClientInWin10/3.png)

2. 解压对应压缩包

3. cd进入解压文件夹，使用指令安装

```bash
python setup.py install
```

但是MySqlClient底层使用C++编写，报错：需要对应版本的C++编译环境。

> *到此处，可以选择安装C++编译环境解决，但我觉得再配一个环境有点麻烦。*

## 3 使用预编译版本的轮子

*弯路一：分不清轮子的命名和版本*

学习[博客](https://blog.csdn.net/weixin_49114503/article/details/139326651)

轮子文件的命名规则：`{distribution}-{version}(-{build tag})? -{python tag}-{abi tag}-{platform tag}.whl`

回到PYPI官网：

![PYPI-MySqlClient-1](/img/img_in_posts/InstallMySqlClientInWin10/4.png)

可以看到有两个满足Python3.8版本的包。

*弯路二：找不到预编译版本*

下载第一个版本的包，依旧会提示无法安装，因为一个轮子未进行C++预编译。

*此处求助了万能的GPT，给了[某大学的网址](https://www.lfd.uci.edu/~gohlke/pythonlibs/)，但网址已移动*

## 4 解决

其实，这里下载第二个预编译的包就可以解决问题了，前面的`c`就是`{build tag}`

![PYPI-MySqlClient-2](/img/img_in_posts/InstallMySqlClientInWin10/5.png)

下载对应`.whl`文件，执行指令。

```bash
pip install path/to/example_package-1.0-py3-none-any.whl
```

完成安装。