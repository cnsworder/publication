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

《GNU/Linux Developer》第Alpha4期在春风中来了了，本期**九州**有**《》**专题和大家分享，另外由于*ownone*工作太忙了**web.py**的专题将会在4月份继续和大家见面，这期**cnsworder**会给大家带来**flask**的专题分享。     

前言
====


专题一 
====================
**作者: **



专题二  flask——KISS之美   
======================
**作者: cnsworder**

这个月ownone由于工作原因无法按期与大家分享**web.py**的内容了，我在想找一个相当量级的内容与大家分享，**Django**太笨重了，**tornado**重点在IO，还是**flask**和**bottle**合适，个人对**flask** 稍有些了解，属于严重*入门级别*，所以打肿脸充胖子来和大家分享一下。

flask是什么？当然他不是flash,官网给出的说明：
> Flask is a microframework for Python based on Werkzeug, Jinja 2 and good intentions. And before you ask: It's BSD licensed!

flask主要

flask的最新版本是0.10。

上官网示例：

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


资源推荐
----------


一段代码
--------
```
```

Tip
-------
#### 开发


#### 运维


#### 使用



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
