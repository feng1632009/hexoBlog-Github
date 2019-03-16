---
title: Flask核心笔记
date: 2018-11-07 09:25:00
author: 空灵
img:  /medias/featureimages/3.jpg
top: true
cover: true
coverImg: 
password: 8d969eef6ecad3c29a3a629280e686cf0c3f5d5a86aff3ca12020c923adc6c92
toc: true
mathjax: false
summary: Flask核心笔记
categories: Flask
tags:
  - Flask
  - Python
---
# 			flask核心知识点

#### flask的上下文对象

flask有两种Context(上下文)，分别是

- RequestContext   请求上下文
- Request   请求的对象，封装Http请求(environ)的内容
- Session 根据请求中的cookie，重新载入该访问者相关的会话信息
- AppContext  程序上下文
- g 处理请求时用作临时存储的对象。每次请求都会重设这个变量
- current_app 当前激活程序的程序实例

生命周期:

- current_app的生命周期最长，只要当前程序实例还在运行，都不会失效。
- Request和g的生命周期为一次请求期间，当请求处理完成后，生命周期也就完结了
- Session 就是传统意义上的session了。只要它还未失效(用户未关闭浏览器、没有超过设定的失效时间)，那么不同的请求会共用同样的session

#### flask处理流程

![img](https://ws2.sinaimg.cn/large/006tKfTcgy1fryvtjc1xbj30hi0ejt93.jpg)

第一步：创建上下文

Flask根据WSGI Server封装的请求等的信息(environ)新建RequestContext对象 和AppContext对象

```python
# 声明对象
# LocalStack  LocalProxy 都由Werkzeug提供
# 我们不深究他的细节，那又是另外一个故事了，我们只需知道他的作用就行了
# LocalStack 是栈结构，可以将对象推入、弹出
# 也可以快速拿到栈顶对象。当然，所有的修改都只在本线程可见。
_request_ctx_stack = LocalStack()
_app_ctx_stack = LocalStack()
# 如果调用一个LocalStack实例， 能返回一个 LocalProxy 对象
# 这个对象始终指向 这个LocalStack实例的栈顶元素。
# 如果栈顶元素不存在，访问这个 LocalProxy 的时候会抛出 RuntimeError异常
# LocalProxy对象你只需暂时理解为栈里面的元素即可了
current_app = LocalProxy(_find_app)
request = LocalProxy(partial(_lookup_req_object, 'request'))
session = LocalProxy(partial(_lookup_req_object, 'session'))
g = LocalProxy(partial(_lookup_app_object, 'g'))

```

```python
# RequestContext
class RequestContext(object):
    def __init__(self, app, environ, request=None):    
        self.app = app    
        if request is None:        
                request = app.request_class(environ)    
        self.request = request    
        self.url_adapter = app.create_url_adapter(self.request)    
        self.flashes = None    
        self.session = None
        
        
```

```python
#AppContext
class AppContext(object):
    def __init__(self, app):    
        self.app = app    
        self.url_adapter = app.create_url_adapter(None)    
        self.g = app.app_ctx_globals_class()    
        self._refcnt = 0
```



这里需要注意的是，RequestContext在初始化的时候，当前Flask的实例作为参数被传进来。虽然每次的请求处理都会创建一个RequestContext对象，但是每一次传入的app参数却是同一个。通过这个机制，可以使得：

> 由同一个Flask实例所创建的RequestContext, 其成员变量app都是同一个Flask实例对象。实现了多个RequestContext对应同一个 current_app 目的



- 第二步：入栈

  将RequestContext对象push进_request_ctx_stack里面。在这次请求期间，访问request对象， session对象将指向这个栈的栈顶元素

```python
class RequestContext(object):
    def push(self):   
        ....
        _app_ctx_stack.push(self)   
        appcontext_pushed.send(self.app)
```

AppContext对象push进_app_ctx_stack里面。在这次请求期间， 访问g对象将指向这个栈的栈顶元素

```python
class AppContext(object):
    def push(self):   
        ....
        _request_ctx_stack.push(self)
```

- 第三步:请求分发

  ```python
  response = self.full_dispatch_request()
  ```

  Flask将调用`full_dispatch_request`函数进行请求的分发，之所以不用给参数，是因为我们可以通过`request`对象获得这次请求的消息。 `full_dispatch_request`  将根据请求的url找到对应的蓝本里面的视图函数，并生成一个`response`对象。注意的是在请求之外的时间，访问request对象是无效的，因为request对象依赖请求期间的`_request_ctx_stack`栈。

- 第四步： 上下文对象出栈

  这次HTTP的响应已经生成了，就不需要两个上下文对象了。分别将两个上下文对象出栈，为下一次的HTTP请求做出准备。

- 第五步：响应WSGI

  调用Response对象，向WSGI Server返回其结果作为HTTP中文。Response对象是一个可调用对象，当调用发生的时，将首先执行的WSGI服务器传入的start_response()函数 发送验证码和HTTP报文头。

最后附上Flask处理请求的wsgi_app函数

```python
# environ: WSGI Server封装的HTTP请求信息
# start_response: WSGI Server提供的函数，调用可以发送状态码和HTTP报文头
def wsgi_app(self, environ, start_response):
    # 根据environ创建上下文
    ctx = self.request_context(environ)
    # 把当前的request context,app context绑定到当前的context
    ctx.push()
    error = None
    try:
        try:
            #根据请求的URL，分发请求，经过视图函数处理后返回响应对象
            response = self.full_dispatch_request()    
        except Exception as e:        
            error = e        
            response = self.make_response(self.handle_exception(e))   
        return response(environ, start_response)
    finally:   
        if self.should_ignore_error(error):        
          error = None    
       # 最后出栈
        ctx.auto_pop(error)
```

