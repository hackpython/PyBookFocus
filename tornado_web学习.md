# Tornado学习要点

## 概述

### 1
Tornado在处理高网络流量是表现的足够强健，创建和编程时又足够的轻量级，能够被大量的应用和工具中。

它提供了异步I/O支持、超时事件处理，这使得Tornado处理可以做web应用服务器，还可以用来做爬虫、超时事件处理、物联网关、游戏服务器等后台应用

提供了高效的HTTPClient，Tornado提供了高效的异步框架HTTP客户端，Tornado的HTTP服务器与Tornadao异步调用精密结合，可以直接用于生产环境

由于上面的特性，Tornado常用语大型站点的接口服务框架。

### 2
在python中，同步I/O可以理解为一个被调用的I/O函数会阻塞调用函数的执行，而异步I/O则不会阻塞调用函数的执行。

```python
from tornado.httpclient import HTTPClient

def synchronous_visit():
    http_client = HTTPClient()
    #阻塞，直到对 www.ayuliao.com 访问完成
    response = http_client.fetch('www.ayuliao.com')
    print(response.body)
```

**HTTPClient是Tornado的同步访问HTTP客户端**，上面代码使用了synchronous_visit()函数使用了**典型的同步I/O操作，该函数的执行时间取决于网络速度、对方服务器响应速度，**只有当对www.ayuliao.com访问完成并获取结果后，才能完成该函数的调用


**使用异步I/O访问www.ayuliao.com网站的函数无须等待访问完成才能返回**

```python
from tornado.httpclient import AsyncHTTPClient

def handle_response(response):
    print(response.body)
    
def asynchronous_visit():
    http_client = AsyncHTTPClient()
    http_client.fetch('www.ayuliao.com', callback=handle_response)
```

**AsyncHTTPClient是Tornado的异步访问HTTP客户端**

http_client.fetch()函数会在**调用后立刻返回而无须等待实际访问的完成**，AsyncHTTPClient会调用callback参数指定的函数，开发者可以在其中写入访问结果的逻辑代码。

### 3
Tornado协程可以开发出类型同步代码的异步行为（类似同步代码，但因为使用协程的特性，实质上是异步运行的）

因为协程本身不使用线程，所以减少了线程上下文切换的开销，是一种更高效的开发模式。

```python
from tornado import gen #引入协程库gen
from tornado.httpclient import AsyncHTTPClient

@gen.coroutine
def coroutine_visit():
    http_client = AsyncHTTPClient()
    response = yield http_client.fetch('www.ayuliao.com')
    print(response)
```

使用异步客户端AsyncHTTPClient进行页面访问，装饰器 `@gen.coroutione`声明这是一个协程函数。因为使用了`yield`关键字，使得代码中不在编写回调函数用于处理访问结果，而可以直接在`yield`语句后编写结果处理语句

### 4

Tornado协程基于python的yield实现，所以不能像调用普通函数一样调用协程函数

协程喊可以通过下面三种方式调用：

+ 本身是协程函数内通过`yield`关键字调用
+ 在IOLoop尚未启动时，通过IOLoop的run_sync()函数调用
+ 在IOLoop已经启动时，通过IOLoop的spawn_callback()函数调用


通过协程调用协程函数

```python
@gen.coroutine
def outer_coroutine():
    # 调用上面定义好的协程方法
    yield coroutine_visit()
```

outer_coroutine()函数和coroutine_visit()函数都是协程函数，之间可以通过yield关键字进行调用

### 5

IOLoop是Torando的主世界循环对象，Tornado程序通过它监听外部客户端请求，并执行相应的操作。

当程序没有进入IOLoop的running状态时，可以通过 `run_sync()函数`调用协程函数

```python
from tornado.ioloop import IOLoop

def func_normal():
    # 在IOLoop没有running前通过run_sync()函数调用异步方法
    IOLoop.current().run_sync(lambda: coroutine_visit())
```

引用了tornado.ioloop包中的IOLoop对象，之后在普通函数中使用run_sync()函数调用**经过lambda封装的协程函数**。run_sync()函数将**阻塞当前函数的执行，**直到被调用的协程执行完成。

**Tornado要求协程函数在IOLoop的running状态中才能被调用，只不过run_sync()函数自动完成了启动、停止IOLoop步骤**

?>run_sync()函数实现逻辑：启动IOLoop-->调用被lambda封装的协程函数-->停止IOLoop

### 6

当Tornado程序已经处于running状态是的协程函数的调用如下：

