# 文章评论列表展示

目前博客只剩下文章评论功能没有做了，这一章我们先完成评论列表展示功能，编码工作由读者完成，教程只提供思路。

文章详情页需要显示网友评论：
![article comments](http://cdn.defcoding.com/3A359525-F08A-4663-963E-A351811BD522.png)

## 数据库表设计
评论的字段如下：

1. 评论人昵称（nickname）
2. 评论人邮箱（email）
3. 评论内容（content）
4. 所属文章，文章与评论是一对多关系（article）
5. 评论创建时间（created_at）
6. 评论更新时间（updated_at）

## Django Admin 中管理评论
参照 [使用 Django Admin 管理博客文章](chapter4.md) 中的教程添加，完成后添加几条评论做测试

## 使用 Django View 将标签显示出来
`BlogDetailView` 中添加获取该文章所有评论代码，模板变量名是 `comment_list`。

测试完成后，需要改进意见可以提交 pull request 给我 code review。

+ 代码位于 Git 分支 `feature-article-comment`
+ 评论展示相关问题通过 [Issue 评论展示](https://github.com/runforever/djblog/issues/12) 讨论
