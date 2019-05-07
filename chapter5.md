# 使用 Django View 展示文章列表

开始之前先认识一下 Web 开发是什么，Web 开发的问题领域是什么。

## Web 开发问题模型
Web 开发基于 [HTTP 协议](https://baike.baidu.com/item/HTTP)，HTTP 协议处理的问题抽象为客户端发送 Request（请求） 和服务器返回 Response（响应），如下图：

![web request&response](http://cdn.defcoding.com/D43F37A1-BB69-41BB-8129-195CA16AD425.png)

## Django 处理 Request 和 Response
Django 接收到 Request 后会经过 URLconf -> View -> Model，然后返回 Template Response，如下图白色部分：
![django request&response](http://cdn.defcoding.com/8D1E6A6B-E18D-49E9-BFBC-A80CF5018B5C.png)

## 博客项目的 Request 和 Response

![blog request&response](http://cdn.defcoding.com/76BFC046-8E3B-46E8-B8F0-0C31376A790F.png)

上面的图片展示了 Django 的 URLConf、View、Model、Template 组件是如何接收 Request 和返回 Response 的流程，下面我们重点介绍一下 Django 中的 MTV 模式和相关组件。

## Django 的 MTV 模式

1. M 代表 Model，即 Django 的模型组件，负责和数据库交互（CURD）。
2. T 代表 Template，即 Django 的模板组件，负责 HTML 网页的数据显示。
3. V 代表 View，即 Django 的视图组件，负责获取 Model 数据并将数据传递给 Template 组件显示。

他们的关系如下图：

![MTV](http://cdn.defcoding.com/475F253E-4C47-41BA-9222-45B76202EC9F.png)

从 Django 的 [设计理念](https://docs.djangoproject.com/zh-hans/2.2/misc/design-philosophies/) 中提到了松耦合，即上面的 Model、Template、View 三个组件各司其职，每个组件只做好自己该做的事，组件之间不关心对方的实现细节。

## 使用 Django View 展示文章列表
使用 Django View 有两种风格：

1. function-based view（基于函数展示）。
2. class-based view（基于类展示）。

由于类比函数容易组织代码，教程里使用的 View 都是 class-based view，现在我们使用 View 组件从 Model 获取文章列表数据并将数据返回给文章列表 HTML 模板，模板使用 `article/list.html`

现在打开 `djblog/app/article/views.py`，编写如下代码：
``` python
from django.shortcuts import render
from django.views import View

from .models import Article


class ArticleListView(View):

    def get(self, request, *args, **kwargs):
        article_list = Article.objects.all()
        return render(request, 'article/list.html', {'article_list': article_list})
```

## 设计入口 URL
View 的逻辑编写完成后，需要添加一个 url 入口才能访问到，风格上 Django 提倡使用简洁、优雅的URL 模式，规则上 Django 使用正则表达式来设计 URL，文档参考 [URLConf](https://docs.djangoproject.com/zh-hans/2.2/topics/http/urls/)：

打开 `djblog/urls.py` 添加下面的代码：
``` python
from django.conf.urls import include, url
from django.contrib import admin

from djblog.app.article import views as article_views

urlpatterns = [
    url(r'^admin/', admin.site.urls),
    url(r'^article/index/$', article_views.ArticleListView.as_view()),
]
```

使用 `python manange.py runserver` 运行调试服务器，访问：[http://127.0.0.1:8000/article/index/](http://127.0.0.1:8000/article/index/) 查看文章列表显示是否正常。

![article list](http://cdn.defcoding.com/1AC604E3-C5AC-4FBC-842A-EC10A7AB3C92.png)

## 使用 Django View 展示文章详情
文章详情 View 组件需要从 URL 获取文章 id，通过 Django ORM 获取 Article Model 相应 id 的数据并传给 HTML 模板，模板使用 `article/detail.html`。

打开 `djblog/app/article/views.py`，添加如下代码：
``` python
from django.shortcuts import render, get_object_or_404


class ArticleDetailView(View):

    def get(self, request, pk, *args, **kwargs):
        article = get_object_or_404(Article, id=pk)

        return render(
            request,
            'article/detail.html',
            {
                'article': article
            }
        )
```
`get` 函数中 `pk` 代表 primary key（数据库主键），从文章详情的 URLPattern 中获取。

[get_object_or_404](https://docs.djangoproject.com/zh-hans/2.2/topics/http/shortcuts/#django.shortcuts.get_object_or_404) 函数作用是根据查询条件如果数据存在则返回数据，数据不存在则返回 HTTP 404 错误。

## 文章详情 URL 设计
文章表使用 id 为主键，文章详情的 URL 模式设计为 `r'/article/detail/(?P<id>\d+)/$'`，其中 `id` 为动态 URL 部分，表示匹配所有整型数据，如：`/article/detail/1/`、`article/detail/2/` 等等。

打开 `djblog/urls.py` 在 `urlpatterns` 添加下面的代码：
``` python
urlpatterns = [
    url(r'^article/detail/(?P<pk>\d+)/$', article_views.ArticleDetailView.as_view()),
]
```

通过点击首页查看博客详情按钮查看访问详情页是否正常：
![article detail](http://cdn.defcoding.com/D7BC8D63-28AF-4655-B60D-DC15744F8984.png)

## 优化 article app 的 urls
设计模式提倡高内聚，低耦合，目前文章展示的 url 配置是放在 `djblog/urls.py` 文件中的，为了提高内聚性，可以将文章的 URL 配置迁移到 `article app` 中。

在 `djblog/app/article` app 中新建 `urls.py` 文件，打开 `djblog/app/article/urls.py` 编写代码：
```python
from django.conf.urls import url
from . import views

urlpatterns = [
    url(
        r'^index/$',
        views.BlogIndexView.as_view(),
        name='index'
    ),
    url(
        r'^detail/(?P<pk>\d+)/$',
        views.ArticleDetailView.as_view(),
        name='detail'
    ),
]
```

将 `djblog/urls.py` 中的代码修改为：
```python
from django.conf.urls import url, include
from django.contrib import admin

urlpatterns = [
    url(r'^admin/', admin.site.urls),
    url(r'^article/', include('djblog.app.article.urls')),
]
```

优化后，文章相关的代码都在 `article app` 中了，以后任何文章相关的改动只需要到这个 app 中维护即可。

## 总结
本章通过图片的形式展示了 Web 开发问题的本质，理解了本质对于我们以后解决问题和思考问题提供了方向，遇到问题我们可以从 HTTP 协议出发，例如：认证、cookie、session 等 Django 组件都是为了解决协议中相应的问题。

本章相关问题请到这个 [Issue](https://github.com/runforever/djblog/issues/5) 讨论。
