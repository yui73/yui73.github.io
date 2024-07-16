---
title: PythonBlog开发学习
date: 2022-05-19 17:00:00
tags: Django
---


# 毕业设计开发记录

## 0 项目流程
- 架构设计
  - 功能模块
  - 设计模式
  - 项目服务器架构（Web/数据库服务器/负载均衡？）
  - 数据库设计（什么数据库/多少张表/层次结构/关系图/分库？）
- 代码模块实现
- UI页面设计&交互实现（用图的方式表现）
- 代码规范是PEP8

## 1  技术难点&开发环境
- 开发环境
  - Win10推荐Linux
  - python
  - django
  - mysql
  - mysql-python
  - pillow(图形处理库)
- 表单提交与处理
- 文件上传
- session&cookie
- ORM（对象映射关系 -> 无需了解SQL细节）
- Django模板语言（模板规划/父子模板继承/标签使用/过滤器）
- JQuery（做表单验证/实现Ajax的请求/Json）
- Ajax
- xml
- 后台管理admin（很强大的后台换模板）
- 日志调试 （日志如何设置）
- 缓存 （博客以内容呈现为主->放入缓存提高效率）
- 安全功能（项目安全配置/防止非法远程提交 -> Key/防止SQL注入）

## 2 项目搭建
- 安装virtualenv,创建虚拟环境 `pip install virtualenv`，python三大神器
- Pycharm新建项目，设置虚拟环境路径，安装相关库
- 配置静态文件&模板
- 访问项目网页

## 3 日志器的使用
1 如何在django中配置日志器的使用
2 如何调用setting.py的配置信息作为全局使用

## 4 数据库设计
1. 使用的工具：PowerDesign,ERWin,Visio，Navicat Data Moduler...
2. 数据库设计过程
- 分析可能存在的设计表
用户表，文章表，评论表，标签（tag)表，分类表，友链表，（后来迭代：信息表
- 分析可能存在的数据列，对应的数据类型及约束
- 设计数据模型图

## 5 Model的设计与使用
`net start mysql`
- django model的设计与使用
主键自动维护生成
- django中mysql数据库的配置
- 如何生成数据库
```
create database blogdb;
python manage.py makemigrations blog
# 此处外键需要定义on_delete
# 代替syncdb使用migrate
# 以后model都先migration在migrate 同步更改数据库
# 创建超级用户
python manage.py createsuperuser
username:admin
email:ad66204635@126.com
password:admin

python manage.py makemigrations
python manage.py migrate
```

## 6 admin配置
1. django admin是django.contrib的一部分
contrib下还包括auth，sessions，comments等模块
2. 如何配置使用django admin
 - 在INSTALLED_APPS中添加django.contrib.admin
 - 配置urls.py
 `url(r'^admin/',include(admin.site.urls))`
 - 在admin中注册Model，（默认方式和自定义方式）,注意model中关于admin的一些配置
 一些常用属性：
   - fields\exclude
   - fieldsets 编辑数据分组
   - list_display
   - list_display_links
   - list_editable
   - list_filter
   - inlines
   - ...具体看文档
  - xadmin表现形式比较丰富
  - admindocs文档生成器

## 7 增加富文本编辑器
1. 常见的富文本编辑器->目前需要markdown的
2. djangoadmin中添加富文本编辑器的几种方式：
 - 使用第三方的一些库ckeditor
 - 在admin中定义富文本编辑器的widget
 - 定义ModelAdmin的媒体文件

## 8 图片上传
1. 配置MEDIA_URL和MEDIA_ROOT
2. 在urls.py中配置路由 url
3. 在model中设置图片的上传位置和路径
4. 一些BUG
   - 上传失败 setting的template没有`'django.template.context_processors.media',`
   - 更新urls如下：
