# 文章标签功能开发

很多博客都有标签云功能，点击标签后可以查看带有相应标签的文章，标签云功能跟文章分类功能类似，在章节 [文章分类功能开发](chapter8.md) 中我们将分类和文章的数据库表关系设计为一对多，这里我们将文章标签和文章的关系设计为多对多，即一个文章标签可以对应多篇文章，一篇文章可以包含多个标签。

标签云功能业务不复杂，在 article app 中编码即可，实现标签云所需要的组件读者已经学过，这一章编码工作由读者完成，完成后通过 GitHub 的 pull request 将代码发送给我 Review。

## 标签云表设计
标签的字段内容如下：

1. 标签名。
2. 文章表添加标签字段，使用 ManyToManyField 来表示多对多关系。

## Django Admin 中添加文章标签管理
参照 [使用 Django Admin 管理博客文章](chapter4.md) 中的教程添加。

## 使用 Django View 将标签显示出来
BlogIndexView 和 BlogDetailView 中添加使用 Django ORM 获取所有文章标签的代码，模板变量名是 `tag_list`。

## 改进获取文章标签代码
BlogIndexView 和 BlogDetailView 中添加相同的获取标签的代码，根据 DRY 原则，我们使用 Python Mixin 机制改进：
```python
class ArticleTagMixin(object):

    def get_tags(self):
        return Tag.objects.all()


class BlogIndexView(View, ArticleTagMixin):

    def get(self, request, *args, **kwargs):
        article_list = ArticleFilter(request.GET, queryset=Article.objects.all()).qs
        category_list = Category.objects.all()
        tag_list = self.get_tags()

        return render(
            request,
            'article/index.html',
            {
                'article_list': article_list,
                'category_list': category_list,
                'tag_list': tag_list,
            }
        )
```
之后获取文章标签有改动，只需要改动 `ArticleTagMixin` 这一个地方。

## 发布代码
TODO

## github 上提交 pull request
编码完成后将代码提交到 github，点击 github 界面的 `new pull request` 按钮，填写 pull request 内容，确认后我就可以收到 pull request 请求了。
![pull request](http://cdn.defcoding.com/79446838-2733-43EE-8A9B-1F3DAE6A0184.png)

## 总结
功能完成后我们要及时发现重复或者让人觉得隐晦的代码，重构这些代码提高代码的可读性和可维护性，后面接手的人都会感谢你的。
