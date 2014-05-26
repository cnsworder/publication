<link rel="stylesheet" href="http://docs.cnsworder.com/styles/monokai_sublime.css" />
<script type="text/javascript" src="http://docs.cnsworder.com/highlight.pack.js"></script>
<script type="text/javascript">
    hljs.initHighlightingOnLoad();
</script>

GNU/Linux Developer
==============================================================  

**Version: Aplha6**  
**QQ群号： 20506135**  
**微信号： linux_developer**  
**主编: 猫猫**  

《GNU/Linux Developer》第**Aplha6**期和大家见面了，本期 *我们* 将会给大家带来 `git` 的相关专题。  

git 入门
------------------------------------------------


git gui
---------------------------

使用 git 进行项目管理
--------------------------------------------
**作者: 江湖郎中**


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

Next
-----------------
<video src="http://docs.cnsworder.com/img/evil.mp4"></video>

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
