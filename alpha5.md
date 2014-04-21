<link rel="stylesheet" href="http://docs.cnsworder.com/styles/monokai_sublime.css" />
<script type="text/javascript" src="http://docs.cnsworder.com/highlight.pack.js"></script>
<script type="text/javascript">
    hljs.initHighlightingOnLoad();
</script>

GNU/Linux Developer
==============================================================  

**Version: Aplha5**  
**QQ群号： 20506135**  
**微信号： linux_developer**  
**主编: 猫猫**  
**本期编辑： 江湖郎中**  

《GNU/Linux Developer》第**Aplha5**期和大家见面了，本期 *我* 将为大家带来专题 **Linux init系统介绍** 和 **flask--kiss之美** ， ownone 将继续 **web.py** 之旅。  


本期专题：Linux init系统介绍
------------------------------------------------
**作者：[江湖郎中](#tj)**  

我手上的版本有archlinux、fedora20、debian7、centos6我主要以以上这些版本为例来描述，BSD init以上版本默认都没有了，所以无法验证，描述很可能有漏洞。其中archlinux、fedora20使用systemd，debian7使用system V init，centOS6使用upstart。 

在谈init之前先说一下linux kernel的启动过程，在PC上和arm嵌入式开发板上会有所不同。

##### 系统启动

+ PC
设备在上电以后会在指定的位置来运行某段代码，这个位置`0xFFFF0`就是固化在主板上的BIOS（现在是UEFI了），BOIS自检后会从磁盘的某个位置加载程序，如果mbr分区会运行位于磁盘0道0柱1扇区上512字节的mbr程序，mbr程序会通过446+1字节的位置读取64字节的分区表，并在boot标志的分区上加载bootload，一般会使grub、lilo。UEFI引导的情况下，UEFI会直接到GPT分区的fat文件系统下找到efi的引导文件并加载，这个文件会加载grub或者其他引导（lilo是否支持有待验证）。grub本身是一个缩减版的内核，grub本身会包含stage1,stage1.5，stage2 三步。它通过kernel指令加载kernel(grub2是linux指令) vmlinuz文件并传递给内核启动参数，通过initrd指令加载ram disk文件，然后内核就被加载进内存中运行了。
efi启动模式下的boot目录如下图：  
![boot](http://docs.cnsworder.com/publication/image/boot_2.png)  
bios启动模式下的boot目录如下图：  
![boot](http://docs.cnsworder.com/publication/image/boot_5.png)  
vimlinuz-linux是kernel指令加载的kernel文件，initramfs-linux.publication/image是initrd指令加载的ramfs文件。
efi目录如下图：  
![efi](http://docs.cnsworder.com/publication/image/boot_3.png)  
efi目录是挂载的一个fat文件系统的分区。包括了可以被UEFI加载的多个efi文件。
grub加载的配置如下图:  
![grub](http://docs.cnsworder.com/publication/image/boot_4.png)

+ 开发板
开发板上电后会从norflash或者nandflash上的某个位置来读取bootload，然后由bootload加载内核到内存，内核直接开始运行。在3.0以后的arm linux上kernel会从flash的某个位置读取LDS来加载板载资源的配置信息。  

在内核初始化完成后，需要和根文件系统建立关联，pc版的kernel会借助initrd来做桥梁加载根文件系统，而armlinux则直接根据配置加载跟文件系统。  

##### 早期init 系统简介
在内核所有的操作完成以后就会从根文件系统中加载地一个要执行的程序，他是所有程序的父进程，也是我们今天的主角**init**。恩，前面的废话太多了，现在才进入正题...

init的历史很久远，早期Linux使用的init有两个版本：`sysV`和`BSD`。现在他们仍旧没有完全被废弃，现在还有不少发行版本采用这两个系统比如dibian还采用system V init,archlinux在20jenk12年前后才放弃BSD init切换到systemd。废话不多说，分别描述一下。

+ BSD
       - 使用/etc目录下以rc.x作为文件名的文件来描述init的操作
       - rc.sysinit
       - rc.single单用户执行
       - rc.multi2～5执行
       - rc.local是杂项
       - init系统会按照以上顺序加载运行
       - rc.conf包含了相关的配置

+ system V
       - 脚本文件目录在`/etc/init.d/`
       - `/etc/rc{runlevel}.d`目录标识相应运行级别的目录
       - 目录下的文件以S开头的是启动的服务，以K开头的是不启动的服务
       - 启动标识后紧跟的数字表示启动优先级，数值越小运行越早
       - 通过service命令来起停服务
       - 通过chkconf来管理服务
       - 通过`/etc/inittab`文件来配置相应的运行级别

以上两种init系统加载的文件本身都是一些脚本文件，通过这些脚本可以加载相应的模块或者启动需要的程序。 
BSD init 具体服务的内容:
```bash
#!/bin/sh
. /etc/rc.subr
name="dummy"
start_cmd="${name}_start"
stop_cmd=":"

dummy_start()
{
    echo "Nothing started."
}

load_rc_config $name
run_rc_command "$1"
```

SystemV init具体服务的配置内容：
```bash
#! /bin/sh

### BEGIN INIT INFO
# Provides:          sudo
# Required-Start:    $local_fs $remote_fs
# Required-Stop:
# X-Start-Before:    rmnologin
# Default-Start:     2 3 4 5
# Default-Stop:
# Short-Description: Provide limited super user privileges to specific users
# Description: Provide limited super user privileges to specific users.
### END INIT INFO

N=/etc/init.d/sudo

set -e

case "$1" in
  start)
        # make sure privileges don't persist across reboots
        if [ -d /var/lib/sudo ]
        then
                find /var/lib/sudo -exec touch -t 198501010000 '{}' \;
        fi
        ;;
  stop|reload|restart|force-reload|status)
        ;;
  *)
        echo "Usage: $N {start|stop|restart|force-reload|status}" >&2
        exit 1
        ;;
esac

exit 0
```

从语法角度来看两者没有什么区别。

早期的init系统存在若干问题，比如，无法并行任务，任务之间缺乏有效的通信机制，对进程的监控只能通过PID来进行...

##### 现代init系统简介

systemd和upstart在这种情况下产生了，他们都是事件驱动的，不得不说的是由于systemd开始时间晚于upstart所以很多特性也借鉴了upstart，同时systemd还借鉴了Mac OS X的Launcher系统。

+ systemd  
>  systemd 是 Linux 下的一款系统和服务管理器，兼容 SysV 和 LSB 的启动脚本。systemd 的特性有：支持并行化任务；同时采用 socket 式与 D-Bus 总线式激活服务；按需启动守护进程（daemon）；利用 Linux 的 cgroups 监视进程；支持快照和系统恢复；维护挂载点和自动挂载点；各服务间基于依赖关系进行精密控制。  
                  --维基百科

systemctl来管理systemd，同时也兼容service命令
所有的systemd配置在`/usr/lib/systemd`目录下,当启动用后会被链接或者拷贝到/etc/systemd目录下

+ upstart
  - 使用initctl来管理upstart
  - 配置项在`/etc/init/`目录下
  - ubuntu中在`/etc/rc.conf`的最后一行通过`exec /etc/init.d/rc $RUNLEVEL`来启动相应级别系统，debina中是在`/etc/inittab`中添加`id:2:initdefault:`

  *我印象中ubuntu的upstart的配置和centos的配置方法是有区别的，这里无法进一步验证。*


##### systemd和upstart的使用

先分别上一段代码,sytemd：

```conf
[Unit]
Description=OpenSSH Daemon
Wants=sshdgenkeys.service
After=sshdgenkeys.service
After=network.target

[Service]
ExecStart=/usr/bin/sshd -D
ExecReload=/bin/kill -HUP $MAINPID
KillMode=process
Restart=always

[Install]
WantedBy=multi-user.target

```

upstart:

``` bash
start on fedora.serial-console-available DEV=* and stopped rc RUNLEVEL=[2345]
stop on runlevel [S016]

instance $DEV
respawn
pre-start exec /sbin/securetty $DEV
exec /sbin/agetty /dev/$DEV $SPEED vt100-nav
post-stop exec /sbin/initctl emit --no-wait fedora.serial-console-available DEV=$DEV SPEED=$SPEED
usage 'DEV=ttySX SPEED=Y  - where X is console id and Y is baud rate'
```

看出什么区别来了吗？

upstart和systemd是全新的方式，upstart是命令的方式，systemd则是conf形式。

###### systemd
+ `Unit`用来描述服务的相关信息和依赖关系
+ `Service`对服务本身进行描述
+ `Install`说明他的运行环境
+ 运行级别也通过systemd来进行管理

###### upstart
+ 通过`start on runlevel [012345]`和`stop on runlevel [!RUNLEVEL]`来制定具体运行级别
+ 通过`exec`来运行相应的程序
+ 通过`script ... end script`指令可以直接嵌入脚本

一些更加细节的配置请查阅相关手册吧。

我们可以通过`--init=xxx`内核参数来指定所使用的init系统。当然指定的这个init可以是任意的程序，你完全可以直接指定成为你想要的任何程序。

目前大部分Linux发行版本都已经采用systemd作为默认的init系统，ubuntu现在使用的是upstart系统，而Debain也在投票的结果是选择了systemd，ubuntu也宣布会接受debian的上游选择决定。

貌似现在systemd有统一Linux世界init系统的趋势。

专题  web.py学习(二)    
---------------------------

**作者: ownone**

### web.py和通用概念

关于web.py解析上一次，和大家一块分析了web.py主体(httpserver)的调用流程。要仔细分析他们的调用流程，这是一件比较困难的事情。
因为web.py为了方便大家的扩展，使用了很多近似的方法名称，如果你要用眼睛上去查看他们的调用，你非常可能被带入迷糊的境地。会有许多让你疑惑的内容我自己使用的方法是在web.py中添加了大量的调试信息。

看了郎中撰写的flask的东东，我觉得我有点太钻牛角尖，非常想把web.py弄明白，但是并没有把web.py用好，这一次我的希望这次的web.py可以作为web.py三部曲（核心逻辑，如何使用，写一个凑合的web程序）的承上启下的内容。

web.py里边包含着几个通用的概念HTTPServer，HTTPRequest，HTTPConnection，GateWay.这些概念在所有的server都会出现。

* HTTPServer是web.py的大管家，创建套接字，建立线程池，处理http header，返回http状态和页面。
* HTTPConnection在服务器上，意味着client，他就是client的代表。web.py非常形象的使用了一个方法communicate，体现了两者之间的关系。
* HTTPRequest是client和server交互的实际的内容，非常直接的request，response。
* GateWay 服务器和开发人员的面板。通过gateway，开发人员实现的逻辑，才能与客户端进行交流。

好了，在这里对web.py的httpserver部分做了一个总结。

### hello world

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

### 路由表和服务器容器

urls --路由表，定义了客户端请求的url，可以使用正则表达式，web.py会为我们只能得匹配到合适的类。这里都是逗号，两两配对。

注意：url匹配只匹配url路径不包括参数，例如：
'/news/create?title=(.+)'
其中起作用的url是/news/create

web.application是我们web服务器的容器，我们把想要的url和对应的逻辑实现以后，它就忠实的为我们工作（因为web.application可见，让我觉得web.py像lib多个framework）
这些自定义的类，有固定的GET，POST方法，以应对get和post请求。实现逻辑，返回给客户端。
web.py使用了类来写视图，这是一个非常赞的设计，这样我们可以通过定义基类来实现很多功能，例如在视图开始前自动检查用户权限，将一些常用的方法写成基类方法，就能很方便的进行调用，甚至在一些特殊需求下，可以通过一个类视图，来衍生出很多页面，既提高了开发速度，也提高了可维护性。

这很好，但是还有个疑问，我写一个helloword还行，让我写一个正常的页面，让我用一个字符串全return出来，这也太为难了，别着急。下边就是我们的模板。
 
### web.py模板
    
这个太重要了，让我想起的就是当初开始学习j2ee的时候，编写jsp，在webpage中编写<%Java代码%>。话说这个是一个伟大的历史的进步不是吗？不用什么都是print("")

web.py在这个地方，有太多的选择，mako，genshi，jinja2.还有它自身的Templetor 。
Templetor是web.py的模板语言，它能负责将 python 的强大功能传递给模板系统。 在模板中没有重新设计语法，它是类 python 的。
在app.py同级目录建立文件夹templ,上创建hello.html
内容为

``` python
    $def with (name) 
    Hello $name!
```

将app.py中index类更新为

``` python
class index:
    #get请求
    def GET(self):
        render = web.template.render('templates')
        return render.hello('world')
```

再次运行python app.py，hello word！又出现了。

Templator非常强大，支持简易的语法可以实现

* 定义变量，赋值

``` python 
    $ bug = get_bug(id)
    <h1>$bug.title</h1>
    <div>
        $bug.description
    <div>
```

* 过滤

``` python
$:form.render()
```
    
* 转义符

``` python
    5$$       $#我显示的是5dollar   ```

* 注释

``` python 
$# this is a comment
hello $name.title()! $# display the name in title case
```

* 控制结构

``` python
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

``` python
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



flask——KISS之美   
--------------------------------------------
**作者: 江湖郎中**

ownone与大家分享**web.py**的内容了，我在想找一个相当量级的内容与大家分享，**Django**太笨重了，**tornado**重点在IO，还是**flask**和**bottle**合适，个人对**flask** 稍有些了解，属于严重*入门级别*，打肿脸充胖子来和大家分享一下。

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
  module: run.py
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

资源推荐
----------
[酷壳](http://coolshell.cn/)    陈皓哥为大家推荐的技术文档，内容很丰富。

[iOS安全攻防](http://blog.csdn.net/column/details/hackingios.html): 美女程序员吴茜的大作，分析iOS数据窃取与防范的好文章。 

[Android安全攻防](http://blog.csdn.net/yiyaaixuexi/article/category/1302842): 美女的另一大作，介绍android安全机制SEAndroid。

一段代码
--------

``` python
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
> 使用logrotate管理日志，通过logrotate来将日志截断，打包，方便以后查看。

#### 使用
> vim `:!%xxd` 查看二进制

作者简介
---------------
<a name="tj"></a>
![Photo](http://www.gravatar.com/avatar/c1991331b3e8139f3168fdaf71cb65c4.png)  
网名: cnsworder/crossword<br/>
群ID: [济南]江湖郎中   
网站: <http://www.cnsworder.com>  
blog: <http://blog.csdn.net/cnsword>  
微博: <http://www.weibo.com/cnsworder>  
技术: Linux C/C++ Python Golang    
简介: 专注于Linux智能设备与云的开发  

- - - -
欢迎群成员自荐自己的blog文章和收集的资源，发[邮件](mailto:cnsworder@gmail.com)给我，如果有意见或建议都可以mail我。  
如果无法直接在邮件内查看，请访问[github上的页面](https://github.com/cnsworder/publication/blob/master/alpha5.md)或[网站](http://docs.cnsworder.com)。  
我们在github上开放编辑希望大家能参与到其中。