```python
from tornado.ioloop import IOLoop

def func_normal():
    print('start func_normal')
    # IOLoop已经running，调用协程函数
    IOLoop.current().spawn_callback(coroutine_visit)
    print('end func_normal')
```

spawn_callback()函数不会等待被调用协程执行完成，所有spawn_callback()之前的之后的pintg语句将会被连续执行，**而coroutine_visit()会由IOLoop在合适的时机进行调用。

**IOLoop的spawn_callback()函数没有为开发者提供获取协程函数调用返回值的方法，所以只能用spawn_callback()调用没有返回值的协程函数**

### 7

在协程中调用阻塞函数

在线程中直接调用阻塞函数会影响协程本身的性能，所以Tornado提供了在协程中利用线程池调度阻塞邯郸，从而不影响协程本身继续执行的方法。

```python
from concurrent.futures import ThreadPoolExecutor
import time

thread_pool = ThreadPoolExecutor(2)

def mySleep(count):
    for i in range(count):
        time.sleep(1)

@gen.coroutine
def call_blocking():
    print('start call_blocking')
    # 阻塞函数交个了其他函数，本方法依旧是异步方法
    yield thread_pool.submit(mySleep, 10)
    print('end call_blocking')
```

代码中引入了ThreadPoolExecutor类，实例化了一个拥有两个线程的线程池thread_pool，需要调用阻塞函数的协程call_blocking，使用`hread_pool.submit`调用阻塞函数，并通过yield返回。**这样不好阻塞协程所在的线程的继续执行，也保证了阻塞函数前后代码的执行顺序**

### 8

协程中等待多个异步调用

Tornado允许在协程中用一个yield等待多个异步调用，只需把这些调用用list或dict的方式传递给yield关键字则可

```python
from tornado import gen
from tornado.httpclient import AsyncHTTPClient

@gen.coroutine
def corouting_visit():
    http_client = AsyncHTTPClient()
    list_response = yield [http_client.fetch('www.ayuliao.com'),http_client.fetch('www.hackpython.com')]

    for response in list_response:
        print(response.body)
```

依旧使用@gen.coroutine装饰器定义协程，在需要yield的地方用列表传递若干个异步调用，只有在列表汇总的所有调用执行完后，yield才会返回并继续执行。

yield以列表方式返回N个调用的输出结果，可以通过for语句逐个访问

当然，以dict传递内容个yield也是一样的

```python
@gen.coroutine
def corouting_visit():
    http_client = AsyncHTTPClient()
    dict_response = yield {'ayuliao':http_client.fetch('www.ayuliao.com'),'hackpython':http_client.fetch('www.hackpython.com')}

    print(dict_response['hackpython'].body)
```

### 9 

通过tornado实现一个网站基本的架构

```python
import tornado.ioloop
import tornado.web


class MainHandler(tornado.web.RequestHandler):
    def get(self):
        self.write('Hello hackpython')


def make_app():
    return tornado.web.Application([
        (r'/', MainHandler),
    ])

def main():
    app = make_app()
    app.listen(8888)
    tornado.ioloop.IOLoop.current().start()

if __name__ == '__main__':
    main()
```

上面的代码，逻辑如下：

1.import了tornado的ioloop和web类型，这两类是tornado程序的基础

2.实现了web.RequestHandler子类，重载其中的get()函数，**该函数负责相应定位到该RequestsHandler的HTTP GET请求的处理。

3.定了make_app()，返回一个web.Application对象，用于Tornado程序的路由映射，这里映射到了MainHandler类

4.用web.Application.listen()函数指定服务监听端口

5.tornado.ioloop.IOLoop.current().start()启动IOLoop，**该函数一直运行且不推出，用于处理完所有客户端的访问请求**

### 10

Tornado的路由字符串有两种：固定字符串路径和参数字符串（python中大多数web框架的路由都是类似形式的）

1.固定字符串路径

```python
Handlers = [
    (r'/', MainHandler),
    (r'/login', LoginHandler ),
    (r'/post/1', PostHandler),
]
```

2.参数字符串路径

参数字符串可以将具有一定模式的路径映射到RequesetHandler中处理，**路径中的参数部分用小括号（）标识**

```python
class PostHandler(tornado.web.RequestHandler):
    def get(self, postnum):
        content = self.db.get('SELECT * FROM post where id =%s', postnum)
        if not content:
            raise tornado.web.HTTPError(404)
        self.render("post.html", content=content)
            

Handlers = [
    (r'/', MainHandler),
    (r'/post/([^/]+)', PostHandler),
]
```

