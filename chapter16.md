# 尾声

俗话说师傅领进门，修行靠个人，能走多远完全是看自己，到这里教程就结束了，有其他想了解的内容欢迎给我反馈。

前面章节提到了基于 API 风格的架构，Django 有 [Django REST framework](https://www.django-rest-framework.org/) 可以实现 RESTful 风格的 API，感兴趣的读者可以查看一下，希望有相关教程的读者请给我反馈。

如果觉得这份教程做的不错，请到 [hello-django](https://github.com/runforever/hello-django) 和 [djblog](https://github.com/runforever/djblog) 项目给我 star，谢谢。

项目的完整代码位于 Git 分支 `final-version`

## 更多挑战
下面准备了一些新需求，感兴趣的读者可以为自己的博客添加一下功能：

1. Django Admin 添加 Markdown 支持，使用 Markdown 写文章。
2. Django Admin 添加富文本编辑器支持，可以使用百度的富文本编辑器。
3. 文章作者添加 nickname 字段，使用 OneToOneField 扩展 Django User。
4. 博客个人信息介绍支持通过 Django Admin 修改。

挑战完成后，需要改进意见的可以提交 pull request 给我进行 code review。
