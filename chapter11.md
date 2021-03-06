# 使用 Django Generic Views 重构

Web 开发大多是解决数据的 CURD 问题，Django Generic Views（通用视图）则是解决数据 CURD 的通用 View 组件。

## 使用通用视图重构文章详情展示
在章节 [使用 Django View 展示文章](chapter6.md) 中，处理文章详情展示的 View 代码是：
```python
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

使用通用视图重构后：
```python
from django.views.generic.detail import DetailView


class ArticleDetailView(DetailView):
    model = Article
    template_name = 'article/detail.html'
    context_object_name = 'article'
```
使用通用视图只需要配置几个参数就实现了同样的功能，后续的开发任务我们都可以思考通用视图能否胜任，毕竟偷懒是程序员的美德，同样的功能更少的代码何乐而不为呢？更多通用视图请查看文档 [generic-view](https://docs.djangoproject.com/zh-hans/2.2/topics/class-based-views/generic-display/)

+ 本章代码位于 Git 分支 `feature-generic-views`。
+ 通用视图相关问题通过 [Issue 通用视图](https://github.com/runforever/djblog/issues/9) 讨论。
