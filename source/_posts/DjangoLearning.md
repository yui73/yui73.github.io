---
title: Django学习
date: 2022-01-08 14:50:00
tags: Django
---

# Django学习之旅

学习视频：[极客](https://time.geekbang.org/course/intro/100061901?tab=catalog)

学习博客：[小青姐姐](https://www.cnblogs.com/xiaoqingSister/p/13355832.html)

前言：公司给买的课 OvO

## 1 初识Django

- 课程需要基础
  - Python & 前端三剑客(HTML CSS JS) & 理解Web应用的前后端交互
  
- Django适用场景
  - 内容管理系统 & 企业内部系统 & 运维管理系统
  
- Django优缺点
  - 优点：代码干净；提供管理后台；复用度高；易于扩展复用的中间件；内置的安全框架，应付常见的Web攻击；丰富的第三方类库
  - 缺点：不易并行开发，单点扩展；不适合高并发的to C项目；不适合非常小的项目
  
- 哪些产品使用了Django

- Django的MTV架构
  - Model 层
    View 层通过 Model 层和数据库交互
  
  - Template 层
  
    View层使用数据填充到Template层模板里面；动态HTML文件里不变的部分
  
  - View 层
    页面呈现 + 处理请求的业务逻辑；用户请求处理层
  
  - Controller层（非严格意义上
  
    网站路由控制器；用正则表达分配路由
  
- Django的设计思想

  - DRY:不重复造轮子
  - 松耦合；快速开发；易于扩展

## 2 搭建环境

- Pycharm
- Anaconda

## 3 使用Django创建第一个项目

### 3.1 初始化

创建Blog项目，项目名为 mydjangoblog

```sh
$ django-admin startproject mydjangoblog
$ cd mydjangoblog
$ python ./manage.py runserver 0.0.0.0:8080 指定端口
```
此时在项目文件夹中初始化了`db.sqlite3`的文件数据库，可以在`setting.py`中进行数据库配置的更改或更改数据库引擎，数据库访问层和Django是松耦合的。

接下来进行数据库的配置。（此步骤，老师的视频中无法访问admin页面，但我的已经可以访问了，但还是照葫芦画瓢做了一遍）

```sh
$ python manage.py makemigrations //此步没有变更，因为是新项目没有Model层
$ python manage.py migrate //此步创建了不同的表和表字段
$ python manage.py createsuperuser //超级用户的用户名为admin 密码为1234
```

此时就可以登录admin后台了

接下来将项目导入Pycharm，看目录结构

```sh
--mydjangoblog
|----iniy.py
|----asgi.py 异步网关接口
|----wsgi.py
|----setting.py 整个项目的配置文件
|----urls.py
|--db.sqlite3
|--manage.py
```

`setting.py`中有几项配置：

- `DEBUG = True` - 展示调试信息
- `INSTALLED_APPS` - 安装的Django应用
- `MIDDLEWARE` - 使用的中间件
- `LANGUAGE_CODE` - 使用的默认语言

### 3.2 创建应用并添加相应的Model

使用命令

```sh
python manage.py startapp articles //文章管理并将articles应用添加到settings里面去
```
>不加括号是引用函数 而非调用

前端：bootstrap4

## 后面直接上手写项目了O V O 啊吧啊吧~