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