上面使用了`/post/(^/]+)`，会匹配以`/post/`开头的URL模式，小括号中的内容是正则表达式，**URL尾部的变量部分以参数形式传递个RequestHandler的get()函数**

但上面的路由无法匹配 `http://xx.xx/post`

为了匹配这种情况，改变一下URL的逻辑和对应get()函数的定义

```python
class PostHandler(tornado.web.RequestHandler):
    def get(self, postnum=1):
        content = self.db.get('SELECT * FROM post where id =%s', postnum)
        if not content:
            raise tornado.web.HTTPError(404)
        self.render("post.html", content=content)

Handlers = [
    (r'/', MainHandler),
    (r'/post/([^/]*)', PostHandler),
]
```

先用星号`*`取代加号`+`定义URL模式`/entry/([^/]*)`，然后为RequestHandler子类的get()函数的postnum参数配置了默认值`1`

### 11

需要子类继承并定义具体的行为的函数在RequestHandler中被称为接入点函数（Entry Point)，如前面常用的get()函数就是典型的接入点函数。

1.RequestHandler.initialize()

该方法被子类重写，实现requestHandler子类实例的初始化过程，可以传递参数，参数来源与配置URL映射时的定义

```python
from tornado.web import RequestHandler
from tornado.web import Application

class ProfileHandler(RequestHandler):
    def initialize(self, database):
        self.database = database
        
    def get(self):
        pass
    
    def post(self):
        pass
    
app = Application([
    (r'/account', ProfileHandler, dict(database="\home\ayuliao\test.db")),
    
])
```

上面代码中initialize有参数database，该参数由Application定义URL映射时，以dict方式给出。

2.RequestHandler.prepare()、RequestHandler.on_finish()

**prepare()方法用于调用请求处理（get、post）方法之前的初始化处理，on_finish()用于请求结束后的一些清理工作**

通常prepare()方法做**资源初始化操作**，而on_finish()方法做**清理对象占用内存或关闭数据库连接等工作**

3.HTTP Action处理函数

每个HTTP Action 在RequestHandler中都有单独的函数进行处理

+ RequestHandler.get(\*args, \**kwargs)
+ RequestHandler.post(\*args, \**kwargs)
+ RequestHandler.head(\*args, \**kwargs)
+ RequestHandler.delete(\*args, \**kwargs)
+ RequestHandler.put(\*args, \**kwargs)

### 12

输入捕捉，可以使用`RequestHandler.get_argument(name)`和`RequestHandler.get_arguments(name)`

都是返回给定参数的值，get_argument获得单个值，而get_arguments针对参数存在**多个值的情况下使用**

RequestHandler.get_query_argument(name)和RequestHandler.get_query_arguments(names)只能从URL查询参数中获取参数，也就是**只能用于GET方法**

RequestHandler.get_body_argument(name)和RequestHandler.get_body_arguments(name)只能从POST提交的参数中获取参数值

**一般使用RequestHandler.get_argument(name)和RequestHandler.get_arguments(name)就可以了，这两个方法会无差别获取GET或POST请求中的值**

### 13
更加cookie获取cookie值

RequestHandler.request

返回`tornado.httputil.HTTPServerRequest`对象实例的属性，通过该对象获取关于HTTP请求的一切信息

其中常用的对象属性


|属性名|描述|
|---|---|
method|HTTP请求方法，GET/POST
uri|客户端请求的完整URL内容
path|uri路径名
query|uri中查询字符串
headers|字典方式表达HTTP Headers
body|字符串方式表达HTTP消息体
remote_ip|客户端IP
protocol|请求协议，如HTTPS
host|请求消息中的主机名
arguments|客户端提交的所有参赛
files|字典方式表达客户端上传的文件，每个文件名对应一个HTTPFile
cookies|客户端提交的Cookie字典

### 14

输出响应为客户端生成处理结果的工具函数，常用输出函数

1.RequestHandler.set_status(status_code,reason=None)

设置HTTP Response返回码，若有描述语句，传给reason参数则可

2.RequestHandler.set_header(name, value)

以键值对的方式设置HTTP Response中的HTTP头参数，使用set_header配置的Heaser值将覆盖之前配置的Header

```python
import tornado.web

class DetailHandler(tornado.web.RequestHandler):
    def get(self):
        self.set_header('NAME','ayuliao')
        self.set_header('NAME','hackpython')
```

上面使用了get()函数调用了2次set_header，后面设置的NAME将会覆盖前面设置的NAME

