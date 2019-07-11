# 使用说明

## 1.创建文件夹
除非特别说明,假设你的工作目录将是在项目的根目录,即 watchlist/ 目录
```bash
mkdir watchlist
cd watchlist
```

## 2.下载安装python
下载地址：https://www.python.org/ftp/python/3.7.3/python-3.7.3-amd64.exe  

记得勾选上添加到环境变量里path  

检测安装是否成功：使用cmd命令行输入：python --version 或pip --version

## 3.下载安装git
下载地址：https://github.com/git-for-windows/git/releases/download/v2.21.0.windows.1/Git-2.21.0-64-bit.exe  

直接默认一直下一步就行  

检测安装是否成功：使用cmd命令行输入：git --version

## 4.下载安装命令行扩展工具cmder
cmder 集成了 Git Bash,支持一些在 Linux 或 macOS 下才能使用的命令（程序）,比如 ls、cat、nano、ssh 等  

下载地址：https://github.com/cmderdev/cmder/releases/download/v1.3.11/cmder.zip  

解压缩后,把cmder.exe所在路径添加到环境变量里,通过在命令行里输入：cmder可以调用出来  

若无特殊说明,以下的命令行操作均是在cmder中完成的,当前目录是watchlist

## 5.配置git全局信息
```bash
git config --global user.name "sandu" # 替换成你的名字
git config --global user.email "1103324414@qq.com" # 替换成你的邮箱地址
```

## 6.创建目录并使用git初始化
创建一个 Git 仓库,会在项目根目录创建一个 .git 文件夹
```bash
git init
```

## 7.创建.gitignore文件
在项目根目录创建一个 .gitignore 文件,在文件中写入忽略文件的规则
```bash
cat .gitignore
*.pyc 
*~ 
__pycache__ 
.DS_Store
.env
*.db
```

## 8.生成密钥文件
先使用下面的命令检查是否已经创建了 SSH 密钥
```bash
cat ~/.ssh/id_rsa.pub
```

如果显示"No such file or directory",就使用下面的命令生成 SSH 密钥对,否则复制输出的值  

```bash
ssh-keygen
```
一路按下 Enter 采用默认值,最后会在用户根目录创建一个 .ssh 文件夹
包含两个文件,id_rsa 和 id_rsa.pub,前者是私钥,不能泄露出去,后者是公钥,用于认证身份,就是我们要保存到 GitHub 上的密钥值


## 9.代码托管到GitHub
### 9.1 注册账号,创建仓库
注册一个 GitHub 账户,访问新建仓库页面（导航栏“+” - New repository）,在“Repository name”处填写仓库名称  

这里填“base_flask”即可,接着选择仓库类型（公开或私有）等选项,最后点击“Create repository”按钮创建仓库.

### 9.2 设置 SSH 密钥
选中并复制id_rsa.pub输出的内容,访问 GitHub 的 SSH 设置页面（导航栏头像 - Settings - SSH and GPG keys）  

点击 New SSH key 按钮,将复制的内容粘贴到 Key 输入框里,再填一个标题,比如“My PC”,最后点击“Add SSH key”按钮保存


### 9.3 本地关联上Github仓库
我们已经提前创建了本地仓库,所以需要指定仓库的远程仓库地址（如果还没有创建,则可以直接将远程仓库克隆到本地）
```bash
git remote add origin git@github.com:yongnights/base_flask.git # 注意地址中的用户名
```
这会为本地仓库关联一个名为“origin”的远程仓库


## 10.创建虚拟环境
使用 Pipenv 来创建和管理虚拟环境、以及在虚拟环境中安装和卸载依赖包,集成了 pip 和 virtualenv,可以替代这两个工具  

还集成了 Pipfile,它是新的依赖记录标准,使用 Pipfile 文件记录项目依赖,使用 Pipfile.lock 文件记录固定版本的依赖列表  

这两个文件替代了手动通过 requirements.txt 文件记录依赖的方式


