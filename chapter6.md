# 使用 Django View 展示文章详情

上一章我们使用 Django View 把博客文章列表显示出来了，接下来我们使用 Django View 将文章的详情页显示出来。

## 文章详情页 URL 设计
数据库中的文章表 id 为主键，所以我们的 URL pattern 可以设计为 `r'/article/(?P<id>\d+)/$'`，其中 `id` 为动态 URL 部分，Django 使用正则表达式来完成动态 URL 的功能。

打开 `djblog/urls.py` 在 `urlpatterns` 添加下面的代码：
``` python
urlpatterns = [
    url(r'^article/(?P<pk>\d+)/$', article_views.ArticleDetailView.as_view()),
]
```

## 文章详情展示 View 代码
文章详情 View 组件需要从 URL 获取文章 id，通过 Django ORM 获取 Article Model 相应 id 的数据并传给 HTML 模板。

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

使用 `python manange.py runserver` 启动调试服务器，访问：[http://127.0.0.1:8000/article/list/](http://127.0.0.1:8000/article/1/) 即可打开文章列表。

## 优化 article app 的 urls
TODO
