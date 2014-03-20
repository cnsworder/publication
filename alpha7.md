<link rel="stylesheet" href="http://ssh.cnsworder.com/styles/monokai_sublime.css" />
<script type="text/javascript" src="http://ssh.cnsworder.com/highlight.pack.js"></script>
<script type="text/javascript">
    hljs.initHighlightingOnLoad();
</script>

![Tux](http://ssh.cnsworder.com/img/tux.png)

GNU/Linux Developer
==============================================================  

**Aplha7**  
**kernel stable： 3.13**  
**QQ群号： 20506135**  
**微信号： linux_developer**  
**主编： 猫猫**  

《GNU/Linux Developer》第**Aplha7**期在春节前和大家见面了，本期将为大家带来专题**面向对象的C** 和 **泛化的C++**。  

专题分享
==============

面向对象的C
-------------------
**作者：江湖郎中**  


泛化的C++
---------------
**作者：江湖郎中** 

大家学golang
-------------- 


资源推荐
----------
《集体智慧编程》：该书完全使用简单易用的python语言描述，为入门者简直是揭开了一层朦胧的面纱。本人也是其中的受益者，所以有兴趣的可以先阅读本书。  
另外专题中用到的代码和讲解内容也是来自于此书。  
[pythonxy](https://code.google.com/p/pythonxy)：一个集成了很多科学计算工具的python版本。本专题的代码虽然都是自己实现，但是也可以通过scipy库中的一些封装好的函数库去实现。其实现更加合理科学。  
[pycharm](http://www.jetbrains.com/pycharm)：个人用过的觉得是最好的python IDE，或许，用多了会上瘾的感觉，（收费的商业版，当然也有社区版。。。怎么使用就看你们的方式了）  
[mahout](http://mahout.apache.org)：一款由java编写的机器学习的库，能够跟hadoop完美的融合，对于大数据的机器学习非常的好，在企业的具体应用中也开始在用了，至于为什么给大家推荐呢，  

不是因为作为一个代码库可以偷懒，我一直的原则都是，能够做得出的才去偷懒，不然就勤快点，主要是因为本期演示的数据非常的少，所以没有什么影响，但是真正应用中的话数据量是非常大的，试想下，如果以淘宝或者亚马逊的交易商品来做推荐，那么多数据，如果自己写代码一个个去跑，该跑到什么时候。。。

一段代码
--------
``` python
#!/usr/env python
import socket
from smtplib import *
from email import *
"""
   上一期，通过bash脚本借助curl获取ifconfig.me返回的地址并发送邮件，
   这一期我们用python实现借助dnspod来获取外网ip地址并发送邮件
"""
def get_ip():
    sock = socket.create_connection(('ns1.dnspod.net', 6666))
    ip = sock.recv(16)
    sock.close()
    return ip
 
def send_mail():
   s = SMTP()
   s.connect("smtp.xxx.com")
   s.login("xx@xx.com", "xx")
   msg = mime.Multipart.MIMEMultipart()
   msg['Subject'] = u"RaspberryPi IP"
   msg['From'] = "xx@xx.com"
   msg['To'] = 'xx@xx.com'
   text = "Your home IP: " + get_ip()
   msg.attach(mime.Text.MIMEText(text, "plain", "utf-8"))
   se = s.sendmail("xx@xx.com", ['xx@xx.com'], msg.as_string())
   s.quit()
```

开源吉祥物
--------
![beasties](http://ssh.cnsworder.com/img/daemon-tux-hexley.png)  
FreeBSD: Beastie  
Linux: Tux  
darwin: Hexley

Tip
-------
#### 开发
> read、write默认是不带缓冲的  
> fread、fwrite默认是带缓冲的  

   

> `int fileno(FILE *stream)`可以将文件指针转换成文件描述符  
> `FILE *fdopen(int fd, const char *mode)`将文件描述符转换成文件指针  

#### 运维
> tmux和screen可以在远程断开后继续运行

#### 使用
> `fedup --network 20` 将fedora升级到最新的20


作者简介
--------
<a name="tj"></a>
![Photo](http://ssh.cnsworder.com/img/weiyi.jpg)  
网名： 唯一<br/>
群ID: [广州]唯一   
微博：<http://www.weibo.com/sadlin>  
技术：java、搜索引擎   
简介：广州小小程序员。喜欢折腾代码。。  
- - -
欢迎群成员自荐自己的blog文章和收集的资源，发[邮件](mailto:cnsworder@gmail.com)给我，如果有意见或建议都可以mail我。  
如果无法直接在邮件内查看，请访问[github上的页面](https://github.com/cnsworder/publication/blob/master/alpha2.md)或[网站](http://ssh.cnsworder.com/alpha2.html)。  
我们在github上开放编辑希望大家能参与到其中。
