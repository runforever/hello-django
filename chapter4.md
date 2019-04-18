# 使用 Django Admin 管理博客文章

## Django 由来
来自 [Django FAQ](https://docs.djangoproject.com/zh-hans/2.2/faq/general/)，Django 从一个非常实际的需求成长而来：World Web 是一家新闻网站，负责在新闻截止期限内建立密集的 Web 应用程序。 在快节奏的新闻编辑室，World Online 往往需要几个小时内将一个复杂的 Web 应用程序从概念推向发布上线。

与此同时，World Online 的 Web开发者在 Web 开发的最佳实践方面一直是完美主义者。

在2003年秋季，世界在线开发者（Adrian Holovaty 和 Simon Willison）放弃 PHP 而开始使用 Python 开发网站。 当他们构建了密集的，强互动性的网站时（如 Lawrence.com），他们开始提取一个通用的 Web 开发框架，使他们可以更快地构建 Web 应用程序。 两年内他们经常调整和改进这个框架。

从 Django 由来可以知道它是一个通用的 Web 开发框架，自然集成了解决 Web 开发问题的通用组件，我们要开发的管理博客文章功能就可以使用 Django 的 Admin 组件来完成。

关于使用 Django 有个不得不说的思考方式，在 Web 开发中要解决的问题大多数已经有人解决过并且已经形成通用组件了，我们在敲代码之前可以发挥一下联想能力在 Google 中搜索 XXX Web 开发框架是否包含 XXX 组件，我经常用这个办法达到事半功倍的效果。

## 配置 Django Admin 管理账号
1. 进入项目并且激活 virtualenv：`cd djblog && source .venv/bin/activate`
2. 使用 createsuperuser 指令创建管理账户：`python manage.py createsuperuser`
3. 运行开发服务器：`python manage.py runserver`
3. 访问 [http://127.0.0.1:8000/admin](http://127.0.0.1:8000/admin) 输入刚刚创建的账号密码进入管理界面

![django admin](http://cdn.defcoding.com/C68E610C-93FA-4B30-BDD8-6A3A239015A0.png)

## 创建文章管理 app
前面的架构设计我们定义好了项目的目录结构，在开始做文章管理相关功能前我们需要将相关目录和文件创建好，Django 提供了 `startapp` 命令方便我们创建功能模块 app。

1. 进入到项目 app 文件夹，`cd djblog/app`
2. 使用 django-admin.py 创建 article（文章）app，`django-admin.py startapp article`
3. 将 article app 添加到 Django settings 的 `INSTALLED_APPS` 中

``` python
INSTALLED_APPS = (
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'django.contrib.admin',

    # djblog app
    'djblog',
    'djblog.app.article',
)
```

打开 article 目录有如下文件：
``` python
├── __init__.py
├── admin.py    # Django Admin 配置文件
├── apps.py
├── migrations  # 数据库表变更文件
│   └── __init__.py
├── models.py   # 数据模型设计文件
├── tests.py    # 单元测试文件
└── views.py    # 视图文件
```

## 定义文章 Model
我们需要使用 Django Model 组件定义文章的模型，即文章的数据库表设计，通常一篇文章会包含如下字段：

1. 文章标题
2. 文章内容
3. 文章作者
4. 文章的创建时间
5. 文章的更新时间

下面就要激动人心的编码环节，打开 `djblog/app/article/models.py` 定义文章 model：
```python
from django.db import models

class Article(models.Model):

    title = models.CharField(
        '标题',
        max_length=50,
    )
    content = models.TextField(
        '内容',
    )
    author = models.ForeignKey(
        'auth.User',
        verbose_name='作者',
        on_delete=models.CASCADE,
    )
    created_at = models.DateTimeField(
        '创建时间',
        auto_now_add=True,
    )
    updated_at = models.DateTimeField(
        '更新时间',
        auto_now=True,
    )

    def __str__(self):
        return self.title

    class Meta:
        verbose_name = '文章'
        verbose_name_plural = verbose_name
```

我们定义好了数据库模型，但是数据库中并没有生成相应的表，这时需要使用 `makemigrations` 命令新增数据库变更文件，在 `djblog` 项目根目录运行：`python manage.py makemigrations`

使用 `migrate` 命令将生成的数据库变更应用到数据库中：`python manage.py migrate`

命令执行完成后数据库就有 article 这张表了，在 SQLite 中查看
![article table](http://cdn.defcoding.com/2A008739-3203-41CA-A308-81FAB2EABADE.png)

## 将文章添加到 Django Admin 中
打开 `djblog/app/article/admin.py` 文件，添加下面代码：
``` python
from django.contrib import admin
from .models import Article

admin.site.register(Article)
```

刷新 [http://127.0.0.1:8000/admin](http://127.0.0.1:8000/admin)，文章管理模块就在 Admin 里了
![admin article](http://cdn.defcoding.com/FB0C8F2C-1D8F-4396-B7A6-88117A09D504.png)

点击“文章”就可以实现文章的新增、编辑和删除了
![manage article](http://cdn.defcoding.com/68F34AAF-1BD7-49A7-9CAD-4CB12C3FA693.png)

## 总结
1. 本章主要介绍了 Django Admin 的使用，后续的文章分类功能开发、留言功能的管理都需要使用到 Django Admin。
2. 本章还介绍了如何生成 Django App 和 Django Model，相比大家有疑惑 `CharField`、`TextField` 是什么，这些还需要读者自己花功夫查看 Django 官方 [Model API](https://docs.djangoproject.com/zh-hans/2.2/topics/db/models/#fields)，教程会告诉大家每个组件的用途，但是细节只有官方 API 是最权威的。

## 作业
目前 Admin 界面使用的语言是英文的，请将 Google 搜索如何将英文改为中文。

## 代码和讨论
1. 本章的代码位于 ...
2. 本章的问题请到这个 [Issue]() 讨论。
