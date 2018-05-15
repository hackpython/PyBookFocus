# 《Python Web开发实战》要点笔记

## 初始Python Web开发

### 1
主流web框架

Django：提供非常齐全的官方文档，提供了一站式解决方案，包含缓存、ORM、管理后台、验证、表单处理等，是开发复杂的数据库驱动网站变得很简单

Flask：只保留核心，通过扩展机制来增加其他功能，Flask用到的依赖都是pocoo团队开发的，基本上web应用的每个环节都有对应的扩展，自己实现一个也很简单

Pyramid：国内知名度不高，主要是中文文档匮乏，高级用法需要通过阅读源码的方式获得，Pyramid性能比Flask高，豆瓣赞赏、豆瓣钱包都是基于此框架

Bottle：单文件，代码只使用了Python标准库，不需要额外依赖其他第三方库，只有4100多行代码

Tornado：被Fackbook收购后开源，非阻塞式服务器，速度相当快。得益于非阻塞的方式和对epoll的运用，Tornado每秒可以处理数以千计的链接，这意味对于长轮询、WebSocket等实时Web服务器来说Tornado是个理想Web框架

Web.py：微框架、基于类的视图，简单易学却功能强大

选择使用哪种框架，要考社区是否活跃，文档是否齐全，没有最好的框架，只有最适合的框架

## Web开发前的准备

### 1
搭建虚拟机环境
 
书中使用Vagrant，Vargent是一个操作虚拟机的工具，它可以很快的完成一套开发环境的部署，解决了各个开发环境不一致的问题
 
个人觉得使用Docker更佳，不多记录Vargent

### 2
Docker是dotCloud开发的一个使用Go语言编写的基于Linux容器的容器引擎

具有很多优点

可以安装书中作者为我们提供好的Docker容器

```bash
docker pull dongweiming/web_develop:dev
```

进入Docker容器

```bash
docker run --name web_dev -it 9000:9000 -p 3141:3141 -p 5000:5000 dongweiming/web_develop /bin/zsh
```

--name指定容器名称为web_dev，如果不指定，会有系统随机取名

-p可以显示地暴力特定端口

### 3
pip内置到了python2.7.9和python3.4中

easy_install只支持安装，没有提供卸载、展示当前已安装的包列表等功能

pip支持二进制包使用wheel格式

pip支持虚拟环境功能virtualenv

pip可以集中管理项目依赖列表，requirement.txt，使用-r选项安装这些依赖

### 4
setuptools：

1.可以创建Eggs和Wheel格式的包
2.自带easy_install
3.支持PYPI商城，可以很方便把本地项目发布到PYPI中
4.支持测试集成


### 5
entry_points：发布的包经常需要一个或多个可执行的入口

entry_points机制还能直接作为插件系统

### 6
Python虚拟环境

virtualenv工具生成标准的python虚拟环境

实际使用汇总，项目都会有第三方包的依赖，会出现多个项目依赖相同的包的情况，如每个项目环境都需要Ipython，为了满足这种情况，可以定制virtualenv虚拟环境创建脚本，在python虚拟环境被创建时就安装好

生成自定义脚本内容，创建python3.6的虚拟环境，并默认安装flake8这个库

```python
'''
virtualenv创建虚拟环境脚本,create-venv-script.py
'''

import subprocess
import virtualenv

virtualenv_path = subprocess.check_output(['which','virtualenv']).strip()

extra_text = '''
def after_install(options,home_dir):
    subprocess.call(['{}/bin/pip'.format(home_dir), 'install', 'flake8'])
'''

def main():
    text = virtualenv.creat_bootstrap_script(extra_text, python_version='3.6')
    print('Updating%s'%virtualenv_path)
    with open(virtualenv_path, 'w') as f:
        f.write(text)
        
if __name__ == '__main__':
    main()
```

?> Flake8包装了下列工具：
?>PyFlakes：静态检查Python代码逻辑错误的工具。
?>pep8： 静态检查PEP 8编码风格的工具。
?>Ned Batchelder’s McCabe script：静态分析Python代码复杂度的工具

生成定制的virtualenv脚本

```bash
python create-venv-script.py
```

这样/usr/local/bin/virtualenv就被替换了，如果不想全局替换，可以吧virtualenv_path换成其他路径

此时再通过virtualenv创建python虚拟环境就会默认安装flake8了

virtualenv定制脚本可以接受3个扩展函数

1.extend_parser(optparse_parser):添加额外的选项
2.adjust_options(options,args):改变当前的选项
3.after_install:在默认的环境安装好之后，执行其他工作，主要通过这个函数完成定制工作

### 7
virtualenvwrapper，virtualenv的功能扩展，用它可以管理全部的虚拟环境，可以很方便的创建、删除、拷贝

virtualenv-burrito是一个安装、配置virtualenv和virtualenvwrapper及其他依赖的傻瓜式工具

autoenv让你在切换目录的时候可以完成自动激活虚拟环境等定制操作

目前我个人使用的虚拟环境和版本控制工具是pyenv

### 8
pip支持自动补全，对zsh用户友好，可以在zshrc文件中添加下面命令

```bash
pip completion --zsh >> ~/.zprofile
source ~/.zprofile
```

如果使用普通的bash，则使用下面命令

```bash
pip completion --bash >> ~/.profile
```

### 9
如果没有root权限，没有sudo权限，也不在虚拟环境里，pip默认的方式是无法安装包的，因为你没有将第三方库写入系统中相应文件中的权限，可以使用下面命令

```bash
pip install django --user
```

这样，安装的包就会放在当前用户的.local目录下

```bash
pip show django|grep location
```

