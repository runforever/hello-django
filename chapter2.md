# Hello Django

## Django 由来
来自 [Django FAQ](https://docs.djangoproject.com/zh-hans/2.2/faq/general/)，Django 从一个非常实际的需求成长而来：World Web 是一家新闻网站，负责在新闻截止期限内建立密集的 Web 应用程序。 在快节奏的新闻编辑室，World Online 往往需要几个小时内将一个复杂的 Web 应用程序从概念推向发布上线。

与此同时，World Online 的 Web开发者在 Web 开发的最佳实践方面一直是完美主义者。

在2003年秋季，世界在线开发者（Adrian Holovaty 和 Simon Willison）放弃 PHP 而开始使用 Python 开发网站。 当他们构建了密集的，强互动性的网站时（如 Lawrence.com），他们开始提取一个通用的 Web 开发框架，使他们可以更快地构建 Web 应用程序。 两年内他们经常调整和改进这个框架。

## 使用 Django 的原因
Python 的 Web 开发是一个百花齐放的世界，隔壁的 Ruby 基本只用 Rails 框架，Python 的 Web 开发框架常用的有 Django、Flask、Webpy、Bottle、Tornado...
造成这样的原因是 Python Web 开发是构建在 WSGI 协议（Python 通用网关协议），任何人只要参照 WSGI 协议都可以开发出自己的 Web 开发框架，每种开发框架都有自己适合的场景，
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

## 如何开始
开发环境推荐使用 Linux 或者 macOS，本项目代码基于 macOS 10.14.1 开发的，Windows 的读者建议安装一个 Ubuntu 虚拟机来做开发，环境依赖：

1. Python 3.7.2
2. Django 2.2
3. SQlite 3.27.2
4. 教程配套的博客项目，项目地址：[https://github.com/runforever/djblog](https://github.com/runforever/djblog)。

## 基础环境准备
搭建开发环境：

1. 安装 [Git](https://github.com/git/git)，最好使用命令行操作 Git，图形化工具推荐使用 [Source Tree](https://www.sourcetreeapp.com/)
2. 注册 [GitHub](https://github.com/) 账号
3. 使用 [pipenv](https://github.com/pypa/pipenv) 安装 Python 3.7.2 和管理 Python 项目依赖。

## 运行项目
1. 项目地址 [https://github.com/runforever/djblog](https://github.com/runforever/djblog)，点击右上角的 Fork 按钮将项目 Fork 到自己的 GitHub
2. 将 Fork 后项目 Clone 到本地，`git clone fork仓库地址`
3. 进入项目 `cd djblog`
4. 使用 pipenv 初始化项目，`pipenv install`
5. 使用 pipenv 进入项目虚拟环境，`pipenv shell`
6. 执行数据库变更 `python manage.py migrate`
7. 运行项目 `python manage.py runserver`

打开浏览器访问 [http://127.0.0.1:8000](http://127.0.0.1:8000)，出现下图则表示成功。

![Hello Django](http://cdn.defcoding.com/E4DB73AF-5F05-46EF-A9FE-67B8CC574F3B.png)

## 项目目录结构
```shell
.
├── LICENSE
├── Pipfile                 # pipenv 依赖文件
├── Pipfile.lock
├── README.md
├── deploy                  # 发布配置文件
│   ├── djblog.conf
│   └── supervisord.conf
├── djblog
│   ├── __init__.py
│   ├── app                 # 自定义 Django App 目录
│   │   ├── __init__.py
│   ├── settings            # Django Settings 配置文件
│   │   ├── __init__.py
│   │   └── settings.py
│   ├── static              # 博客模板静态资源
│   ├── templates           # 博客模板
│   │   ├── article
│   │   │   ├── detail.html
│   │   │   └── index.html
│   │   ├── base.html
│   │   └── el_pagination
│   ├── urls.py             # Django URFConf 配置文件
│   └── wsgi.py
├── fabfile.py              # Fabric 任务自动化文件
└── manage.py
```
项目已经配置好了博客的前端模板，读者后续只需要使用 Django 实现业务。

项目初始化相关问题 [Issue 项目初始化](https://github.com/runforever/djblog/issues/1) 里讨论。
