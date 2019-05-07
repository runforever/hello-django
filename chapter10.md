# 分页功能开发

这一章我们要为博客添加分页功能，Django 有 [Paginator](https://docs.djangoproject.com/zh-hans/2.2/topics/pagination/) 组件可以实现分页功能，同时也有 [django-el-pagination](https://github.com/shtalinberg/django-el-pagination) 第三方库的实现，编码前我们要尽可能多的找解决方案，了解每种方案的优缺点，最后结合实际场景选择最优的方案，这里 `django-el-pagination` 可以更快速和通用的解决我们的问题。

## 使用 django-el-pagination
根据 django-el-pagination 的 [Getting started](https://django-el-pagination.readthedocs.io/en/latest/start.html) 文档，在 settings 的 `INSTALLED_APPS` 添加 `el_pagination`，打开 `djblog/templates/article/index.html` 添加如下代码：
```html
{% load el_pagination_tags %}
{% paginate 10 article_list %}
{% for article in article_list %}
<article class="blog-entry">
...
</article>
{% endfor %}

<ul class="pagination">
  {% show_pages %}
</ul>
```
这里配置了一页显示 10 篇文章，完成后打开 django 调试服务器测试一下吧。

## 总结
这一章和上一章演示使用了 Django 的第三方组件解决问题，编程中我们容易给自己限定边界条件，例如学习 Django 就只用 Django 组件解决问题，如果我们跳出这个盒子，也许你会看到更多东西。
![out of box](http://cdn.defcoding.com/6DA95E94-5296-4FB4-8DC6-2DC67E3D1CD5.png)