3.RequestHandler.add_header(name,value)

以键值对的方式设置HTTP Response中的HTTP参数，与set_header不同的是，它不会覆盖

4.RequestHandler.write(chunk)

将给定的块作为HTTP Body发送给客户端。

一般情况下，该函数输出字符串给客户端，如果给定的块是一个字典，贼会将这个块以json格式发送个客户端，同时将HTTP Headeer中的Content_Type设置成application/json

5.RequestHandler.finish(chunk=None)

当Tornado：Response生成工作完，chunk参数是需要传递给客户端的HTTP body。调用finish()后，Tornado将想客户端发送HTTP Response。

该方法适用于RequestHandler的异步请求处理。

6.RequestHandler.render(template_name, **kwargs)

给定的参数渲染模板。该函数传入模板文件名称和模板参数

**Tornado的基本模板语法与Django相同，但功能被弱化了，高级过滤器不可用**

```python
import tornado.web

class MainHandler(tornado.web.RequestHandler):
    def get(self):
        items = ['Python', 'C++','Java']
        self.render('index.html',title='Test',items=items)
```

7.RequestHandler.redirect(url, permanent=False, status=None)

进行页面重定向

```python
import tornado.web

class LoginHandler(tornado.web.RequestHandler):
    def get(self):
        self.render('login.html',next=self.get_argument('next','/'))
        
    def post(self):
        username = self.get_argument("username","")
        password = self.get_argument("password","")
        # 查找数据库中有无匹配的账号密码
        auth = self.db.authenticate(username, password)
        
        if auth:
            self.set_current_user(username)
            self.redirect(self.get_argument('next','/'))
        else:
            error_msg = 'login error'
            self.redirect('/login'+error_msg)
```

LoginHandler的post处理函数汇总，根据验证结果将成功登陆和不成功登陆的重定向到不同的页面中

成功重定向到next参数所指向的URL；不成功，重定向到`/login`页面

8.RequestHandler.clear()

清空所有在本次请求中之前写入的Header和Body内容

```python
import tornado.web

class DetailHandler(tornado.web.RequestHandler):
    def get(self):
        self.set_header('NAME',8)
        self.clear()
        self.set_header('AGE',28)
```

最后的Header中不包含参数`NAME`

9.RequestHandler.set_cookie(name,value)

按键/值对设置Response中的Cookie值

10.RequestHandler.clear_all_cookies(path='/', domain=None)

清空本次请求中所有的Cookie

### 15

Tornado有两种方式可改变同步的处理流程

+ 异步化：针对RequestHandler处理函数使用`@tornado.web.asynchronous`装饰器，将默认的同步机制改为异步机制
+ 协程化：针对RequestHandler处理函数使用`@tornado.gen.coroutine`装饰器，将默认的同步机制改为协程机制

异步化如下：

```python
import tornado.web
import tornado.httpclient


class MainHalder(tornado.web.RequestHandler):
    @tornado.web.asynchronous
    def get(self):
        http = tornado.httpclient.AsyncHTTPClient()
        http.fetch('http://www.hackpython.com', callback=self.on_response)
        
    def on_response(self, response):
        if response.error: raise tornado.web.HTTPError(500)
        self.write(response.body)
        self.finish()
```

`@tornado.web.asynchronous`装饰器定义了get()方法，**当get()函数返回时，对该HTTP房屋的请求尚未完成，所以Tornado无法发送HTTP Response给客户端。**（get()函数运行完了，内容还没有返回给客户端，异步了）

只有当随后的on_response()中的finish()函数被调用时，Tornado才知道本次处理已完成，可以发送给Response给客户端

异步编程提供服务器的并发能力，但在编程方法上交同步显得更加繁琐

协程化

```python
import tornado.web
import tornado.httpclient

class MainHandler(tornado.web.RequestHandler):
    @tornado.gen.coroutine
    def get(self):
        http = tornado.httpclient.AsyncHTTPClient()
        response = yield http.fetch('http:hackpython.com')
        self.write(response.body)
```

+ `@tornado.gen.coroutine`装饰MainHandler的get()、post()等处理函数
+ 异步对象处理耗时处理，如AsyncHTTPClient
+ 调用yield关键字获取异步对象的处理结果

### 16

Tornado框架本身较其他python web框架集成了最为丰富的用户身份验证功能，使用该框架，可以快速开发出强大安全的用户身份认证机制。

Cookie是很多网站辨别用户身份而存储在本地终端（client side）的数据，定义RFC2019.