```bash
pip install pipenv  # 安装pipenv
pipenv install      # 为当前项目创建一个虚拟环境
pipenv shell        # 激活虚拟环境, 执行 exit 可以退出虚拟环境
pipenv run pip list # 在虚拟环境内执行  pip list  命令
```
pipenv install命令执行的过程包含下面的行为：
- 为当前目录创建一个 Python 解释器环境,按照 pip、setuptool、virtualenv 等工具库.
- 如果当前目录有 Pipfile 文件或 requirements.txt 文件,那么从中读取依赖列表并安装.
- 如果没有发现 Pipfile 文件,就自动创建

## 11.安装flask
无论是否已经激活虚拟环境,都可以使用下面的命令来安装 Flask
```bash
pipenv install flask
```

## 12.提交并推送
使用  git status 命令可以查看当前仓库的文件变动状态
```bash
git status
```
将文件改动提交进 Git 仓库,并推送到在 GitHub 上创建的远程仓库
```bash
git add .
git commit -m "I'm ready!"
git push -u origin master # 如果你没有把仓库托管到 GitHub,则跳过这条命令
```
最后一行命令添加了 -u 参数,会将推送的目标仓库和分支设为默认值,后续的推送直接使用 git push 命令即可

## 13.程序执行
按照惯例,把程序保存为 app.py,确保当前目录是项目的根目录,然后执行flask run  命令启动程序（按下 Control + C 可以退出）
```bash
flask run
```
Flask 默认会假设你把程序存储在名为 app.py 或 wsgi.py 的文件中  

如果你使用了其他名称,就要设置系统环境变量  FLASK_APP  来告诉 Flask 你要启动哪个程序

## 14.管理环境变量
在启动 Flask 程序的时候,通常要和两个环境变量打交道： FLASK_APP  和  FLASK_ENV   

因为我们的程序现在的名字是 app.py,暂时不需要设置  FLASK_APP   

FLASK_ENV  用来设置程序运行的环境,默认为  production    
 
在开发时,我们需要开启调试模式（debug mode）  

调试模式可以通过将系统环境变量  FLASK_ENV  设为  development  来开启.

调试模式开启后,当程序出错,浏览器页面上会显示错误信息；代码出现变动后,程序会自动重载

为了不用每次打开新的终端会话都要设置环境变量,我们安装用来管理系统环境变量的 python-dotenv
```bash
pipenv install python-dotenv
```
当 python-dotenv 安装后,Flask 会从项目根目录的 .flaskenv 和 .env 文件读取环境变量并设置  

创建这两个文件, .flaskenv 用来存储 Flask 命令行系统相关的公开环境变量  

而 .env 则用来存储敏感数据,不应该提交进Git仓库,把 .env 添加到 .gitignore 文件的结尾（新建一行）来让 Git 忽略它  

在新创建的 .flaskenv 文件里,写入一行  FLASK_ENV=development ,将环境变量  FLASK_ENV 的值设为  development  ,以便开启调试模式
```bash
FLASK_ENV=development
```

## 15.url_for函数
Flask 提供了一个  url_for  函数来生成 URL，它接受的第一个参数就是端点值，默认为视图函数的名称
```python
from flask import Flask,url_for

app = Flask(__name__)

@app.route('/')
def hello():
    return 'Hello'
    
@app.route('/user/<name>')
def user_page(name):
    return 'User: %s' % name
    
@app.route('/test')
def test_url_for():
    # 下面是一些调用示例：
    print(url_for('hello')) # 输出：/
    # 注意下面两个调用是如何生成包含 URL 变量的 URL 的
    print(url_for('user_page', name='greyli')) # 输出：/user/greyli
    print(url_for('user_page', name='peter')) # 输出：/user/peter
    print(url_for('test_url_for')) # 输出：/test
    # 下面这个调用传入了多余的关键字参数，它们会被作为查询字符串附加到 URL 后面。
    print(url_for('test_url_for', num=2)) # 输出：/test?num=2
    return 'Test page'
```

提交代码到仓库
```bash
git add .
git commit -m "Add minimal home page" # -m 参数给出简单的提交信息
git push
```

## 16.模板
按照默认的设置，Flask 会从程序实例所在模块同级目录的 templates 文件夹中寻找模板
```bash
mkdir templates
```
三种常用的定界符:
```jinja2
{{ ... }}  用来标记变量
{% ... %}  用来标记语句，比如 if 语句，for 语句等
{# ... #}  用来写注释
```
过滤器:
```jinja2
{{ 变量|过滤器 }}
```

