# 初识 Django

## 为什么用 Django
Python 的 Web 开发是一个百花齐放的世界，隔壁的 Ruby 基本只用 Rails 框架，Python 的 Web 开发框架我用过的都好几个 Django、Flask、Webpy、Bottle、Tornado...
造成这样的原因是 Python Web 开发是构建在 WSGI 协议上的，所以任何人只要参照 WSGI 协议都可以写一个属于自己的 Web 开发框架，Python 的各个 Web 开发框架都有适合的场景，
我个人认为用 Django 作为 Python Web 开发入门的好处是：
1. Django 是 Full Stack 式的开发框架，包含了 Python Web 开发所需要的各种组件，初学者可以一次了解完。
2. Django 的项目结构和配置对于初学者是很好的项目模板。
3. Django 用的人多，很多开发问题都可以从 Google 找到答案。

个人经验是学习完 Django 后相当于将 Web 开发常用的组件都学了，再学习其他的 Web 开发框架你也会自然而然联想到该用用哪些组件解决问题。

## 为什么要写一个博客项目
在我学习 Django 的时候看到过一句话，博客项目就相当于 Web 开发的 Hello World，开发博客会用到 Web 开发的常用组件和工具，博客项目可以看做是 Web 开发的地基。

## 如何开始
开发环境推荐使用 Linux 和 Mac OS，本书的项目是基于 Mac OS 下开发的，Windows 的读者建议安装一个 Ubuntu 虚拟机来做开发，环境依赖：
1. Python 3.7.2
2. Django 2.2
3. SQlite 3.27.2

下一章节我们正式开始。