在Tornado中使用RequestHandler.get_cookie()、RequestHandler.set_cookie()可以方便对Cookie进行读写。

```python
import tornado.web

session_id = 1

class MainHandler(tornado.web.RequestHandler):
    def get(self):
        global session_id
        if not self.get_cookie('session'):
            self.set_cookie('session', str(session_id))
            session_id = session_id + 1
            self.write("get a new session")
        else:
            self.write('session was set')
```

实际应用中，Cookie经常像这样保存于Session信息中

因为Cookie总是被保存到客户端，保证cookie不被篡改是服务器程序要解决的问题。

Tornado提供了Cookie信息加密机制，使得客户端无法随意解析和修改Cookie的键值。

使用安全cookie的网站实例

```python
import tornado.web
import tornado.ioloop

session_id = 1

class MainHandler(tornado.web.RequestHandler):
    def get(self):
        global session_id
        if not self.get_secure_cookie('session'):
            self.set_secure_cookie('session', str(session_id))
            session_id = session_id + 1
            self.write('session got a new')
        else:
            self.write('session was set')

application = tornado.web.Application([
    (r'/', MainHandler),
], cookie_secret= 'SECRET_DONT_LEAK')

def main():
    application.listen(8888)
    tornado.ioloop.IOLoop.current().start()

if __name__ == '__main__':
    main()
```

tornado.web.Application对象初始化时传递了cookie_secret参数，该参数是一个字符串，用于保存本站Cookie加密时的密钥

需要读取Cookie的地方，使用RequestHandler.get_secure_cookie替换RequestHandler.get_cookie调用

需要写入Cookie的地方用的RequestHandler.set_secure_cookie替换原理的RequestHandler.set_cookie调用

开发者无需担心Cookie的伪造问题

?>cookie_secret参数是Cookie加密密钥，不可泄露

### 17

RequestHandler类中有current_user属性用于保存当前请求的用户名，默认是None，在get()、post()处理函数中可以随时读取该属性以获取当前的用户名

`RequestHandler.current_user`只是一个只读属性，开发者需要重载RequestHandler.get_current_user()函数设置该属性

下面实例实现用户身份控制

```python
 import tornado
import tornado.web
import tornado.ioloop
import uuid ## UUID生成库


dict_session = {} #所有登录的Session

class BaseHandler(tornado.web.RequestHandler):#公共基类
    def get_current_user(self): #写入current_user的函数
        sesssion_id = self.get_secure_cookie('session_id')
        return dict_session.get(sesssion_id)

class MainHandler(BaseHandler):
    @tornado.web.authenticated #需要省份认证才能访问的处理器
    def get(self):
        # 转义一个字符串使它在HTML 或XML 中有效.
        name = tornado.escape.xhtml_escape()
        self.write('Hello,'+name)

class LoginHandler(BaseHandler):
    def get(self):
        self.write('''<!DOCTYPE html>
                    <html lang="en">
                    <head>
                      <meta charset="UTF-8">
                      <title>tornado test</title>
                    </head>
                    <body>
                      <form action="">
                        <input type="text" name="name">
                        <input type="submit" value="login in"></form>
                    </body>
                    </html>''')

    def post(self):
        if len(self.get_argument('name')) < 3:
            self.redirect('/login')
            session_id = str(uuid.uuid4())
            dict_session[session_id] = self.get_argument('name')
            self.set_secure_cookie('session_id', session_id)
            self.redirect('/')

application = tornado.web.Application(
    [   #URL映射定义
        (r'/', MainHandler),
        (r'/login', LoginHandler),
    ],
    cookie_secret = '123', #Cookie加密密钥
    login_url='/login'#登录不成功，tornado.web.authenticated自动重定向到该URL上
)

def main():
    application.listen(8888)
    tornado.ioloop.IOLoop.current().start()

if __name__ == '__main__':
    main()
```

+ 全局字典dict_sessons保存已登录用户信息
+ 定义公共基类 `BaseHandler`，该类继承自tornado.web.RequestHandler，**用于定义本网站的所有处理器的功能属性和行为，重载它的get_cureent_user()函数，在其开发者访问RequestHandler.current_user属性时自动被Tornado调用**，该函数首先用`self.get_secure_cookie`方法获得本次访问回答的ID，然后用绘画ID才能从dict_sessions中获得用户名并返回
+ MainHander类是一个要求经过用户身份认证才能访问的处理器实例，get()方法使用了装饰器 `tornado.web.authenticated`，具有该装饰器的处理函数**在执行前更加current_user是否已经被赋值来判断用户的身份情况，如果已经被赋值，则可以进行正常逻辑，否则自动重定向到网站的登录页面
+ LoginHandler类是登录页面处理器，其get()喊用于渲染登录页面，post()函数用于验证是否允许用户登录
+ tornado.web.Application的初始化函数中通过**login_url参数给出网站的登录页面地址**，该地址被用于tornado.web.authenticated装饰器在发现用户尚未验证时重定向到一个URL

