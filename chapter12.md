# 部署上线

目前为止，博客的 MVP（Minimum Viable Product）最小可行性产品已经完成，写文章、展示文章的功能完成就可以发布使用了，其他的功能和使用过程中发现的问题可以通过后续的发布来完成，敏捷开发流程也倡导通过不断迭代来改进产品，好处是：

1. 可以更早的使用到产品。
2. 可以更早的发现问题。
3. 可以更快速的响应问题和解决问题。

这一章的内容讲的是部署需要的工具，没有服务器或者暂不想部署的同学可以直接跳下一章节。

## 架构 & 准备工作
在章节 [需求分析 & 架构设计](chapter3.md) 中，我们提到过总体架构：
![djblog components](http://cdn.defcoding.com/992D079C-8A59-40F1-9B6F-4B519A90C730.png)

根据上面的图，我们需要准备的东西如下：

1. 服务器一台，可以使用阿里云或者腾讯云等云服务器，操作系统使用 ubuntu 或其他读者熟悉的 Linux 发行版。
2. 域名一个，可以到阿里云或者腾讯云购买。
3. 域名备案，阿里云和腾讯云都可以。
4. 服务器上安装 Nginx Web 服务器用作反向代理。
5. 服务器上安装 pyenv 和 virtualenv。
6. 服务器上安装 Gunicorn 网关组件。
7. 服务器上安装 SQLite 数据库。
8. 服务器上安装 Git。
9. 服务器上安装 Supervisor 监控 Django 服务进程。
10. 本地使用 Fabric 将发布过程自动化。

## 服务器一些注意事项
1. 不要使用 root 用户部署，创建名为 deploy 用户用于部署。
2. 使用 ssh 公钥认证登录服务器，关闭 ssh 密码登录。
2. 新建 `/data/deploy_app/` 目录用于存放部署 app。
3. 新建 `/data/var/` 目录用户存放部署 app 的静态文件。

## 基础环境准备
这一阶段使用 Google 的技能尤为重要，因为每个人环境不一样，安装过程中遇到的问题也可能各种各样的，遇到奇怪的问题就求助 Google 吧。

安装过程：

1. Google 搜索安装最新版 Nginx 并安装，教程：[install nginx](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/)
2. Google 搜索安装最新版 Git，教程：[install git stackoverflow](https://stackoverflow.com/a/19109661/4265779)
3. 进入 `/data/deploy_app` 目录将项目 Clone 下来，`git clone your_repo_url`。
4. Google 搜索安装 SQlite 3.27.2，教程：[install sqllite3](https://linuxhint.com/install-sqlite-ubuntu-linux-mint/)
5. 根据 [pyenv](https://github.com/pyenv/pyenv-installer) 文档，使用 deploy 账户安装 pyenv 和 Python 3.7.2。
6. 进入项目设置 Python 版本：`cd djblog; pyenv local 3.7.2`
7. 安装 virtualenv 并初始化：`pip install virtualenv && virtualenv .venv`
8. 安装项目依赖：`source .venv/bin/activate && pip install requirements.txt`
9. 使用 migrate 初始化数据库表：`python manage.py mirate`
10. 安装 supervisord：`apt-get install supervisord`
11. 使用 supervisord 启动 gunicorn：。
12. 配置 Nginx 反向代理。
13. 访问测试。

## 持续发布
我们开发新功能后发布版本的步骤如下：

1. 登录服务器。
2. 进入到项目目录使用 Git 更新代码。
3. 安装新的依赖。
4. 重启 Gunicorn 进程。

每发一次版本都需要重复上述步骤，偷懒是美德，我们使用 Fabric 将上述步骤自动化，每次发布只需要本地执行 fab deploy 即可。

fabric 任务代码：
```python
```

## 总结
1. Google 是程序员的好帮手，遇到问题要擅于使用 Google 自行解决。
2. 感兴趣的读者可以调研一下 CircleCI 的使用，后续可以改进为使用 CI 发布版本。