```python
     from django.conf import settings
     from django.conf.urls.static import static
     urlpatterns = [    
         path('admin/', admin.site.urls), 
     ] + static(settings.STATIC_URL, document_root=settings.STATIC_ROOT)
     
     urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```

   - STATICFILES_DIRS是List
   - MEDIA_ROOT是str

> 进入前端开发

## 9 前台模板规划和设计
> 个人认为html做的前端有点难看，使用bootstrap替换组件
> 参考博客：[Django制作个人博客——前端页面](https://blog.csdn.net/weixin_43376839/article/details/90584377)

功能点：
- 模板的规划和设计
- 导航数据获取（分类）
技术点：
- 拆分模板，抽离Base模板，block和include的使用->提高代码重用性
- 基本查询的使用

> 暂停看视频（https://www.bilibili.com/video/BV1mZ4y1p7oy?p=12）
> 去学一下bootstrap把前端先写了

## 10 Bootstrap
学习博客：[关于django写前端的几个好用的前端框架](https://blog.csdn.net/python_jw/article/details/78914562?spm=1001.2101.3001.6650.5&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-5.pc_relevant_paycolumn_v3&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-5.pc_relevant_paycolumn_v3&utm_relevant_index=10)

- 在Django中使用Bootstrap--crispy-forms
  `pip install django-crispy-forms`
- 在settings.py添加如下配置
```
  INSTALLED_APPS = [    ...    'crispy_forms',]CRISPY_TEMPLATE_PACK = 'bootstrap4'
```
> 该方法有bug不好解决，替换成极客时间的方法
- 在Django中使用Bootstrap
> 动态方法
```
  pip install django-bootstrap4
  添加到app中
  模板中使用bootstrap标签
```
> 静态方法

- 去Bootstrap官网下载（CSS/JS/FONT）
- 去JQuery官网下载JQuery
- 前台模板使用Clean Blog


## 11 admin使用simpleui美化
- 安装时遇到的问题
  settings配置里注册app simpleui必须是第一个 覆盖默认的admin才可以
-  重写预设的menu
-  在admin中设置网站title

app/admin.py中
```python
  admin.site.site_header = '博客后台管理系统' 
  admin.site.site_title = '博客后台管理系统'
  admin.site.index_title = '博客后台管理系统'
```

## 12 分页器
> article表和tag表通过article_tag表连接
```python
#分页器实现
        paginator = Paginator(all_article,10)
        # 分页器异常处理
        try:
            page = int(request.GET.get('page',1))
            all_article = paginator.page(page)
        except (EmptyPage,InvalidPage,PageNotAnInteger):
            # 出错时默认第一页
            all_article = paginator.page(1)
```

## 13 Manager管理器

> 开始接触一些关于数据联动的问题

功能点：文章归档

技术点：

1. 使用`filter()`查询
2. `values()` `distinct()`的使用 可以去重复 但还是不满足需求
3. django直接使用SQL的两种方式 - 不推荐
   - raw （返回结果必须包含主键，因此异常）
   - excute （游标实现）
4. 尝试一些优雅的方式解决数据处理上的问题
   自定义Manager的管理器 - 管理Modal方法
   
## 14 第一次代码重构
> 提高代码可重用性

技术点：
1. orderby的使用，限制取出条数
2. 使用filter()完成查询
3. 提高代码可重用性
  - 分页代码重构
  - urls.py路由重构
  - 全局代码重构


## 15 文章详情页的实现
- 新增article.html、login.html、reg.html
- 修改了Comment Model和User Model->实现匿名评论
- 增加功能代码
技术点：
- DoesNotExist异常,文章查询不出的异常,get方法查询数据。
- safe过滤器的使用&date过滤器的使用
- 自定义过滤器
难点：
- 将后台markdown格式的content渲染成前端的正常页面
1. 使用django的django-markdown-deux模块
   - `pip install django-markdown-deux`
   - 修改setting.py,在installed_apps中加入django-markdown-deux
   - 在文章内容后面加上`|markdown`过滤器

2. 模块不支持代码渲染
3. 模块图片显示过大

> 因此更换第二种方法

---

备用方法：

1. 使用python的markdown模块

   - `pip install markdown`

   - 在文章view里引入

```python
       article = markdown.markdown(article.body,extensions=[
             'markdown.extensions.extra',
             'markdown.extensions.codehilite',
             'markdown.extensions.toc',
         ])
```
   - 在文章内容后面加上`|markdown`过滤器

2. 此时代码能正常显示，但没有高亮，增加高亮
   - `pip install Pygments`
   - 在静态资源库中新建`md_css`文件夹存放代码高亮的css
   - 进入`md_css`执行指令`pygmentize -S monokai -f html -a .codehilite > monokai.css` `-S`后面是风格样式；也可用`pygmentize -S default -f html -a .codehilite > styles.css`这样就是默认样式
   - 将`<link rel="stylesheet" href="{% static 'blogpost/css/styles.css' %}">`放到`article.html`的最前面
3. 注意上述方法都要加`{|safe}`过滤器，否则html会转义内容，造成乱码。
4. 图片过大显示问题
   - 在上传时`![](/media/editor\Cygwin_20220501234455635202.PNG){:width="100%" align=center}`即可。

## 16 第二次代码重构-页面管理解耦
> 多表继承方法
- 分开写设计网页管理父类`PageManager`
- About页面继承`PageManager`后进行修改（不可继承-还是分开写了
- 代码解耦（html)

## 17 Contact Me页面

> sb-from 要钱才能获得API-Token 换成MUI的Form

1. [MUI官网](https://www.muicss.com/)

## 18 增加文章目录

> 在view.py中 -> 有bug暂缓 -> 论如何在html里调用JS -> 还是有bug

1. 增加toc插件`markdown.extensions.toc`
2. 将md文件进行转义
3. 目录美化

## 19 增加评论功能

> 单独写在comment.html里 先写评论功能

1. 评论信息的读取。注意一些细节（减少数据库读写次数）
2. 发表评论
3. 客户端验证和服务器端验证
4. csrf验证，防止跨站请求伪造（Cross-site forgery)
5. 表单的使用
6. 评论表单路径配置与表单action功能绑定
7. articleId 获取失败 -> 如何自动获取: action里绑定articleId ` action="{% url 'comment_post' %}?id={{ article.id }}" `

- 技术点2： 回复评论功能

1. 使用Modal，但是Bootstrap的Modal只加载一次，需要在JS里remove，再重新add。



## 20 登录功能

> 注册/登录/注销功能

1. django.contrib.auth(logout,login,authenticate)
2. django.contrib.auth.hashers(make_password) django的加密方式
3. 使用redirect进行跳转
4. 模板在如何判断用户是否登陆
5. django 和 bootstrap等前端模板的相互融合

## 21 ContactMe自动发邮件功能

1. 安装包`pip install flask -mail`
2. 开启邮箱POP3/SMTP服务
3. 增加settings.py中的相关配置
4. 在views.py里进行函数调用
5. 防止表单重新提交-> 用重定向解决

## SPECIAL 一些问题

- Pycharm Community 对Django没有support，因此需要升级成Pycharm Professional版本
- 升级过后 开启项目设置中对django的支持(如下图)

![DjangoSupport](/img/img_in_posts/PythonBlog/DjangoSupport.png)

- Bootstrap下拉菜单失效问题
  JS导入不全，必须按照以下顺序
  ```html
  <!-- JS顺序不可变 -->
  <script src="{% static 'js/jquery-3.6.0.min.js' %}"></script>
  <script src="{% static 'js/bootstrap.js' %}"></script>
  <!-- Bootstrap core JS-->
  <script src="{% static 'js/bootstrap.bundle.min.js' %}"></script>
  <!-- Core theme JS-->
  <script src="{% static 'js/scripts.js' %}"></script>
  ```