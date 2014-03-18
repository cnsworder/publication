<link rel="stylesheet" href="http://ssh.cnsworder.com/styles/monokai_sublime.css" />
<script type="text/javascript" src="http://ssh.cnsworder.com/highlight.pack.js"></script>
<script type="text/javascript">
    hljs.initHighlightingOnLoad();
</script>

GNU/Linux Developer
==============================================================  
![Tux](http://ssh.cnsworder.com/img/tux.png)  
**Alpha3**  
**kernel stable： 3.13**  
**QQ群号： 20506135**  
**微信号： linux_developer**  
**主编： 猫猫**  

《GNU/Linux Developer》第Alpha4期在春风中来了了，本期**九州**有**大数据**、**android系统定制**两个专题和大家分享，另外由于*ownone*工作太忙了**web.py**的专题将会在4月份继续和大家见面，这期**江湖郎中**会给大家带来**flask**的专题分享。     

前言
-----------


专题一 
--------------------
**作者: **



专题二  flask——KISS之美   
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

flask目前发布的最新版本是0.10。flash是开源项目托管在github上的，如果有兴趣可以直接git代码，地址是: <https://github.com/mitsuhiko/flask>。

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
解释一下哈，`@app.route("/")`是Werkzeug的路由系统，它是通过python的修饰器来实现的，

运行一下,
```bash
python hello.py
```
哈哈，就这么简单。

### 路由

这就是路由，
```python
@app.route("/")
```
根据路由，flask将http请求交给对应的处理函数

路由是使用修饰器来实现的，比起django和web.py这些使用全局的定义要更灵活，但是萝卜青菜各有所爱，不如他们集中配置方便。

### 反向路由

什么是反向路由？
> 路由是根据地址进行路由，反向路由应该是根据路由得出地址。

它通过`url_for`系列函数来使用

### 模板

flask使用的模板系统是作者自己的`jinja2`

模板的语法和django的模板系统差不多

```html
{% base base.html%}
{% block name%}
<div>{{user.name}}</div>
{% endblock%}
```
是不是和django的模板一样呢。

### 其他支持

```python
app.register_module()
```

flask的最大的亮点就是松耦合，flask只和web层面耦合，除了内置的路由系统和模板系统就没有内置其他功能了，真的很轻量级，但是flask可以快速的接入第三方库来完善自己的功能。

### 生产环境部署

这里我直接使用uwsgi来部署，当然你也可以使用tornado来做前端

### 简单即是美

flask太简单了，以至于我们不得不去自己去做很多事情来使他完成我们的任务。某种程度上说他不应该是*框架*而是一个*库*。

最后给出三个官方推荐的示例

<https://github.com/mitsuhiko/flask/tree/master/examples/flaskr/>

<https://github.com/mitsuhiko/flask/tree/master/examples/minitwit/>

<https://github.com/mitsuhiko/flask/tree/website>

资源推荐
----------
[python入门](http://pythontutor.com/)

[docker入门](https://www.docker.io/gettingstarted/#)

[golang入门](http://tour.golang.org/#1)


一段代码
--------
```bash
function scaner() {
   count=0
   for i in 192.168.0.{110..120};do
    ¦  ping -c 1 -W 1 $i &> /dev/null
    ¦  if [[ $? != 0 ]]; then
    ¦   ¦  echo "${i} 可用"
    ¦   ¦  #count=$((${count}+1))
    ¦   ¦  let count++
    ¦  fi
   done
   echo "共${count}个ip可用"
}
```

Tip
-------
#### 开发
shared_ptr的内存所有权使用计数器是非独占的，weak_ptr弱引用只引用不计数。

#### 运维
使用virtualenv可以更好的隔离python的版本依赖以便于部署与生产环境

#### 使用
emacs启动慢，通过hosts文件设置本机的机器名对应的ip即可


作者简介
--------
<a name="tj"></a>
![Photo](http://ssh.cnsworder.com/img/)  
网名：   
群ID: 九州  
微博：<http://>   
技术：  
简介：
- - -
欢迎群成员自荐自己的blog文章和收集的资源，发[邮件](mailto:cnsworder@gmail.com)给我，如果有意见或建议都可以mail我。  
如果无法直接在邮件内查看，请访问[github上的页面](https://github.com/cnsworder/publication/blob/master/alpha4.md)或[网站](http://ssh.cnsworder.com/alpha4.html)。  
如果查看历史订阅请到[readthedocs](http://linux.readthedocs.org/zh_CN/latest/)。  
我们在github上开放编辑希望大家能参与到其中。
