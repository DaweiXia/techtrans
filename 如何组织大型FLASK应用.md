# 如何组织大型FLASK应用

作者：O.S. Tezer
发表时间：2014年1月16日
原文链接: [How To Structure Large Flask Applications](https://www.digitalocean.com/community/tutorials/how-to-structure-large-flask-applications)

## 引言

有许多组织Python Web应用的方法和惯例。尽管某些框架附带了工具（充当脚手架），以自动化和简化任务（和令人头疼的问题），但几乎所有解决方案都依赖于打包/模块化应用程序，因为代码库在相关文件和文件夹中有逻辑地分布。

极简主义的WEB应用程序开发框架Flask有自己的工具——blueprints。

在本 DigitalOcean 文章中，我们将了解如何创建应用程序目录，并将其组织为使用 Flask blueprints创建的可重用组件。这些部分极大地允许（并简化了）应用程序组件的维护和开发。

## 章节标题
