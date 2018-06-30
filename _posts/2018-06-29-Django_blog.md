---
layout: post
cover: assets/images/tianchi.jpg
title: (原)用Django配合mysql搭建个人博客(一)
date: 2018-06-29 21:21:00
tags: code
author: Kim
---
<p>在这里介绍下我那个比较简陋的Django博客吧<a href="http://github.com/qibinjin/Django_blog_0.1" target="_blank">附上Github地址</a>，虽然麻雀很小但是五脏还是很全的，前端的部分参考了实验楼的'Django建立简易博客'课程，基本的功能实现了数据库存储文章，评论，用户注册，登陆，评论。。有很多地方可能处理的不好，所以大神请绕道。。下面是首页效果图</p>
<amp-img width="600" height="252"  src="assets/images/blog.png"></amp-img>

<p>环境是ubuntu配合Django1.8.7.首先要清楚的是Django的目录结构以及各个文件的作用,创建Django的项目我建议使用pycharm在linux和windows下都可以使用，简单方便。下图就是最简单的Django目录结构，其中urls.py是用来管理访问路径与视图函数的绑定的，settings可以设置各种参数，manage.py类似与一个总的管理文件,我们接下来新建app和建立model等都要用到manage.py。</p>
```shell
tree
.
├── blog
│   ├── __init__.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
├── manage.py
└── templates
```
<p>接下来我们新建一个自己的app,cd 到manage.py的目录</p>
```shell
python manage.py startapp article
tree
.
├── article
│   ├── admin.py
│   ├── __init__.py
│   ├── migrations
│   │   └── __init__.py
│   ├── models.py
│   ├── tests.py
│   └── views.py
├── blog
│   ├── __init__.py
│   ├── __pycache__
│   │   ├── __init__.cpython-35.pyc
│   │   └── settings.cpython-35.pyc
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
├── manage.py
└── templates
```
<p>Django提供了一个用于调试的服务命令</p>
```shell
运行
python manage.py runserver 8000
最后接的是端口号
```
<p>如果提示It worked 说明到这里都没有问题！恭喜你！</p>
<p>以上的article目录就是我们新建的app，我是用的mysql做的数据库，由于Django默认是sqlite所以我们需要做一下修改。</p>
```python
#找到settings里面的DATABASES 改成如下即可如果你的mysql的账号密码改过的话就对应改下即可
#NAME代表你要使用的database名，
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'python',
        'USER': 'root',
        'PASSWORD': 'mysql',
        'HOST': 'localhost',
        'PORT': '3306',
    }
}
#接下来还要在你Django项目目录下的init文件夹(非app下)里添加
import pymysql
pymysql.install_as_MySQLdb()

```
<p>接下来就可以着手建立需要的数据库了，在Django里面建立表格很简单只要在models文件里集成models.Model写对应的类即可，打开article目录下的models.py</p>
```python
from django.db import models

class Article(models.Model):
    title = models.CharField(max_length=100)
    category = models.CharField(max_length=50, blank=True)
    date_time = models.DateTimeField(auto_now_add=True)
    content = models.TextField(blank=True, null=True)

    def __str__(self):
        return self.title

    class Meta(object):
        ordering = ['-date_time']#这是代表用时间逆序排序
```
<p>建立好models之后运行</p>
```shell
python manage.py makemigrations
python manage.py migrate
```
<p>现在你可以上mysql查看新建立的表格，命名是article_article你的app名字加上你的类名。你可以向里面插入一些数据。现在我们有了数据想要显示就需要前端界面前端的部分我就不做解释了贴上代自己看下吧。</p>

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>My_Blog</title>
    {% raw %}{% load staticfiles %}{% endraw %}
    <link rel="stylesheet" href="{% raw %}{% static 'article/pure-min.css' %}{% endraw %}">
    {% raw %}{% load staticfiles %}{% endraw %}
    <link rel="stylesheet" href="{% raw %}{% static 'article/grids-responsive-min.css' %}{% endraw %}">
    {% raw %}{% load staticfiles %}{% endraw %}
    <link rel="stylesheet" href="{% raw %}{% static 'article/blog.css' %}{% endraw %}">
    {% raw %}{% load staticfiles %}{% endraw %}
    <script type="text/javascript" src="{% raw %}{% static 'article/wangEditor.js' %}{% endraw %}"></script>
    {% raw %}{% load staticfiles %}{% endraw %}
    <script type="text/javascript" src="{% raw %}{% static 'article/xss.js' %}{% endraw %}"></script>
    {% raw %}{% load staticfiles %}{% endraw %}
    <script type="text/javascript" src="{% raw %}{% static 'article/jquery-1.12.4.min.js' %}{% endraw %}"></script>
    <style type="text/css">
        .toolbar {
            border: 1px solid #ccc;
        }

        .text {
            border: 1px solid #ccc;
            height: 200px;
        }

        .hello {
            float: right;
            position: relative;
            bottom: 30px;
        }

        .username {
            color: black;
            text-transform: none;
        }
    </style>
</head>
<body>
<div id="layout" class="pure-g">
    <div class="sidebar pure-u-1 pure-u-md-1-4">
        <div class="header">

            <h1 class="brand-title"><a href="{% raw %}{% url 'home' %}{% endraw %}">MY_BLOG</a></h1>
            <h2 class="brand-tagline">Snow Memory</h2>
            <nav class="nav">
                <ul class="nav-list">
                    <li class="nav-item">
                        <a class="button-success pure-button" href="/">Home</a>
                    </li>
                    <li class="nav-item">
                        <a class="button-success pure-button" href="/">Archive</a>
                    </li>
                    <li class="nav-item">
                        <a class="button-success pure-button" href="{% raw %}{% url 'pages' %}{% endraw %}">Pages</a>
                    </li>
                    <li class="nav-item">
                        <a class="button-success pure-button" href="/">About me</a>
                    </li>
                    <li class="nav-item">
                        <a href="#" class="pure-button">GitHub</a>
                    </li>
                    <li class="nav-item">
                        <a class="button-success pure-button" href="#">Weibo</a>
                    </li>
                </ul>
            </nav>
        </div>
    </div>
    <div class="content pure-u-1 pure-u-md-3-4">
        <span class="hello"><span id="hello">请</span> <a href="{% raw %}{% url 'login' %}{% endraw %}" id="logged">登陆</a>,&nbsp;<a
                href="{% raw %}{% url 'regist' %}{% endraw %}">注册</a></span>
        <script>
            if (typeof ({{ username }}) != undefined){
                document.getElementById('hello').innerHTML = "Hello,&nbsp;<a href='{% raw %}{% url 'user_info' %}{% endraw %}'>{{ username }}</a>";
                document.getElementById('logged').innerHTML = "<a href='{% raw %}{% url 'logout' %}{% endraw %}'>登出</a>";


            }
        </script>
        <div>
            {% raw %}{% block content %}{% endraw %}
            {% raw %}{% endblock %}{% endraw %}
            <div class="footer">
                <div class="pure-menu pure-menu-horizontal pure-menu-open">
                    <ul>
                        <li><a href="#">About</a></li>
                        <li><a href="#">twitter</a></li>
                        <li><a href="#">GitHub</a></li>
                    </ul>

                </div>

            </div>
        </div>

    </div>
</div>
</body>
</html>

```

<p></p>
<a href="assets/images/tianchi(source).jpg" target="_blank">封面图片的高清大图</a>