## 17.静态文件
静态文件（static files）指的是内容不需要动态生成的文件。比如图片、CSS 文件和 JavaScript 脚本等

在 Flask 中，创建一个 static 文件夹来保存静态文件，它应该和程序模块、templates 文件夹在同一目录层级
```jinja2
mkdir static
```

## 17.1 生成静态文件 URL
假如在 static 文件夹的根目录下面放了一个 foo.jpg 文件，下面的调用可以获取它的 URL：
```jinja2
<img src="{{ url_for('static', filename='foo.jpg') }}">
```
花括号部分的调用会返回  /static/foo.jpg

在 Python 脚本里， url_for() 函数需要从 flask 包中导入，而在模板中则可以直接使用，因为 Flask 把一些常用的函数和对象添加到了模板上下文（环境）里

## 17.2 添加 Favicon
Favicon（favourite icon） 是显示在标签页和书签栏的网站头像  

一个 ICO、PNG 或GIF 格式的图片，大小一般为 16×16、32×32、48×48 或 64×64 像素

```jinja2
<link rel="icon" href="{{ url_for('static', filename='favicon.ico') }}">
```

保存后刷新页面，即可在浏览器标签页上看到这个图片

## 17.3 添加图片
在 static 目录下面创建一个子文件夹 images，把图片都放到这个文件夹里：
```jinja2
<img alt="Walking Totoro" src="{{ url_for('static', filename='images/totoro.gif') }}">
```

## 17.4 添加CSS
在 static 目录下创建一个 CSS 文件 style.css,在页面的<head>标签内引入这个 CSS 文件：
```jinja2
<link rel="stylesheet" type="text/css" href="{{ url_for('static', filename='style.css') }}">
```

## 18.数据库
选择属于关系型数据库管理系统（RDBMS）的 SQLite，它基于文件，不需要单独启动数据库服务器，适合在开发时使用，或是在数据库操作简单、访问量低的程序中使用。  

## 18.1 使用 SQLAlchemy 操作数据库
借助 SQLAlchemy，可以通过定义 Python 类来表示数据库里的一张表（类属性表示表中的字段 / 列），通过对这个类进行各种操作来代替写 SQL 语句。这个类称之为模型类，类中的属性我们将称之为字段。

使用一个叫做Flask-SQLAlchemy 的官方扩展来集成 SQLAlchemy,使用 Pipenv 安装它：
```bash
pipenv install flask-sqlalchemy
```

导入扩展类，实例化并传入 Flask 程序实例：
```python
from flask_sqlalchemy import SQLAlchemy # 导入扩展类
app = Flask(__name__)
db = SQLAlchemy(app) # 初始化扩展，传入程序实例 app
```

## 18.2 设置数据库 URI
用SQLALCHEMY_DATABASE_URI 变量来告诉 SQLAlchemy 数据库连接地址：
```python
import os
# ...
app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:////' + os.path.join(app.root_path, 'data.db')
```
SQLALCHEMY_DATABASE_URI变量值，不同的 DBMS 有不同的格式，对于 SQLite 来说，这个值的格式如下：
```text
sqlite:////数据库文件的绝对地址
```
数据库文件一般放到项目根目录， app.root_path 返回程序实例所在模块的路径（目前来说，即项目根目录），使用它来构建文件路径。
数据库文件的名称和后缀你可以自由定义，一般会使用 .db、.sqlite 和 .sqlite3 作为后缀。