为了让最近开发版代码及时生效，开发过程中可以使用pip的编辑模式

pip install -e . 会把开发目录作为包的路径，这样就可以实时修改了

### 10
pip缓存只针对当前的用户，如果公司使用python的规模很大，可以使用缓存代理的方式来提高下载效率，这样就不再需要依赖网络环境到PYPI下载包了

```bash
pip install devpi-server
```

启动devpi-server

```bash
devpi-server --host=0.0.0.0 --start
```

默认缓存服务器使用3141端口，开启缓存服务器后就可以使用-i参数指定使用缓存代理了

```bash
pip install -i http://localhost:3141/root/pypi tornado
```

当然可以吧这个'index-url'写入到配置文件中，这样下次使用pip就会使用配置文件中的index-url作为源了

```bash
mkdir -/.config/pip -p
cat ~/.config/pip/pip.conf
[global]
index-url = http://localhost:3141/root/pypi/+simple/
```

可以为devpi安装一个web界面

```bash
pip install -U devpi-web
```

重启 devpi-server

```bash
devpi-server --host=0.0.0.0 --stop
devpi-server --host=0.0.0.0 --start
```

然后就可以通过 127.0.0.1:3141来访问这个web界面了

### 11
可以使用bandersnatch实现PYPI镜像客户端，它可以帮我们创建一个包含了全部包的本地镜像服务

PYPI上所有包加起来有几百GB

## Flask Web开发

### 1

Flask：使用装饰器配置路由，用Blueprint实现模块化，请求、应用上下文

Flask主要依赖三个库：

1.Jinja2：默认模板引擎
2.Weerkzeug：一个包含WSGI、路由、调试的工具集
3.Itsdangerous：基于Django签名模块的签名实现

### 2

最简单的flask程序

```python
from flask import Flask # 1

app = Flask(__name__) # 2

@app.route('/') # 3
def hello_world():
    return 'Hello ayuliao!'

if __name__ == '__main__': # 4
    app.run(host='0.0.0.0', port=9000) # 5
```

第一行，引入了Flask类，Flask类实现了一个WSGI应用

第二行，app是Flask的实例，它接受包或者模块的名字作为参数，但一般传入\__name\__，让flask.helpers.get_root_path函数通过传入这个名字确定程序的根目录，以便获取静态文件和模板文件的目录

第三行，使用app.route装饰器会将URL和执行的视图函数的关系保存到app.url_map属性上，处理URL和视图函数的关系的程序就是路由

第四行，使用这个判断保证其他文件引用该文件时，不会执行判断内的代码，也不会执行app.run函数

第五行，执行app.run启动服务，Flask默认监听本地127.0.0.1这个地址，端口为5000

服务器启动后，会调用werkzeug.serving.run_simple进入轮询，默认使用单进程单线程的werkzeug.serving.BaseWSGIServer处理请求，实际上还是使用标准库Base-HTTPServer.HTTPServer，通过select.select做0.5秒的 "while True" 的事件轮询

当我们访问 http://127.0.0.1:9000时，通过app.url_map找到注册的 "/" 这个URL模式，就知道了对应的hello_world函数执行，如果访问不存在的路径，Flask在app.url_map中就找不到

默认的app.run启动方式只适合调试，不能再生产环境中使用，生产环境应该使用**Gunicorn或uWSGI**

werkzeug自带类型还包括 ThreadedWSGIServer、ForkingWSGIServer

### 3
app.config是flask.config.Config类的实例，继承自Python内置数据结构dict，使用update方法

```python
app.config.update(
    DEBUG=True,
    SECRET_KEY='...'
)
```

app.config支持多种更新配置的方式，假设现在有个较早settings.py的配置文件，可以通过下面方法添加配置

1.配置文件加载

```python
app.config.from_object('settings') #通过字符串的模块名字

# 或者引用之后直接传入模块对象
import settings
app.config.from_object(settings)
```

2.通过文件名字加载，直接传入文件名，不限于只使用.py后缀的文件名

```python
app.config.from_pyfile('settings.py', silent=True) #默认当配置文件不存在时会抛出异常，使用silent=True是值方位False，但不会抛出异常
```

### 4
app.run方式适用于本地开发服务器，但每次修改完代码后都要手动重启，不方便，启动调试模式服务器会在代码修改后自动重新载入

1.

```python
app.debug=True
app.run()
```

2.

```python
app.run(debug=True)
```

代码修改后自动重新载入的功能，可以通过脚本判断代码所在文件的修改日期与此前日期是否相同，不相同说明代码被修改了，重新载入则可

开启调试模式是一个巨大的安全隐患，Werkzeug从0.11开始默认启用PIN码做省份认证，当程序有异常进入错误栈模式时，第一次点击某个堆栈想查看其中错误参数时会被要求输入PIN码，只有输入正确的PIN码才能查看，Werkzeug将PIN作为Cookie的一部分存起来，失效前都不必再次输入，默认失效时间8小时

在调试模式下运行项目，会产生PIN码，下面的PIN为272-342-903

```bash
/Users/ayuliao/.pyenv/versions/ayupy35/bin/python3 /Users/ayuliao/Desktop/workplace/python_web_code/flask_project/hello_flask.py
 * Serving Flask app "hello_flask" (lazy loading)
 * Environment: production
   WARNING: Do not use the development server in a production environment.
   Use a production WSGI server instead.
 * Debug mode: on
 * Running on http://0.0.0.0:9000/ (Press CTRL+C to quit)
 * Restarting with stat
 * Debugger is active!
 * Debugger PIN: 272-342-903
 ```
 

 