?>加入身份认证的所有页面处理器需要继承自BaseHandler雷，而不是直接继承原理的tornado.web.RequestHandler类

### 18

CSRF或XSRF跨站请求伪造是一种对网站的恶意利用。通过CSRF，攻击者可以冒用用户的身份，在用户不知情的情况下执行恶意操作。

CSRF攻击原理

+ 1.用户访问了存在CSRF漏洞的网站A，成功登录并获取到网站A的Cookie后，以后该用户对网站A的访问均会携带该Cookie，网站A通过该Cookie认证用户的身份
+ 2.此时用户又访问了网站B，网站B返回页面中带有一个访问网站A进行而已操作链接，但表面上伪装成合法内容，如:`<ahref="www.siteA.com/get_money?amount=5000$dest_card=xxxxx">贡献你获得了十万元大抽奖机会</a>`
+ 3.用户一旦点击恶意链接，就在不知情的情况下向网站A站点发送了请求并携带了网站A的cookie作为身份认证，网站A受到用户的请求和附带的Cookie时会任务该请求是用户发出的正常请求，就执行请求对应的操作，此时网站B就完成了CSRF恶意攻击的目的


Tornado为了防范CSRF攻击，要求每个请求包括一个参数作为令牌来匹配存储在Cookie中对应值。Tornado可以通过一个Cookie头和一个隐藏的HTML表单元素向页面提供令牌。这样合法页面表单提交时，就好将表单值和已存储的Cookie都传递给Tornado，如果两者匹配，则认为请求有效

开启Tornado的CSRF防范功能需要两个步骤

1.实例化tornado.web.Application是传入 xsrf_cookies=True参数

```python
application = tornado.web.Application([
    (r'/', MainHandler),
    (r'/login', LoginHandler),
], cookie_secret="123", xsrf_cookies=True,)
```

或者

```python
settings = {
    "cookie_secret":"123",
    "xsrf_cookies":True
}

applications = tornado.web.Application([
    (r'/', MainHandler),
    (r'/login', LoginHandler),
], **settings)
```

?>当tornado.web.Application需要初始化的参数过多时，可以使用第二种方式

2.在每个具有HTML表单的模板文件中，为所有表单添加xsrf_from_html()函数标签，如：

```html
<form action="/login" method="post">
  {% module xsrf_form_html() %}
  <input type="text" name="name">
  <input type="submit" value="post">
</form>
```

`{% module xsrf_form_html() %}`为表单添加了隐藏元素，以防止跨站攻击

### 19

Tornado异步特性让它非常适合服务器高并发处理，客户端与服务器的持久链接应该框架就是高并发的典型应用。

WebSocket正是HTTP客户端与服务器之间建立持久连接的HTML5标准技术

WebSocket protocol是HTML5定义的标准协议 RFC6455，实现了浏览器与服务器的全双工通信

WebSocket与普通Socket通信类似，打破了原来的HTTP的Request和Response一对一通信模型，**同时打破了服务器只能被动接受客户端请求的应用场景**

传统的HTTP+HTML方案只适用于客户端主动发出请求的场景，而无法满足服务器端发起的通信要求。

WebSocket正是为了解决传统HTTP遇到的问题，相对于普通的Socket通信，WebSocket在应用层定义了基本的交互流程，是的Tornado服务器框架和JavaScript客户端可以构建出标准的WebSocket模块

WebSocket特定如下：

+ WebSocket适合服务器**主动**推送的场景
+ 相对于Ajax和Long poll，WebSocket通信模型更高效
+ WebSocket仍然与HTTP完成Internet通信
+ WebSocket是HTML5标准协议，不受企业防火墙拦截

### 20

WebSocke通信原理是在**客户端与服务器之间建立TCP持久链接，使得当前服务器有消息需要推送给客户端时能够进行即时通信**

**WebSocket不是HTTP，但由于在Internet上的HTML本身有HTTP封装进行创世的，所以WebSocket仍然需要与HTTP进行协作，IETF在RFC6455中定义了基于HTTP链路建立WebSocket**


