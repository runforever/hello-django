# 文章评论列表展示

接下来的两章我们要完成文章详情页的评论功能。

## 数据库表设计
评论的字段如下：

1. 评论人昵称
2. 评论人邮箱
3. 评论内容
4. 所属文章，文章与评论是一对多关系。

## Django Admin 中管理评论
参照 [使用 Django Admin 管理博客文章](chapter4.md) 中的教程添加。

## 使用 Django View 将标签显示出来
BlogDetailView 中添加使用 Django ORM 获取该文章所有评论的代码。
