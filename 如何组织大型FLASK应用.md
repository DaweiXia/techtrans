# 如何构建大型FLASK应用

作者：O.S. Tezer
发表时间：2014年1月16日
原文链接: [How To Structure Large Flask Applications](https://www.digitalocean.com/community/tutorials/how-to-structure-large-flask-applications)

## 引言

有许多构建Python Web应用的方法和惯例。尽管某些框架附带了工具（充当脚手架），以自动化和简化任务（和令人头疼的问题），但几乎所有解决方案都依赖于打包/模块化应用程序，因为代码库在相关文件和文件夹中有逻辑地分布。

极简主义的WEB应用程序开发框架Flask有自己的工具——blueprints。

在本 DigitalOcean 文章中，我们将了解如何创建应用程序目录，并将其构建为使用 Flask blueprints创建的可重用组件。这些部分极大地允许（并简化了）应用程序组件的维护和开发。

## 章节标题

1. Flask: 极简主义的应用程序开发框架
2. 本文中的选择
3. 为Flask准备系统
    1. 准备操作系统
    2. 设置Python、pip、virtualenv
4. 构建应用程序目录
    1. 创建应用程序文件夹
    2. 创建虚拟环境
    3. 创建应用程序文件
    4. 安装Flask
5. 使用模块和Blueprints（组建）
    1. 模块基础知识
    2. 模块模板
6. 创建应用程序（run.py、init.py等）
    1. 使用 nano 编辑run.py
    2. 使用nano编辑config.py
7. 创建模块/组件
    1. 第1步：构建模块
    2. 第2步：定义模块数据模型
    3. 第3步：定义模块表单
    4. 第4步：定义应用程序控制器（视图）
    5. 第5步：在"应用/init.py"中设置应用程序
    6. 第6步：创建模板
    7. 第7步：在操作中查看你的模块

## Flask: 极简主义的应用程序开发框架

Flask是一个极简主义（或微型的）框架，它避免强加处理关键事物的方式。相反，Flask允许开发人员使用他们期望且熟悉的工具。为此，它附带了自己的扩展索引，并且已经存在大量工具来处理从登录到日志记录的所有内容。

它不是一个严格的"常规"框架，部分依赖于配置文件，坦率地说，当涉及到开始和保持事情在控制中，这使得很多事情更容易。

## 本文中的选择

正如我们刚刚在上一节中介绍的那样，Flask 的做事方式包括使用你感到最舒服的工具。在我们的文章中，我们将使用扩展和库（即数据库提取层）方面最常见的（也是明智的）选择：

- SQLAlchemy (通过Flask-SQLAlchemy)
- WTForms (通过Flask-WTF)

### Flask-SQLAlchemy

向 Flask 添加 SQLAlchemy 支持。又快又容易。

这是被认可的扩展。

```bash
作者: Armin Ronacher
PyPI页面: Flask-SQLAlchemy
文档: 阅读packages.python.org上的文档
On Github: [mitsuhiko/flask-sqlalchemy](https://github.com/mitsuhiko/flask-sqlalchemy)
```

### Flask-WTF

Flask-WTF 提供与 WTForms 的简单集成。此集成包括可选的 CSRF 处理，以提高安全性。

这是被认可的扩展。

```bash
作者:  Anthony Ford (created by Dan Jacob)
PyPI页面: Flask-WTF
文档: 阅读packages.python.org上的文档
On Github: [ajford/flask-wtf](https://github.com/mitsuhiko/flask-wtf)
```

## 为Flask准备系统

在我们开始构建大型 Flask 应用程序之前，让我们准备我们的系统并下载（和安装）Flask 。

**注意：** 我们将在运行了最新版可用操作系统（即 Ubuntu 13）的新近Droplet（一款DigitalOcean云主机产品）实例上工作。强烈建议你在新系统上测试所有内容，尤其是如果你积极为客户服务。


## 构建应用程序目录

## 使用模块和Blueprints（组建）

## 创建应用程序（run.py、init.py等）

## 创建模块/组件
