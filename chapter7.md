# 使用 Django Template

目前流行的 Web App 开发架构是前后端分离的架构，后端开发提供数据 API（通常是 RESTful 风格），前端使用 React、Vue.js、Angular.js 等前端框架替代 Django Template 将数据展示出来，这样做的好处是：

1. 后端只需要提供一套 API 就可以为 Web App、Android、IOS 等前端提供数据。
2. 前后端是不同的项目，前后端团队各司其职，只需要做好自己的事情。

基于 API 开发架构图：
![API architecture](http://cdn.defcoding.com/E3E045F2-5FF7-4E2F-95D1-89BB9A026179.png)

这里介绍 Django Template 的一些基础知识，一方面方便读者阅读项目中 Template 的代码，另一方面让读者可以体会一下 Django 组件中的 DRY 原则（don't repeat yourself，不要重复自己）。

## 博客项目 Django Template 设计
打开项目目录 `djblog/template`，目录结构如下：
```bash
├── article
│   ├── detail.html  # 博客详情页模板
│   └── index.html   # 博客主页模板
└── base.html        # 基础模板，包含其他模板的通用部分
```

### base.html 代码设计
```html
{% load static %}
<!DOCTYPE HTML>
<html>
  <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <title>Article Template</title>
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <meta name="description" content="" />
    <meta name="keywords" content="" />
    <meta name="author" content="" />

    <!-- HTML 头部通用配置和通用 CSS 文件 -->
    <meta property="og:title" content=""/>
    ...
    <link href="https://fonts.loli.net/css?family=Grand+Hotel" rel="stylesheet">
    ...
    <!-- Animate.css -->
    <link rel="stylesheet" href="{% get_static_prefix %}css/animate.css">
    ...
  </head>
  <body>
    <div class="colorlib-loader"></div>

    <!-- 博客导航通用 HTML 代码 -->
    <div id="page">
      <nav class="colorlib-nav" role="navigation">
        <div class="top-menu">
          <div class="container-fluid">
            <div class="row">
              <div class="col-xs-2">
                <div id="colorlib-logo"><a href="index.html">Article.</a></div>
              </div>
              <div class="col-xs-10 text-right menu-1">
              ...
              </div>
            </div>
          </div>
        </div>
      </nav>

      <!-- 博客首页轮播图 -->
      {% block slideshow %}
      ...
      {% endblock %}

      <!-- 自定义内容部分 -->
      {% block content %}
      {% endblock %}

      <!-- 通用页脚 HTML 代码 -->
      <footer id="colorlib-footer" role="contentinfo">
        <div class="container">
        ...
        </div>
      </footer>
    </div>

    <!-- 回到顶部按钮 -->
    <div class="gototop js-top">
      <a href="#" class="js-gotop"><i class="icon-arrow-up2"></i></a>
    </div>

    <!-- 通用 js 文件 -->
    <script src="{% get_static_prefix %}js/jquery.min.js"></script>
    ...
  </body>
</html>
```

`base.html` 是基础模板，包含其他模板需要的通用部分代码代码，如：css 和 js 文件、页面头部的导航栏，页面底部的页脚，其他的模板通过 `extend` 关键字继承 `base.html`，通过重写 `base.html` 中 `{% block content %}{% endblock %}` 实现自定义内容，设计 Django 模板结构需要充分考虑好通用的部分和非通用的部分。

### index.html 博客首页代码设计
```html
<!-- 继承 base.html -->
{% extends "base.html" %}
{% load static %}
{% load el_pagination_tags %}

<!-- 重写 block content 实现自定义内容 -->
{% block content %}
<div id="colorlib-container">
  <div class="container">
    <div class="row">
    <div class="content">
        <!-- 文章列表展示代码 -->
        {% paginate 10 article_list %}
        {% for article in article_list %}
        <article class="blog-entry">
          <div class="blog-wrap">
            <span class="category text-center">
              <a href="{% url 'index' %}?category={{ article.category_id }}">{{ article.category.name }}</a>
            </span>
            <h2 class="text-center"><a href="{% url 'detail' article.id %}">{{ article.title }}</a></h2>
            {% if article.images %}
            <div class="blog-image">
              {% for image in article.images %}
              <div class="owl-carousel owl-carousel2 blog-item">
                <div class="item">
                  <a href="#" class="blog image-popup-link"
                     style="background-image: url({% get_static_prefix %}images/blog-1.jpg);">
                  </a>
                </div>
                <div class="item">
                  <a href="#" class="blog image-popup-link"
                     style="background-image: url({% get_static_prefix %}images/blog-2.jpg);">
                  </a>
                </div>
              </div>
              {% endfor %}
            </div>
            {% endif %}
            <span class="category text-center">
              <a href="#"><i class="icon-calendar3"></i>{{ article.created_at|date:'Y-m-d H:i' }}</a>
              | <a href="#" class="posted-by"><i class="icon-user2"></i> {{ article.author.username }}</a>
              | <a href="#"><i class="icon-bubble3"></i>{{ article.comment_set.count }}</a></span>
          </div>
          <div class="desc">
            {{ article.content }}
          </div>
          <p class="text-center"><a href="{% url 'detail' article.id %}" class="btn btn-primary btn-custom">Continue Reading</a>
          </p>
        </article>
        {% empty %}
        {% endfor %}

        <!-- 分页代码 -->
        {# paginator #}
        <div class="row">
          <div class="col-md-12 text-center">


            <ul class="pagination">
              {% show_pages %}
            </ul>
          </div>
        </div>
      </div>

      <!-- 首页侧边栏 -->
      <aside class="sidebar">
        <div class="side">
          <div class="form-group">
            <input type="text" class="form-control" id="search" placeholder="Enter any key to search...">
            <button type="submit" class="btn btn-primary"><i class="icon-search3"></i></button>
          </div>
        </div>
        ...
        </div>
      </aside>
    </div>
  </div>
</div>
{% endblock %}
```
`index.html` 中包含文章列表展示的代码 `{% for article in article_list%}...{% endfor%}` 这是 Django 模板提供的语法，具体参考：[Django Template](https://docs.djangoproject.com/zh-hans/2.2/ref/templates/language/)

## 总结
1. 这一章我们学习了 Django Template 的基础知识，当今的开发流程中也许会很少用到它，一些老项目可能会用到它，另外 template 中的继承机制是一定要学会使用的。
2. DRY 原则不仅仅适用在 Django 模板设计上，当我们写代码发现很多地方是重复的时候就应该停下来思考一下是不是该将重复的地方抽象封装成函数、类等来减少代码重复，重复往往导致后续维护更加困难，想想你改动代码的时候，所有重复的地方都需要改一遍场景。

Django Template 相关问题通过 [Issue Django Template](https://github.com/runforever/djblog/issues/2) 来讨论。
