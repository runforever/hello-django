# 使用 Django Form 提交文章评论

这一章我们学习使用 Django Form 提交文章评论。

## 方案调研
前面的章节都是从 Django View 获取数据展示，使用的是 HTTP 协议的 GET 方法，现在我们要提交数据给 Django View，通常的做法是使用 HTTP 的 POST 方法提交数据，这里抛一个问题，GET 方法和 POST 方法的区别，请读者使用 Google 了解。

HTML 提供了 Form 标签来提交数据，Django 提供了 Form 组件来接收数据，Django Form 组件的作用是：

1. 校验用户提交的数据是否有问题。
2. Form 组件提供 save 方法将数据保存到数据库中。

发挥一下联想能力，提交表单这个问题是 Web 开发的一种通用模式，那么 Django 是不是提供了相应的组件解决这个问题呢？答案是：Yes

## 使用 Django ModelForm、
Django 提供了 [ModelForm 组件](https://docs.djangoproject.com/en/2.2/topics/forms/modelforms/)，方便我们从 Model 直接生成 Form。
```python
from django.forms import ModelForm
from .models import Comment


class CommentForm(ModelForm):

    class Meta:
        model = Comment
        fields = (
            'article',
            'nickname',
            'email',
            'content',
        )
```

## 编写 Django View
使用 post 方法接收 HTML 提交的评论数据。
```python
class CommentView(View):

    def post(self, request, **kwargs):
        form = CommentForm(request.POST)

        if form.is_valid():
            comment = form.save()
            return redirect('detail', pk=comment.article_id)

        return HttpResponse(form.errors)
```

## 提交评论入口 url
打开 `djblog/app/article/urls.py` 添加下面代码到 urlpatterns 中：
```python
    url(
        r'^comment/$',
        views.CommentView.as_view(),
        name='comment'
    ),
```

运行 Django Server，填写评论内容后进行测试：
![post comment](http://cdn.defcoding.com/544C8499-D36E-49B6-A8B9-57B9D721EBF2.png)

## 使用 Django Generic View
Django 提供了 [FormView 和 CreateView](https://docs.djangoproject.com/en/2.2/topics/class-based-views/generic-editing/) 这两个通用视图，读者根据文档使用通用视图重构提交评论代码，比较一下非通用视图和通用视图的区别。

如有需求请提交 pull request 给我进行 code review。

## 总结
编程中很多时候需要我们发挥联想能力和想象力，在 [《程序员思维修炼中》](https://book.douban.com/subject/5372651/) 作者提到过：专家是靠直觉解决问题的，新手在学习过程中，除了努力学习基础知识外，也要学学专家思维，当然一切要以科学为基础，天马行空的想象力反而是浪费时间，编码前调研方案是很有帮助的。

+ 本章代码位于 Git 分支 `feature-django-form`
+ 提交评论相关代码到 [Issue 提交评论](https://github.com/runforever/djblog/issues/13)
