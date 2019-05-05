# 使用 Django Admin 管理博客文章

博客文章管理功能可以使用 Django 的 Admin 组件来完成。

## 配置 Django Admin 管理账号
1. 进入项目并且激活 virtualenv：`cd djblog && source .venv/bin/activate`
2. 使用 createsuperuser 指令创建管理账户：`python manage.py createsuperuser`
3. 运行开发服务器：`python manage.py runserver`
3. 访问 [http://127.0.0.1:8000/admin](http://127.0.0.1:8000/admin) 输入刚刚创建的账号密码进入管理界面

![django admin](http://cdn.defcoding.com/127718BE-B6A6-4A52-AE93-812EDF672F61.png)

## Django App 介绍
Django 使用 app 来组织项目文件，一个 Django app 有如下文件：
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

通常按功能模块建立相应的 app 目录，如下图：

![djblog app](http://cdn.defcoding.com/D62ADCF7-2563-4923-9158-BB5BA0329536.png)

## 创建文章管理 app
Django 提供了 `startapp` 命令创建 app，创建步骤：

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

app 创建完成就可以在相应的文件里编写代码了，在这之前，我们还需要先了解 Django ORM。

## Django ORM 介绍
对数据库表进行正删改查通常是使用 SQL 语言，SQL 代码相对来说较难维护，ORM，即 Object-Relational Mapping（对象关系映射），主要解决的是对象和关系的映射。它把一个类和一个表一一对应，类的每个实例对应表中的一条记录，类的每个属性对应表中的每个字段，ORM 提供了对数据库的映射，不用直接编写 SQL 代码，只需像操作对象一样从数据库操作数据，让软件开发人员专注于业务逻辑的处理，提高了开发效率，ORM 和数据库的关系如下：

![django orm](http://cdn.defcoding.com/33ACB617-84B4-4BE7-887B-794DD8CB620F.png)

Django 使用 Model 组件定义与数据库对应的类。

## 定义博客文章 Model
通常一篇文章会包含如下字段：

1. 文章标题
2. 文章内容
3. 文章作者
4. 文章的创建时间
5. 文章的更新时间

参考 [Django Model](https://docs.djangoproject.com/zh-hans/2.2/topics/db/models/) 文档，打开 `djblog/app/article/models.py` 定义文章 model：
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

定义好了数据库模型，要将模型映射到数据库中还需要以下步骤：

1. 使用 `makemigrations` 命令新增数据库变更文件，在 `djblog` 项目根目录运行：`python manage.py makemigrations`
2. 使用 `migrate` 命令将生成的数据库变更应用到数据库中：`python manage.py migrate`

执行完成后数据库就有 article 这张表了，在 SQLite 中查看
![article table](http://cdn.defcoding.com/2A008739-3203-41CA-A308-81FAB2EABADE.png)

## Django Admin 中集成文章管理
参考 [Django Admin 文档](https://docs.djangoproject.com/zh-hans/2.2/ref/contrib/admin/#modeladmin-objects)，打开 `djblog/app/article/admin.py` 文件，添加下面代码：
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
1. 本章介绍了 Django Admin、Django Model 和 ORM 的概念和基本用法，更多的使用细节需要读者自己去 API 文档中研究，有一个经验是读者应该对 API 中每个知识都有所了解，不必死记，要解决问题的时候就能快速联想到相应的技术，如：了解 Django Model 中每种 ModelField 类型，在需要上传图片的场景，我们就使用 ImageField 解决。
2. Django Admin 能大大提高录入数据和调试效率，即使项目不用 Django Admin 管理数据，我们也可以合理的运用它提高工作效率。
3. 用 Django 有个不得不说的思考方式，在 Web 开发中要解决的问题大多数已经有人解决过并且已经形成通用组件了，在敲代码之前可以发挥一下联想能力在 Google 中搜索 “Django 如何解决 XXX 问题” 会有意想不到的收获，我经常用这个办法快速的使用 Django 组件或 Django 第三方库解决问题。

## 代码和讨论
1. 本章的代码位于 ...
2. 本章的问题请到这个 [Issue](#) 讨论。
