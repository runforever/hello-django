# 使用 django-filter 库重构文章分类筛选

先回顾上一章写的根据分类筛选文章的代码，重点关注处理 category_id 的部分代码：
```python
class BlogIndexView(View):

    def get(self, request, *args, **kwargs):
        article_list = Article.objects.all()
        category_list = Category.objects.all()

        # category_id 处理
        category_id = request.GET.get('category')
        try:
            category_id = int(category_id)
        except (ValueError, TypeError):
            category_id = None

        if category_id:
            article_list = article_list.filter(category_id=category_id)

        return render(
            request,
            'article/index.html',
            {
                'article_list': article_list,
                'category_list': category_list,
            }
        )
```
现在新增几个文章筛选需求：

1. 通过创建时间年筛选文章，如：2019 年所有的文章。
2. 通过创建时间年月筛选文章：如：2019 年 1 月的文章。
3. 通过文章标题关键词搜索：如：搜索所有包含 Django 关键词的文章标题。

实现上面几个需求，可以写类似代码中的筛选 category_id 那样的代码，通过 URL 传递相应的筛选参数给 View，View 中处理文章筛选，如果这样做，我们的 View 函数会变得十分臃肿庞大，虽然可以实现功能，但是对于代码的可读性和可维护性来说都太差了。

更好的做法是，首先要识别这是一种通用的模式，即：获取 request.GET 参数，然后通过参数筛选 Model，最后返回 queryset，因此我们可以设计一个筛选工具类或者函数，专门处理这样的筛选问题，然而，已经有人为了解决这个问题开发了 Django 的第三方库 [django-filter](https://django-filter.readthedocs.io/en/latest/guide/usage.html) 。

## 使用 django-filter 重构代码
查看 django-filter 的文档 [FilterSet](https://django-filter.readthedocs.io/en/latest/ref/filterset.html)，先定义文章筛选类 `ArticleFilter`，打开 `djblog/app/article/filters.py`：
```python
import django_filters

from .models import Article


class ArticleFilter(django_filters.FilterSet):

    class Meta:
        model = Article
        fields = {
            'category': ['exact'],
            'created_at': ['year', 'month'],
            'title': ['icontains'],
        }
```

在 `BlogIndexView` 中使用 `ArticleFilter`，打开 `djblog/app/article/views.py`，修改代码：
```python
from .filters import ArticleFilter


class BlogIndexView(View):

    def get(self, request, *args, **kwargs):
        # 使用 ArticleFilter 做筛选
        article_list = ArticleFilter(request.GET, queryset=Article.objects.all()).qs
        category_list = Category.objects.all()

        return render(
            request,
            'article/index.html',
            {
                'article_list': article_list,
                'category_list': category_list,
            }
        )
```

使用 django-filter 重构后代码变得简单明了，更容易扩展，有新的筛选需求只需要修改 `ArticleFilter` 即可，这个方案显然更优雅。

运行 `python manage.py runserver`，浏览器访问以下网址做测试：

1. `http://127.0.0.1:8000/article/index/?category=1`
2. `http://127.0.0.1:8000/article/index/?created_at__year=2019`
3. `http://127.0.0.1:8000/article/index/?created_at__year=2019&created_at__month=4`
4. `http://127.0.0.1:8000/article/index/?title__icontains=第一`

由于各自开发环境文章内容不一，请替换参数内容做测试。

## 总结
1. Django 有很多第三方库，开发过程中我们要学会识别问题类型，然后通过 Google 去找有没有现成方案帮助我们快速解决问题，不要自己重复造轮子。
2. 阅读英文文档对于编程来说是很重要的技能，英文不好的同学得想办法提高。

## 代码和讨论
+ 本章代码位于 Git 分支 `feature-django-filter`。
+ 本章的问题请到 [Issue Django Filter](https://github.com/runforever/djblog/issues/7) 讨论。
