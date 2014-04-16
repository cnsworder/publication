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

《GNU/Linux Developer》第**Aplha5**期和大家见面了，本期*我*将为大家带来专题**Linux init系统介绍**。  


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

```bash
start on fedora.serial-console-available DEV=* and stopped rc RUNLEVEL=[2345]
stop on runlevel [S016]

instance $DEV
respawn
pre-start exec /sbin/securetty $DEV
exec /sbin/agetty /dev/$DEV $SPEED vt100-nav
post-stop exec /sbin/initctl emit --no-wait fedora.serial-console-available DEV=$DEV SPEED=$SPEED
usage 'DEV=ttySX SPEED=Y  - where X is console id and Y is baud rate'
``

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

资源推荐
----------
[iOS安全攻防](http://blog.csdn.net/column/details/hackingios.html): 美女程序员吴茜的大作，分析iOS数据窃取与防范的好文章。  
[Android安全攻防](http://blog.csdn.net/yiyaaixuexi/article/category/1302842): 美女的另一大作，介绍android安全机制SEAndroid。
一段代码
--------


Tip
-------
#### 开发

> 
>

#### 运维
> 

#### 使用
> 


作者简介
--------
<a name="tj"></a>
![Photo](http://www.gravatar.com/avatar/c1991331b3e8139f3168fdaf71cb65c4.png)  
网名: cnsworder/crossword<br/>
群ID: [济南]江湖郎中   
网站: <http://www.cnsworder.com>  
blog: <http://blog.csdn.net/cnsword>  
微博: <http://www.weibo.com/cnsworder>  
技术:    
简介:   
- - -
欢迎群成员自荐自己的blog文章和收集的资源，发[邮件](mailto:cnsworder@gmail.com)给我，如果有意见或建议都可以mail我。  
如果无法直接在邮件内查看，请访问[github上的页面](https://github.com/cnsworder/publication/blob/master/alpha5.md)或[网站](http://docs.cnsworder.com)。  
我们在github上开放编辑希望大家能参与到其中。
