# 文章分类设计

目前流行的 Web App 开发架构是前后端分离的架构，后端开发提供数据 API（通常是 RESTful 架构），前端使用 React、Vue.js、Angular.js 等前端框架替代 Django Template 将数据展示出来，这样做的好处是：

1. 后端只需要提供一套 API 就可以为 Web App、Android、IOS 等前端提供数据。
2. 前后端是不同的项目，前后端团队各司其职，只需要做好自己的事情。

这里介绍 Django Template 的一些基础知识，一方面方便读者阅读项目中 Template 的代码，另一方面让读者可以体会一下 Django 组件的 DRY（do not repeat yourself，不要重复自己）原则。

## Django Template 结构设计
打开项目目录 `djblog/template`
```
├── article
│   ├── detail.html
│   └── index.html
└── base.html
```

`base.html` 是基础模板，css 和 js 等所有模板通用的代码都包含在内，其他的模板如：`index.html` 通过 `extend` 关键字继承 `base.html`，通过重新 `{% block content %}{% endblock %}` 来 `index.html` 中的自定义内容。

DRY 原则不仅仅适用在 Django 模板设计上，当我们写代码发现很多地方是重复的时候就应该停下来思考一下是不是该将重复的地方抽象封装成函数、类等来减少代码重复，重复意味着维护的困难，想想你需要改动代码的时候，所有重复的地方都需要改一遍。

## 总结
这一章我们学习了 Django Template 的基础知识，在现在的工作中也许会很少用到它，但是 template 中 DRY 的继承机制是学习 Django 的同学一定要理解的。

本章文通通过 [Issue](#) 来讨论。
