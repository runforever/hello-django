# Hello Django

## 如何开始
开发环境推荐使用 Linux 和 Mac OS，本书的项目是基于 Mac OS 下开发的，Windows 的读者建议安装一个 Ubuntu 虚拟机来做开发，环境依赖：

1. Python 3.7.2
2. Django 2.2
3. SQlite 3.27.2

## 基础环境准备
搭建开发环境：

1. 必须安装 [Git](https://github.com/git/git)，最好使用命令行操作 Git，图形化工具推荐使用 [Source Tree](https://www.sourcetreeapp.com/)
2. 注册 [GitHub](https://github.com/) 账号
3. 推荐用 [pyenv](https://github.com/pyenv/pyenv) 安装 Python 3.7.2
4. 推荐用 [virtualenv](https://github.com/pypa/virtualenv) 管理 Python 项目依赖

关于以上工具有安装和使用问题可以到这个 [Issue](https://github.com/runforever/djblog/issues/2) 里讨论。

## 运行项目
1. 项目地址 [https://github.com/runforever/djblog](https://github.com/runforever/djblog)，点击右上角的 Fork 按钮将项目 Fork 到自己的 GitHub
2. 将 Fork 后项目 Clone 到本地，`git clone fork后仓库地址`
3. 进入项目 `cd djblog`
4. 使用 pyenv 将项目的 Python 版本切换为 3.7.2，`python local 3.7.2 `
5. 使用 virtualenv 初始化环境 `virtualenv .venv`
6. 安装依赖 `source .venv/bin/activate && pip install requirements.txt`
7. 迁移数据库 `python manage.py migrate`
8. 运行项目 `python manage.py runserver`

打开浏览器访问 [http://127.0.0.1:8000](http://127.0.0.1:8000)，出现下图则表示安装成功。

![Hello Django](http://cdn.defcoding.com/E4DB73AF-5F05-46EF-A9FE-67B8CC574F3B.png)

## 项目目录结构

项目初始化相关问题 [Issue](https://github.com/runforever/djblog/issues/1) 里讨论。