如果你使用 Windows 系统，上面的 URI 前缀部分需要写入三个斜线(即sqlite:///).

采用兼容性处理后实际代码如下：
```python
import os
import sys
from flask import Flask

WIN = sys.platform.startswith('win')

if WIN: # 如果是 Windows 系统，使用三个斜线
    prefix = 'sqlite:///'
else: # 否则使用四个斜线
    prefix = 'sqlite:////'
    
app = Flask(__name__)

app.config['SQLALCHEMY_DATABASE_URI'] = prefix + os.path.join(app.root_path, 'data.db')
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False # 关闭对模型修改的监控

# 在扩展类实例化前加载配置
db = SQLAlchemy(app)
```

## 18.3 创建数据库模型
app.py：创建数据库模型
```python
class User(db.Model): # 表名将会是 user（自动生成，小写处理）
    id = db.Column(db.Integer, primary_key=True) # 主键
    name = db.Column(db.String(20)) # 名字
    
class Movie(db.Model): # 表名将会是 movie
    id = db.Column(db.Integer, primary_key=True) # 主键
    title = db.Column(db.String(60)) # 电影标题
    year = db.Column(db.String(4)) # 电影年份
```
模型类的编写有一些限制：
1. 模型类要声明继承  db.Model  。
2. 每一个类属性（字段）要实例化db.Column，传入的参数为字段的类型,下面列出了常用的字段类。
3. 在 db.Column()中添加额外的选项(参数)可以对字段进行设置。比如，primary_key设置当前字段是否为主键。除此之外，常用的选项还有nullable(布尔值，是否允许为空值)、 index(布尔值，是否设置索引)、 unique(布尔值，是否允许重复值)、 default(设置默认值)等。

常用的字段类型如下所示：
db.Integer 整型
db.String (size) 字符串，size 为最大长度，比如  db.String(20)
db.Text 长文本
db.DateTime 时间日期，Python  datetime  对象
db.Float 浮点数
db.Boolean 布尔值

## 18.4 创建数据库表

在 Python Shell 中创建创建表和数据库文件
```bash
$ flask shell
>>> from app import db
>>> db.create_all()
```

然后在项目根目录下出现了新创建的数据库文件 data.db。这个文件不需要提交到 Git 仓库，在 .gitignore 文件最后添加一行新规则：*.db

如果改动了模型类，想重新生成表模式，那么需要先使用db.drop_all()删除表，然后重新创建：
```bash
>>> db.drop_all()
>>> db.create_all()
```

注意这会删除所有数据，如果想在不破坏数据库内的数据的前提下变更表的结构，需要使用数据库迁移工具，比如集成了Alembic的Flask-Migrate扩展。

使用flask shell命令启动的Python Shell 激活了“程序上下文”，它包含一些特殊变量，这对于某些操作是必须的（比如上面的db.create_all()  调用）。

请记住，后续的 Python Shell 都会使用这个命令打开。

和flask shell类似，可以编写一个自定义命令来自动执行创建数据库表操作：
```python
import click

@app.cli.command() # 注册为命令
@click.option('--drop', is_flag=True, help='Create after drop.') # 设置选项
def initdb(drop):
"""Initialize the database."""
    if drop: # 判断是否输入了选项
        db.drop_all()
    db.create_all()
    click.echo('Initialized database.') # 输出提示信息
```

默认情况下，函数名称就是命令的名字，现在执行  flask initdb  命令就可以创建数据库表:
```bash
$ flask initdb
```

使用--drop选项可以删除表后重新创建：
```bash
$ flask initdb --drop
```

## 18.5 创建、读取、更新、删除

### 18.5.1 创建
下面的操作演示了如何向数据库中添加记录：
```bash
>>> from app import User, Movie # 导入模型类
>>> user = User(name='Grey Li') # 创建一个 User 记录
>>> m1 = Movie(title='Leon', year='1994') # 创建一个 Movie 记录
>>> m2 = Movie(title='Mahjong', year='1996') # 再创建一个 Movie 记录
>>> db.session.add(user) # 把新创建的记录添加到数据库会话
>>> db.session.add(m1)
>>> db.session.add(m2)
>>> db.session.commit() # 提交数据库会话，只需要在最后调用一次即可
```
最后一行db.session.commit()很重要，只有调用了这一行才会真正把记录提交进数据库，前面的db.session.add()调用是将改动添加进数据库会话(一个临时区域)中。

### 18.5.2 读取
通过对模型类的  query  属性调用可选的过滤方法和查询方法，我们就可以获取到对应的单个或多个记录(记录以模型类实例的形式表示)。
查询语句的格式如下：<模型类>.query.<过滤方法(可选)>.<查询方法>

下面是一些常用的过滤方法：
过滤方法     说明
filter()    使用指定的规则过滤记录，返回新产生的查询对象
filter_by() 使用指定规则过滤记录（以关键字表达式的形式），返回新产生的查询对象
order_by()  根据指定条件对记录进行排序，返回新产生的查询对象
group_by()  根据指定条件对记录进行分组，返回新产生的查询对象

下面是一些常用的查询方法：
查询方法          说明
all()           返回包含所有查询记录的列表
first()         返回查询的第一条记录，如果未找到，则返回None
get(id)         传入主键值作为参数，返回指定主键值的记录，如果未找到，则返回None
count()         返回查询结果的数量
first_or_404()  返回查询的第一条记录，如果未找到，则返回404错误响应
get_or_404(id)  传入主键值作为参数，返回指定主键值的记录，如果未找到，则返回404错误响应
paginate()      返回一个Pagination对象，可以对记录进行分页处理

下面的操作演示了如何从数据库中读取记录，并进行简单的查询：
```bash
>>> from app import Movie # 导入模型类
>>> movie = Movie.query.first() # 获取 Movie 模型的第一个记录（返回模型类实例）
>>> movie.title # 对返回的模型类实例调用属性即可获取记录的各字段数据
'Leon'
>>> movie.year
'1994'
>>> Movie.query.all() # 获取 Movie 模型的所有记录，返回包含多个模型类实例的列表
[<Movie 1>, <Movie 2>]
>>> Movie.query.count() # 获取 Movie 模型所有记录的数量
2
>>> Movie.query.get(1) # 获取主键值为 1 的记录
<Movie 1>
>>> Movie.query.filter_by(title='Mahjong').first() # 获取 title 字段值为 Mahjong 的记录
<Movie 2>
>>> Movie.query.filter(Movie.title=='Mahjong').first() # 等同于上面的查询，但使用不同的过滤方法
<Movie 2>
```

提示:我们在说Movie模型的时候，实际指的是数据库中的movie表。
表的实际名称是模型类的小写形式(自动生成)，如果你想自己指定表名，可以在数据库模型类中定义__tablename__属性。

### 18.5.3 更新
下面的操作更新了Movie模型中主键为2的记录：
```bash
>>> movie = Movie.query.get(2)
>>> movie.title = 'WALL-E' # 直接对实例属性赋予新的值即可
>>> movie.year = '2008'
>>> db.session.commit() # 注意仍然需要调用这一行来提交改动
```

### 18.5.4 删除
下面的操作删除了Movie模型中主键为1的记录：
```bash
>>> movie = Movie.query.get(1)
>>> db.session.delete(movie) # 使用 db.session.delete() 方法删除记录，传入模型实例
>>> db.session.commit() # 提交改动
```

## 19.在程序里操作数据库

## 19.1 在主页视图读取数据库记录
负责显示主页的  index  可以从数据库里读取真实的数据：
```python
@app.route('/')
def index():
    user = User.query.first() # 读取用户记录
    movies = Movie.query.all() # 读取所有电影记录
    return render_template('index.html', user=user, movies=movies)
```

## 19.2 生成虚拟数据
编写一个命令函数把虚拟数据添加到数据库里:
```python
@app.cli.command()
def forge():
    """Generate fake data."""
    db.create_all()

    name = 'Grey Li'
    movies = [
        {'title': 'My Neighbor Totoro', 'year': '1988'},
        {'title': 'Dead Poets Society', 'year': '1989'},
        {'title': 'A Perfect World', 'year': '1993'},
        {'title': 'Leon', 'year': '1994'},
        {'title': 'Mahjong', 'year': '1996'},
        {'title': 'Swallowtail Butterfly', 'year': '1996'},
        {'title': 'King of Comedy', 'year': '1999'},
        {'title': 'Devils on the Doorstep', 'year': '1999'},
        {'title': 'WALL-E', 'year': '2008'},
        {'title': 'The Pork of Music', 'year': '2012'},
    ]

    user = User(name=name)
    db.session.add(user)

    for m in movies:
        movie = Movie(title=m['title'], year=m['year'])
        db.session.add(movie)

    db.session.commit()

    click.echo('Done.')
``` 

执行flask forge命令就会把所有虚拟数据添加到数据库里：
```bash
$ flask forge
```

