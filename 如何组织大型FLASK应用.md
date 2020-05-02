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

这一步，我们将在继续使用模块和蓝图（blueprints）之前形成基本的应用程序文件。

运行以下命令以创建基本的应用程序文件：

```bash
touch ~/LargeApp/run.py
touch ~/LargeApp/config.py
touch ~/LargeApp/app/__init__.py
```

我们目前的结构：

```bash
~/LargeApp
    |-- run.py
    |-- config.py
    |__ /env             # Virtual Environment
    |__ /app             # Our Application Module
         |-- __init__.py
         |__ /templates
         |__ /static
```

### 安装Flask和应用程序依赖项

一切准备就绪后，要开始使用Flask进行开发，请使用pip下载并安装它。

运行以下命令在虚拟环境env中安装Flask。

```bash
cd ~/LargeApp
env/bin/pip install flask
env/bin/pip install flask-sqlalchemy
env/bin/pip install flask-wtf
```

注意：在这里，我们在未激活虚拟环境的情况下下载和安装Flask。但是，鉴于我们使用的是虚拟环境本身的pip，它可以完成相同的任务。如果你使用的是激活的环境，则可以使用pip代替。

就是这样！ 现在，我们准备使用蓝图（blueprints）构建模块化的更大的Flask应用程序。

## 使用模块和Blueprints（组件）

### 模块基础

至此，我们已经建立了应用程序结构，并已下载和准备好了其依赖项。

我们的目标是模块化（即使用Flask blueprints创建可重复使用的组件）所有可以逻辑分组的相关模块。

认证系统就是一个例子。将所有视图（views），控制器（controllers），模型（models）和帮助器（helpers）放在一个地方，并以允许可重用性的方式进行设置，从而使这种结构成为维护应用程序并提高生产率的好方法。

目标示例模块（组件）结构（在/ app内部）：

```bash
# 我们在这里的模块示例称为* mod_auth *
# 只要遵循约定，你就可以随意命名

/mod_auth
    |-- __init__.py
    |-- controllers.py
    |-- models.py
    |-- ..
    |-- .
```

### 模块模板

为支持最大模块化，我们将按照上面的约定构造“templates”文件夹，并包含一个新文件夹（与模块具有相同或相似的相关名称）以包含其模板文件。

目标示例模板目录结构（在LargeApp内部）：

```bash
/templates
    |-- 404.html
    |__ /auth
         |-- signin.html
         |-- signup.html
         |-- forgot.html
         |-- ..
         |-- .
```

## 创建应用程序（run.py、init.py等）

在本节中，我们将继续前面的步骤，并从实际的应用程序编码开始，然后继续创建我们的第一个模块化组件（使用blueprints）：mod_auth，用于处理所有与身份验证有关的过程（即登录，注册等）。

### 使用nano编辑“run.py”

注意：nano是Linux系统中的一种文本编辑器。

```bash
nano ~/LargeApp/run.py
```

敲入内容：

```bash
# 运行测试服务器
from app import app
app.run(host='0.0.0.0', port=8080, debug=True)
```

使用CTRL + X保存并退出，然后使用Y确认。

### 使用nano编辑“config.py”

```bash
nano ~/LargeApp/config.py
```

敲入内容：

```bash
# 设置允许调试，此语句只用于开发环境
DEBUG = True

# 定义应用程序目录
import os
BASE_DIR = os.path.abspath(os.path.dirname(__file__))  

# 定义数据库 - 在此示例中我们使用SQLite
SQLALCHEMY_DATABASE_URI = 'sqlite:///' + os.path.join(BASE_DIR, 'app.db')
DATABASE_CONNECT_OPTIONS = {}

# 设置应用程序线程数。一个普遍通用假设是每个可用处理器内核
# 使用2个线程。一个用来处理传入请求，一个用来执行后台操作。
THREADS_PER_PAGE = 2

# 启用针对“跨站点请求伪造（CSRF）”的保护
CSRF_ENABLED     = True

# 使用安全，唯一且绝对保密的密钥对数据进行签名。
CSRF_SESSION_KEY = "secret"

# 签署Cookie的秘钥
SECRET_KEY = "secret"
```

使用CTRL + X保存并退出，然后使用Y确认。

## 创建模块/组件

本节是定义本文核心的第一步。在这里，我们将了解如何使用Flask的蓝图（blueprints）创建模块（即组件）。

这样做的高明之处在于，未来的你将不胜感激它所提供的易于维护的代码具有可移植性、可重用性，因为通常情况下，要回来了解剩下的东西是相当困难的。

### 步骤1：结构化模块

如我们所愿，让我们创建第一个模块的（mod_auth）目录和文件以开始对其进行操作。

```bash
# 在 *app* 模块中创建mod_auth模块
mkdir ~/LargeApp/app/mod_auth

# 创建模块模板存放的目录
mkdir ~/LargeApp/app/templates/auth

# 创建__init__.py以将目录设置为Python模块
touch ~/LargeApp/app/mod_auth/__init__.py

# 创建模块的控制器（视图）与数据模型等文件
touch ~/LargeApp/app/mod_auth/controllers.py
touch ~/LargeApp/app/mod_auth/models.py
touch ~/LargeApp/app/mod_auth/forms.py

# 创建模块的模板文件
touch ~/LargeApp/app/templates/auth/signin.html

# 创建 http 404错误页
touch ~/LargeApp/app/templates/404.html
```

完成这些操作后，文件夹结构应如下所示：

```bash
~/LargeApp
    |-- run.py
    |-- config.py
    |__ /env             # 虚拟环境
    |__ /app             # 我们的应用程序模块
         |-- __init__.py
         |-- /mod_auth   # 我们的第一个模块, mod_auth
             |-- __init__.py
             |-- controllers.py
             |-- models.py
             |-- forms.py
         |__ /templates
             |-- 404.html
             |__ /auth
                 |-- signin.html
         |__ /static
```

### 步骤2：定义模块数据模型

### 步骤3：定义模块表单

### 步骤4：定义应用程序控制器（视图）

### 步骤5：在“app/init.py”中设置应用程序

### 步骤6：创建模板

### 步骤7：使用模块
