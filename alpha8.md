
专题  web.py学习(二)    
======================

*作者: ownone*

web.py和通用概念
----------------
关于web.py解析上一次，和大家一块分析了web.py主体(httpserver)的调用流程。要仔细分析他们的调用流程，这是一件比较困难的事情。
因为web.py为了方便大家的扩展，使用了很多近似的方法名称，如果你要用眼睛上去查看他们的调用，你非常可能被带入迷糊的境地。会有许多让你疑惑的内容我自己使用的方法是在web.py中添加了大量的调试信息。

看了郎中撰写的flask的东东，我觉得我有点太钻牛角尖，非常想把web.py弄明白，但是并没有把web.py用好，这一次我的希望这次的web.py可以作为web.py三部曲（核心逻辑，如何使用，写一个凑合的web程序）的承上启下的内容。

web.py里边包含着几个通用的概念HTTPServer，HTTPRequest，HTTPConnection，GateWay.这些概念在所有的server都会出现。

* HTTPServer是web.py的大管家，创建套接字，建立线程池，处理http header，返回http状态和页面。
* HTTPConnection在服务器上，意味着client，他就是client的代表。web.py非常形象的使用了一个方法communicate，体现了两者之间的关系。
* HTTPRequest是client和server交互的实际的内容，非常直接的request，response。
* GateWay 服务器和开发人员的面板。通过gateway，开发人员实现的逻辑，才能与客户端进行交流。

好了，在这里对web.py的httpserver部分做了一个总结。

hello world
------------
下边我将要更多的介绍web.py了。让大家怎么开始使用web.py，好了从臭名昭著的helloworld开始吧。
```
    #-*- coding:utf-8 -*-
    import web
         
    #定义url，将地址映射到对应的类
    urls = (
        "/", "index",
    )
    app = web.application(urls, globals())
    #定义index类
    class index:
        #get请求
        def GET(self):
            return "Hello World"
 
    if __name__ == "__main__":
        app.run()
```

请将这段代码保存到app.py里边，运行python app.py (你必须已经安装了web.py),用浏览器访问http://localhost:8080，如果看到Hello World，那么恭喜你你已经成功运行了网站。你的代码已经提供了http服务。

这个程序是我们的开端，所有的内容都在这个基础上延伸。让我们好好认识这个开端。
路由表和服务器容器
--------
urls --路由表，定义了客户端请求的url，可以使用正则表达式，web.py会为我们只能得匹配到合适的类。这里都是逗号，两两配对。

注意：url匹配只匹配url路径不包括参数，例如：
'/news/create?title=(.+)'
其中起作用的url是/news/create

web.application是我们web服务器的容器，我们把想要的url和对应的逻辑实现以后，它就忠实的为我们工作（因为web.application可见，让我觉得web.py像lib多个framework）
这些自定义的类，有固定的GET，POST方法，以应对get和post请求。实现逻辑，返回给客户端。
web.py使用了类来写视图，这是一个非常赞的设计，这样我们可以通过定义基类来实现很多功能，例如在视图开始前自动检查用户权限，将一些常用的方法写成基类方法，就能很方便的进行调用，甚至在一些特殊需求下，可以通过一个类视图，来衍生出很多页面，既提高了开发速度，也提高了可维护性。

这很好，但是还有个疑问，我写一个helloword还行，让我写一个正常的页面，让我用一个字符串全return出来，这也太为难了，别着急。下边就是我们的模板。
 
web.py模板
----------
    
这个太重要了，让我想起的就是当初开始学习j2ee的时候，编写jsp，在webpage中编写<%Java代码%>。话说这个是一个伟大的历史的进步不是吗？不用什么都是print("")

web.py在这个地方，有太多的选择，mako，genshi，jinja2.还有它自身的Templetor 。
Templetor是web.py的模板语言，它能负责将 python 的强大功能传递给模板系统。 在模板中没有重新设计语法，它是类 python 的。
在app.py同级目录建立文件夹templ,上创建hello.html
内容为

```
    $def with (name) 
    Hello $name!
```

将app.py中index类更新为

```
class index:
    #get请求
    def GET(self):
        render = web.template.render('templates')
        return render.hello('world')
```

再次运行python app.py，hello word！又出现了。

Templator非常强大，支持简易的语法可以实现

* 定义变量，赋值
```
    $ bug = get_bug(id)
    <h1>$bug.title</h1>
    <div>
        $bug.description
    <div>
```
* 过滤
```
$:form.render()
```
    
* 转义符
```
    5$$       $#我显示的是5dollar   ```

* 注释
```
$# this is a comment
hello $name.title()! $# display the name in title case
```

* 控制结构
```python
$while a:
    hello $a.pop()
$if times > max: 
    Stop! In the name of love. 
$else: 
    Keep on, you can do it.
```
* 内置 和 全局

   像 python 的任何函数一样，模板系统同样可以使用内置以及局部参数。很多内置的公共方法像 range，min，max等，以及布尔值 True 和 False，在模板中都是可用的。部分内置和全局对象也可以使用在模板中。
全局对象可以使用参数方式传给模板，使用 web.template.render：
```
    import web
    import markdown

    globals = {'markdown': markdown.markdown}
    render = web.template.render('templates', globals=globals)
```
内置方法是否可以在模板中也是可以被控制的,为了安全性需要去掉内置的方法：
```
    # 禁用所有内置方法
    render = web.template.render('templates', builtins={})
```
通过模板的这些定义，web.py具有了非常强大的页面表现能力。
最后说一下，豆瓣后台部分就是使用web.py，不过模板系统使用的是mako。可见web.py是非常具有实用性的。 


资源推荐
----------

[酷壳](http://coolshell.cn/)    陈皓哥为大家推荐的技术文档，内容很丰富。

一段代码
--------
```python
    a = ['1', '2', '3', '4']
    b = ['5', '6', '7', '8']
    s = '1234'
    print ''.join(dict(zip(a, b)).get(c) for c in s)
    #处理批量代换大代码
```

Tip
-------
#### 开发
   python的编码问题是一个老大难的问题，所以提议
   * 使用字符编码声明，并且同一工程中的所有源代码文件使用相同的字符编码声明。
   * 抛弃str，全部使用unicode

    python和我们的os交互的时候，使用unicode会自动使用locale的方言，我们就不用为了系统使用什么方言而恼火。

#### 运维
使用logrotate管理日志，通过logrotate来将日志截断，打包，方便以后查看。

#### 使用
None

作者简介
--------
<a name="tj"></a>
![Photo](http://ssh.cnsworder.com/img/ownone.jpg)  
网名： ownone  
群ID: [北京]Num1*  
微博：<http://t.qq.com/ownone_vip>   
技术：C、python、linux、web、网络编程   
简介：北京程序员一枚，带一点开源的浪漫主义情怀，带一点悲观主义（开源在中国的商业化）。努力工作。  
- - -
欢迎群成员自荐自己的blog文章和收集的资源，发[邮件](mailto:cnsworder@gmail.com)给我，如果有意见或建议都可以mail我。  
如果无法直接在邮件内查看，请访问[github上的页面](https://github.com/cnsworder/publication/blob/master/alpha3.md)或[网站](http://ssh.cnsworder.com/alpha3.html)。  
如果查看历史订阅请到[readthedocs](http://linux.readthedocs.org/zh_CN/latest/)。  
我们在github上开放编辑希望大家能参与到其中。
