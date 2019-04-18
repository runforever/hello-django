# Django 中的 MTV

## MTV 是什么
从字面上的解释：

1. M 代表 Model，即 Django 的模型组件。
2. T 代表 Template，即 Django 的模板组件。
3. V 代表 View，即 Django 的视图组件。

从 Django 的 [设计理念](https://docs.djangoproject.com/zh-hans/2.2/misc/design-philosophies/) 中提到了松耦合，即上面的 Model、Template、View 三个组件各司其职，每个组件只做好自己该做的事，组件之间不关心对方的实现细节。

下面讲讲 Web 开发的问题模型，理解了这些模型，大家可以更全面的思考 Web 开发中遇到的问题。

## Web 开发 Request 和 Response
Web 开发处理的问题可以抽象为接收 Request 和返回 Response，如下图：
![web request&response](http://cdn.defcoding.com/D43F37A1-BB69-41BB-8129-195CA16AD425.png)

## Django 处理 Request 和 Response
Django 接收到 Request 后会经过 URLconf -> View -> Model，然后返回 Template Response，如下图：
![django request&response](http://cdn.defcoding.com/8D1E6A6B-E18D-49E9-BFBC-A80CF5018B5C.png)

## 博客项目的 Request 和 Response
![blog request&response](http://cdn.defcoding.com/76BFC046-8E3B-46E8-B8F0-0C31376A790F.png)

## 编写 MTV 代码
我们需要通过编写 MTV 来实现一个博客的功能，下面我们做一下博客项目的需求分析。

博客最重要的功能就是写博客，展示博客，所以目前要实现的功能有：
1. 定义文章的数据库模型。
2. 使用 Django Admin 编辑文章。
3. 展示文章列表和文章详情。
