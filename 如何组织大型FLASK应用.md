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

### 准备操作系统

为了有一个稳定的服务器，我们必须有所有相关工具和库的最新和良好维护。

为了确保我们拥有默认应用程序的最新可用版本，让我们从更新开始。

在基于 Debian 的系统（即 Ubuntu、Debian）上运行以下命令：

```bash
aptitude    update
aptitude -y upgrade
```

要获得必要的开发工具，请使用以下命令安装"build-essential"：

```bash
aptitude install -y build-essential python-dev python2.7-dev
```

### 设置Python, pip and virtualenv

在 Ubuntu 和 Debian 上，默认有一个最新版本的 Python 解释器（您可以使用）。只剩有限几个额外软件包需要我们去安装：

- python-dev（开发工具）
- pip（用于管理包）
- virtualenv（用于创建独立的虚拟环境）

**注意：** 此处给出的说明保持简短。要了解更多信息，请查看我们关于 pip 和 virtualenv 的"how-to"一文：[Using virtualenv, Installing with Pip, and Managing Packages.](https://www.digitalocean.com/community/tutorials/common-python-tools-using-virtualenv-installing-with-pip-and-managing-packages)

#### pip

pip 是一个包管理器，它将帮助我们安装我们需要的应用程序包。

运行以下命令以安装 pip：

```bash
curl https://bitbucket.org/pypa/setuptools/raw/bootstrap/ez_setup.py | python -
curl https://raw.github.com/pypa/pip/master/contrib/get-pip.py | python -
export PATH="/usr/local/bin:$PATH"
```

#### virtualenv

最好将 Python 应用程序及其所有依赖项一起包含在其自己的环境中。环境可以被理解为（简单地说）一个存有所有东西的独立位置（一个文件夹）。为此，使用名为 virtualenv 的工具。

运行以下操作以使用 pip 安装 virtualenv：

```bash
sudo pip install virtualenv
```

## 构建应用程序目录

我们将使用LargeApp作为我们的示例应用程序文件夹的名称。其中，我们将在应用程序包（即app）和其他一些文件（例如用于运行测试或开发服务器的“run.py”和用于保存Flask配置的“config.py”）旁边有一个虚拟环境（即env）。

该结构（如下所示）是高度可扩展的，并且它是被构建来利用Flask和其他库提供的所有有用工具的。当你看到它的时候别害怕，因为我们将通过构建所有内容来逐步解释所有内容。

目标示例结构：

```bash
~/LargeApp
    |-- run.py
    |-- config.py
    |__ /env             # Virtual Environment
    |__ /app             # Our Application Module
         |-- __init__.py
         |-- /module_one
             |-- __init__.py
             |-- controllers.py
             |-- models.py
         |__ /templates
             |__ /module_one
                 |-- hello.html
         |__ /static
         |__ ..
         |__ .
    |__ ..
    |__ .
```

### 创建应用程序文件夹

让我们从创建主文件夹开始。

依次运行以下命令：

```bash
mkdir ~/LargeApp
mkdir ~/LargeApp/app
mkdir ~/LargeApp/app/templates
mkdir ~/LargeApp/app/static
```

我们目前的结构：

```bash
~/LargeApp
    |__ /app             # Our Application Module
         |__ /templates
         |__ /static
```

### 创建一个虚拟环境

使用虚拟环境能带来大量好处。强烈建议你为每个应用程序使用心的虚拟环境。将virtualenv文件夹保留在应用程序内部是一种使事情井然有序的好方法。

运行以下命令以创建安装了pip的新虚拟环境。

```bash
cd         ~/LargeApp
virtualenv env
```

### 创建应用程序文件

### 安装Flask和应用程序依赖项

## 使用模块和Blueprints（组建）

## 创建应用程序（run.py、init.py等）

## 创建模块/组件
