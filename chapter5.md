# 使用 Django View 展示文章列表

开始之前先认识一下 Web 开发是什么，Web 开发的问题领域是怎样的。

## Web 开发问题模型
Web 开发基于 [HTTP 协议](#)，HTTP 协议处理的问题抽象为客户端发送 Request（请求） 和服务器返回 Response（响应），如下图：

![web request&response](http://cdn.defcoding.com/D43F37A1-BB69-41BB-8129-195CA16AD425.png)

## Django 处理 Request 和 Response
Django 接收到 Request 后会经过 URLconf -> View -> Model，然后返回 Template Response，如下图白色部分：
![django request&response](http://cdn.defcoding.com/8D1E6A6B-E18D-49E9-BFBC-A80CF5018B5C.png)

## 博客项目的 Request 和 Response

![blog request&response](http://cdn.defcoding.com/76BFC046-8E3B-46E8-B8F0-0C31376A790F.png)

上面的图片展示了 Django 的 URLConf、View、Model、Template 组件是如何接收 Request 和返回 Response 的流程，下面我们重点介绍一下 Django 中的 MTV 模式和相关组件。

## Django 的 MTV 模式

1. M 代表 Model，即 Django 的模型组件，负责 Model 和数据库之间的交互（CURD）。
2. T 代表 Template，即 Django 的模板组件，负责 HTML 网页的数据显示。
3. V 代表 View，即 Django 的视图组件，负责获取 Model 数据并将数据传递给 Template 组件显示。

他们的关系如下图：

![MTV](http://cdn.defcoding.com/475F253E-4C47-41BA-9222-45B76202EC9F.png)

从 Django 的 [设计理念](https://docs.djangoproject.com/zh-hans/2.2/misc/design-philosophies/) 中提到了松耦合，即上面的 Model、Template、View 三个组件各司其职，每个组件只做好自己该做的事，组件之间不关心对方的实现细节。

## 设计入口 URL
TODO URL 作用和设计

## 编写文章列表展示 View 代码
现在我们要使用 View 组件从 Model 获取文章列表数据并将数据返回给文章列表 HTML 模板。

前一章章节我们创建好了 `article app`，现在打开 `djblog/app/article/views.py`，编写如下代码：
``` python
from django.shortcuts import render
from django.views import View

from .models import Article


class ArticleListView(View):

    def get(self, request, *args, **kwargs):
        article_list = Article.objects.all()
        return render(request, 'article/list.html', {'article_list': article_list})
```

View 的逻辑编写完成后，需要添加一个 url 入口才能访问到，打开 `djblog/urls.py` 添加下面的代码：
``` python
from django.conf.urls import include, url
from django.contrib import admin

from djblog.app.article import views as article_views

urlpatterns = [
    url(r'^admin/', admin.site.urls),
    url(r'^article/list/$', article_views.ArticleListView.as_view()),
]
```

使用 `python manange.py runserver` 启动调试服务器，访问：[http://127.0.0.1:8000/article/list/](http://127.0.0.1:8000/article/list/) 即可打开文章列表。

![article list](http://cdn.defcoding.com/1F23A15A-34A7-4B07-A20C-E2F49F2711DB.png)

## 总结
1. Django 的 MTV 模式和 Web 开发问题模型要理解是怎么一回事。
2. View 组件的使用和 ORM 使用方法需要读者自己后续下功夫查询官方文档。

本章相关问题请到这个 [Issue](https://github.com/runforever/djblog/issues/5) 讨论。
