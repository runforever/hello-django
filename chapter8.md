# 文章分类功能开发

目前为止我们使用 Django 的 Admin、Model、View、Template 组件完成了博客文章的 CURD 功能，接下来我们要添加文章分类功能。

## 文章分类功能需求分析
总体来说，编码前的计划和设计是可以提高我们的开发效率的：

1. 将分类和文章的数据库关系设置为一对多，一个分类下可以有多篇文章。
2. 使用 Django Admin 来实现文章分类的增、删、改功能。
3. 博客主页的文章分类展示功能。
4. 点击分类后展示该分类下的所有文章。
5. 由于文章分类功能比较简单，不需要单独创建分类 Django app，继续在 article app 中编码即可。

需要完成的功能在界面的侧边栏：
![article category](http://cdn.defcoding.com/ACEA5F7C-D77E-4577-B98D-903A281511B9.png)

## 文章分类的数据库表设计
1. 文章分类所需字段：分类名。
2. 为了表示与文章的一对多关系，需要在 Ariticle Model 添加分类字段，使用 ForeignKey。

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
打开 `djblog/app/article/views.py` 在 `BlogIndexView` 中添加获取分类的代码:
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
在章节 [使用 Django View 展示博客文章](chapter5.md) 中介绍过 Web 开发就是解决 request 和 response 问题，为了筛选分类下的所有文章，可以使用 HTTP 协议中的 GET 参数在 URL 中带上分类的 category id，BlogIndexView 中获取 category id 并筛选出相应的数据，URL 的格式如下：`/article/index/?category=1`

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
这一章用了很少的代码就把将分类功能开发完了，有没有感觉用 Django 开发很简单呢？

本章问题通过 [Issue](#) 来讨论。
