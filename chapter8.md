# 文章分类功能开发

目前为止我们使用 Django 的 Admin、Model、View、Template 组件完成了博客文章的 CURD 功能，接下来我们要为文章添加分类功能。

## 文章分类功能需求分析
写代码之前我们要明白做的功能是什么，思考如何去做，使用哪些组件，总体来说，好的计划和设计是可以提高我们的开发效率的。

1. 从文章分类数据库设计考虑，一个分类下可以有多篇文章，一篇文章也可以属于多个分类，即分类和文章的数据库表关系多对多，为了简化要做的功能，我们可以将文章和分类的关系设置为一对多。
2. 文章分类的增、删、改功能，我们使用 Django Admin 来实现。
3. 主页的文章分类展示功能，我们使用 Django ORM queryset 来查询。
4. 点击分类后展示该分类的文章，我们使用 Django ORM queryset 来查询。
5. 文章分类功能比较简单，我们继续在 article app 中编码即可，不需要再创建单独的 app 来放分类代码。

## 文章分类的数据库表设计
1. 文章分类需要包含的字段只有：分类名。
2. 为了表示与文章的一对多关系，我们只需要在 Ariticle Model 添加分类的 ForeignKey 字段。

分类 Model 代码实现，打开 `djblog/app/article/models.py`:
```python
class Category(models.Model):
    """
    文章分类
    """

    name = models.CharField(
        '分类名',
        max_length=50,
    )

    def __str__(self):
        return self.name

    class Meta:
        verbose_name = '文章分类'
        verbose_name_plural = verbose_name


# 文章表中添加分类字段
class Article(models.Model):
    ...
    category = models.ForeignKey(
        'article.Category',
        verbose_name='分类',
        null=True,
        blank=True,
        on_delete=models.SET_NULL,
    )
    ...
```

执行 `python manage.py makemigrations` 和 `python manage.py migrate` 将 Model 的变更应用到数据库中。

## 文章分类添加的 Admin 管理
打开 `djblog/app/article/admin.py`
```python
from .models import (
    Article,
    Category,
)

admin.site.register(Category)
```

执行 `python manage.py runserver` 运行 Django 开发 Server，浏览器中访问 `http://127.0.0.1:8000/admin/` 即可查看到分类管理

![blog category](http://cdn.defcoding.com/53049493-9E2B-4737-9E71-23820BB294B7.png)

## 在博客主页中展示所有分类
打开 `djblog/app/article/views.py`:
```python
class BlogIndexView(View):

    def get(self, request, *args, **kwargs):
        article_list = Article.objects.all()
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
浏览器访问 `http://127.0.0.1:8000/article/index/` 查看

![index category](http://cdn.defcoding.com/EA069CD8-F0F8-4966-8239-ABE2C47AF4FC.png)

## 展示分类下的文章
当点击博客分类后，我们需要筛选该分类下的文章展示出来，在章节 [5.使用 Django View 展示博客文章列表](chapter5.md) 中介绍过 Web 开发就是解决 request 和 response 问题，这里我们需要在 URL 中带上分类的 category id，BlogIndexView 中获取 category id 并筛选出相应的数据，URL 的格式如下：`/article/index/?category=1`

BlogIndexView 处理逻辑如下：
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
代码写完后我们测试一下功能是否正常
![category article](http://cdn.defcoding.com/F466A06A-3C61-4232-937B-FE3F6DE8614F.png)

## 总结
这一章我们将分类功能开发完了，你有没有感觉用 Django 开发很简单呢？

本章问题通过 [Issue](#) 来讨论。