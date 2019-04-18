# 需求分析 & 架构设计

## 项目需求分析
在项目开始之前我们要确定要做那些功能，对项目有个全局的认识，一个博客最的基础功能如下：

1. 文章管理模块，包含文章的新增、删除、编辑、查看、（增删改查）
2. 文章分类模块，包含分类的增删改查
3. 文章评论模块，包含评论的增删改查

增删改查的术语是 CURD（Create、Update、Read、Delete）

## 架构设计
架构设计要确定我们使用哪种技术和组件实现以上功能。

Web 开发框架毫无疑问是 Django，Django 使用 App 来组织功能模块，所以之后我们的项目目录结构会如下图所示：

![djblog apps](http://cdn.defcoding.com/D62ADCF7-2563-4923-9158-BB5BA0329536.png)

文章、分类、评论等数据的存取需要数据库，数据库我们选用 SQLite 这个简单关系型数据库，至于为什么不用 MySQL、PostgreSQL 这种数据库的原因是：

1. 我们的重点在如何使用 Django。
2. SQLite 完全胜任个人博客的读写性能。

当我们开发完成后，项目需要上线，这时候需要 Web 服务器做反向代理，我们选用 Nginx，为了让 Django 识别 Web 服务器的请求，我们需要 Python 的 WSGI 网关组件：uWSGI 或者 Gunicorn，我们选用 Gunicorn，所以总体的架构需要的组件可以用下图中的服务端所示：

![djblog components](http://cdn.defcoding.com/992D079C-8A59-40F1-9B6F-4B519A90C730.png)

之后的章节我们只需要关注 Django 部分。

## 总结
编码前对问题做需求分析和技术选项是很重要的环节，在外界看来程序员是码农，搬砖者，但其实我们是一个工程师，我们要学会独立思考，用合适的技术高效的解决问题，关于本章的问题欢迎在这个 [Issue](https://github.com/runforever/djblog/issues/3) 里讨论。
