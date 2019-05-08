# 使用 Nginx Gunicorn Supervisor 部署 Django

目前，博客的 MVP（Minimum Viable Product）最小可行性产品已经完成，写文章、展示文章的功能完成就可以上线进行内测了，其他的功能和使用过程中发现的问题可以通过后续的发布来完成，敏捷开发流程也倡导通过不断迭代来改进产品，好处是：

1. 可以更早的使用到产品。
2. 可以更早的发现问题。
3. 可以更快速的响应问题和解决问题。

这一章的内容讲的是部署需要的工具，没有服务器或者暂不想部署的同学可以直接跳下一章节。

## 准备工作
在章节 [需求分析 & 架构设计](chapter3.md) 中，我们提到过总体架构：
![djblog components](http://cdn.defcoding.com/992D079C-8A59-40F1-9B6F-4B519A90C730.png)

根据上面的图，我们需要准备的东西如下：

1. 服务器一台，阿里云或者腾讯云购买，操作系统使用 ubuntu 或其他熟悉的 Linux 发行版。
2. 域名一个，阿里云或者腾讯云购买。
3. 域名备案，阿里云或者腾讯云备案。
4. 服务器上安装 Nginx Web 服务器用作反向代理。
5. 服务器上安装 pipenv。
6. 服务器上安装 Gunicorn 网关组件。
7. 服务器上安装 SQLite3 数据库。
8. 服务器上安装 Git。
9. 服务器上安装 Supervisor 监控 Django 服务进程。
10. 本地使用 Fabric 将发布过程自动化。

## 服务器一些注意事项
1. 不要使用 root 用户部署，创建名为 deploy 用户用于部署：`useradd -d /home/deploy -m deploy`。
2. 使用 ssh 公钥认证登录服务器，关闭 ssh 密码登录。
3. 新建 `/data/deploy_app/` 目录用于存放部署 app。
4. 新建 `/data/djblog/` 目录用户存放部署 app 的静态文件。

## 基础环境准备

安装过程：

1. Google 搜索安装最新版 Nginx 并安装，教程：[install nginx](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/)
2. Google 搜索安装最新版 Git，教程：[install git stackoverflow](https://stackoverflow.com/a/19109661/4265779)
3. 进入 `/data/deploy_app` 目录将项目 Clone 下来，`git clone your_repo_url`。
4. Google 搜索安装 SQlite 3.27.2，教程：[install sqllite3](https://linuxhint.com/install-sqlite-ubuntu-linux-mint/)
5. 根据 [pipenv](https://github.com/pypa/pipenv) 文档安装：`apt install pipenv`
6. 进入项目安装项目依赖：`cd djblog; pipenv install --skip-lock`
7. 使用 migrate 初始化数据库表：`python manage.py mirate`
8. 使用 collectstatic 收集项目静态文件：`python manage.py collectstatic`
9. 安装 supervisord：`apt-get install supervisord`
10. 将项目的 supervisor 配置通过软连接到系统的 supervisor 配置：`ln -s /data/deploy_app/djblog/deploy/supervisor.conf /etc/supervisor/conf.d/djblog.conf`
11. 使用 supervisor 启动博客服务：`supervisorctl reload && supervisorctl start djblog`
12. 查看 djblog 运行情况：`supervisorctl status`
13. 将项目 nginx 配置软连接到系统 Nginx 配置：`ln -s /data/deploy_app/djblog/deploy/djblog.conf /etc/nginx/site-enabled/djblog.conf`
14. 使用 root 用户重载 Nginx：`nginx -t && nginx -s reload`
15. 访问测试

supervisor.conf 配置文件:
```ini
[program:djblog]
command=pipenv run gunicorn -b 127.0.0.1:3901 djblog.wsgi
directory=/data/deploy_app/djblog
user=deploy
autostart=true
autorestart=true
redirect_stderr=true
```

djblog.conf nginx 配置文件：
```shell
server {
    listen 80;
    # 替换成你的域名
    server_name yourdomin;

    location / {
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header Host $http_host;
        proxy_redirect off;
        proxy_pass    http://127.0.0.1:3901;
    }

    location ^~ /static {
        alias /data/djblog/static;
    }

    location ^~ /media {
        alias /data/djblog/media;
    }
}
```

## 持续发布
我们开发新功能后发布版本的步骤如下：

1. 登录服务器。
2. 进入到项目目录使用 Git 更新代码。
3. 安装新的依赖。
4. 重启 Gunicorn 进程。

每发一次版本都需要重复上述步骤，偷懒是美德，我们使用 Fabric 将上述步骤自动化，每次发布只需要本地执行 `fab deploy` 即可。

fabric 任务代码：
```python
from fabric.api import (
    env,
    task,
    run,
    cd,
    hosts,
)

env.use_ssh_config = True

# 填写你的服务器 IP
PROD_ENV = 'your server ip'
PROD_USER = 'deploy'

env.user = PROD_USER


@task
@hosts(PROD_ENV)
def deploy():
    with cd('/data/deploy_app/djblog'):
        run('git checkout -- .')
        run('git pull --rebase')
        run('pipenv run python manage.py migrate')
        run('pipenv run python manage.py collectstatic --noinput')
        run('supervisorctl restart djblog')
```

## 总结
编程是个遇到问题解决问题的过程，而 Google 上就有很多现成的解决方法，关键是看你搜索的关键词能不能从 Google 上找到答案。

+ 部署问题通过 [Issue Django 部署](https://github.com/runforever/djblog/issues/10) 讨论。
