flask——KISS之美   
--------------------------------------------
**作者: 江湖郎中**

这个月ownone由于工作原因无法按期与大家分享**web.py**的内容了，我在想找一个相当量级的内容与大家分享，**Django**太笨重了，**tornado**重点在IO，还是**flask**和**bottle**合适，个人对**flask** 稍有些了解，属于严重*入门级别*，打肿脸充胖子来和大家分享一下。

flask是什么？当然他不是flash,官网给出的说明：

> Flask is a microframework for Python based on Werkzeug, Jinja 2 and good intentions. And before you ask: It\'s BSD licensed!

github上的说明是:

>  Flask is a microframework for Python based on Werkzeug and Jinja2.  It\'s intended for getting started very quickly and was developed with best intentions in mind.

所以flsak

+ 使用Python写的
+ 一个微型框架
+ 建立在Werkzeug和Jinja2基础上
+ 采用BSD协议
+ 能够非常快速高效的开发

flask目前发布的最新版本是0.10。flask是开源项目托管在github上的，如果有兴趣可以直接git代码，地址是: <https://github.com/mitsuhiko/flask>。

flask的对外部的依赖很少，只需要Werkzeug,Jinja2,itsdangerours三个库，在setup.py文件中有定义:

```python
install_requires=[
    'Werkzeug>=0.7',
    'Jinja2>=2.4',
    'itsdangerous>=0.21'
    ]
```

好吧，不说太多的废话了先跑起第一个应用吧。

### 第一个应用

```python
#!/bin/env python
# file: hello.py
from flask import Flask
app = Flask(__name__)

@app.route("/")
def hello():
    return "Hello World!"

if __name__ == "__main__":
    app.run()
```

解释一下哈，`@app.route("/")`是Werkzeug的路由系统，它是通过python的修饰器来实现的，什么是修饰器吗？上期ownone已经在专题中讲过了，我就不废话了。
运行一下,

```bash
python hello.py
```
哈哈，就这么简单。通过`app.debug=True`或者`app.run(debug=True)`可以轻松的进入调试模式

### 路由

这就是路由，
```python
@app.route("/")
```

根据路由，flask将http请求交给对应的处理函数

路由系统的可以通过<var_name>的形式轻松与函数的参数结合起来，同时可以限制类型`int`、`float`、`path`，像这样`<int:port_id>`

```python
@app.route("/user/<name>")
def name(name):
    return name
```

路由是使用修饰器来实现的，比起django和web.py这些使用全局的定义要更灵活，但是萝卜青菜各有所爱，不如他们集中配置方便。

flsak支持完整的Restful接口，路由通过`methods=[GET, PUT]`形式来指定处理的类型，全局对象`request`的`method`可以获得请求类型

### 反向路由

什么是反向路由？
> 路由是根据地址进行路由，反向路由应该是根据路由得出地址。

它通过`url_for`系列函数可以自动构建出相应的URL来

```python
url_for("user", name="CROSS")
```

就会返回

> /user?name=CROSS

通过`redirect(url(`log`))`可以轻松完成请求的转发

### 模板

flask使用的模板系统是作者自己的`Jinja2`

模板的语法和django的模板系统差不多

```html
<!doctype html>
<!-- base.htm -->
<html>
<body>
<div>名称:</div>
{% block name%}
基础模板的内容
{% endblock%}
</body>
</html>
```

```html
<!-- name.html -->
{% extends base.html%}
{% block name -%}
{# 注释:减号是移除空白的 #}
{% if user %}
<div>{{user.name}}</div>
{% else %}
<div>他没有名字</div>
{% endif %}
{% endblock%}
```
是不是和django的模板一样呢。

模板的使用也很简单直接使用渲染器就可以

```python
from flask import  Flask, render_template 

app = Flask(__name__)

class User(object):
    def __init__(self):
        self.name = ""

@app.route("/name")
def name():
   user = User()
   user.name = "my_name"
   return render_template("name.html", user)

if __name__ == "__main__":
   app.run()
```

当然Jinja2是一套完整的模板系统，功能足够强大，过滤器、上下文、加载器等高级特性。

### 生产环境部署

这里我直接使用uwsgi来部署，当然你也应该使用nginx或者tornado来做前端。

uwsgi的配置文件支持xml、ini、yaml，个人感觉xml太繁琐了，ini和yaml不错，yaml通过缩进标识关系很有`python范`，所以这里就用yaml来展示一下配置

```yaml
#flask.yaml
uwsgi:
  pythonpath: /opt/flask_test
  module run.py
  callable: app
  processes: 5
  socket: /tmp/flask.socket
```
启动测试

> uwsgi --yaml flask.yaml

成功后可以将启动添加到系统服务中。

### 简单即是美

flask太简单了，以至于我们不得不去自己去做很多事情来使他完成我们的任务。某种程度上说他不应该是*框架*而是一个*库*。

flask虽小但是他通过`request`、`session`等对象漂亮的完成了对web相关的工作

flask的最大的亮点就是松耦合，flask只和web层面耦合，除了内置的路由系统和模板系统就没有内置其他功能了，真的很轻量级，但是flask可以快速的接入第三方库来完善自己的功能。

最后给出三个官方推荐的示例

<https://github.com/mitsuhiko/flask/tree/master/examples/flaskr/>

<https://github.com/mitsuhiko/flask/tree/master/examples/minitwit/>

<https://github.com/mitsuhiko/flask/tree/website>