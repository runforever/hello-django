# 初识 Django

## Django 由来
来自 [Django FAQ](https://docs.djangoproject.com/zh-hans/2.2/faq/general/)，Django 从一个非常实际的需求成长而来：World Web 是一家新闻网站，负责在新闻截止期限内建立密集的 Web 应用程序。 在快节奏的新闻编辑室，World Online 往往需要几个小时内将一个复杂的 Web 应用程序从概念推向发布上线。

与此同时，World Online 的 Web开发者在 Web 开发的最佳实践方面一直是完美主义者。

在2003年秋季，世界在线开发者（Adrian Holovaty 和 Simon Willison）放弃 PHP 而开始使用 Python 开发网站。 当他们构建了密集的，强互动性的网站时（如 Lawrence.com），他们开始提取一个通用的 Web 开发框架，使他们可以更快地构建 Web 应用程序。 两年内他们经常调整和改进这个框架。

## 使用 Django 的原因
Python 的 Web 开发是一个百花齐放的世界，隔壁的 Ruby 基本只用 Rails 框架，Python 的 Web 开发框架常用的有 Django、Flask、Webpy、Bottle、Tornado...
造成这样的原因是 Python Web 开发是构建在 WSGI 协议（Python 通用网关协议），任何人只要参照 WSGI 协议都可以开发出自己的 Web 开发框架，现有的 Python Web 开发框架都有自己适合的场景，
我个人认为用 Django 作为 Python Web 开发入门的好处是：

1. Django 是 Full Stack 式的开发框架，包含了 Python Web 开发所需要的各种组件，初学者可以一次了解完，学习其他框架能达到举一反三的效果。
2. Django 的项目结构和配置对于初学者是很好的项目结构模板。
3. Django 用的人多，很多问题都可以从 Google 找到答案。

## Django 组件概览
来自 [Django 文档](https://docs.djangoproject.com/zh-hans/2.2/)：

1. Model 模型层：Django 提供了一个抽象的模型 ("models") 层，为了构建和操纵你的Web应用的数据。
2. View 视图层：Django 具有 “视图” 的概念，负责处理用户的请求并返回响应。
3. Template 模板层：模板层提供了一个对设计者友好的语法用于渲染向用户呈现的信息。
4. Form 表单：Django 提供了一个丰富的框架来帮助创建表单和处理表单数据。
5. Admin 管理：通过简单的代码配置即可用 Django Admin 组件生成数据管理后台。
