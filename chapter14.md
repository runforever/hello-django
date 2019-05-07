# 文章评论列表展示

目前博客只剩下文章评论功能没有做了，这一章我们先完成评论列表展示功能，编码工作由读者完成，教程只提供思路。

## 数据库表设计
评论的字段如下：

1. 评论人昵称（nickname）
2. 评论人邮箱（email）
3. 评论内容（content）
4. 所属文章，文章与评论是一对多关系（article）

## Django Admin 中管理评论
参照 [使用 Django Admin 管理博客文章](chapter4.md) 中的教程添加。

## 使用 Django View 将标签显示出来
BlogDetailView 中添加使用 Django ORM 获取该文章所有评论的代码，模板变量名是 `comment_list`。

本地测试完成后，如果需要改进意见可以提交 pull request 给我进行 code review。
