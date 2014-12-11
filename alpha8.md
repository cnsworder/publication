<link rel="stylesheet" href="http://docs.cnsworder.com/styles/monokai_sublime.css" />
<script type="text/javascript" src="http://docs.cnsworder.com/highlight.pack.js"></script>
<script type="text/javascript">
    hljs.initHighlightingOnLoad();
</script>

GNU/Linux Developer
==============================================================  

**Version: Aplha8**  
**QQ群号： 20506135**  
**微信号： linux_developer**  
**主编: 猫猫**  
**本期编辑： 江湖郎中**  

抱歉各位，我们的订阅中断了这么长时间。大家工作都很忙，没有太多时间去写些东西来和大家分享。好了，年底了，又有时间了，来和大家一块分享经验了。

9月份 **自由软件日** 活动时，*猫猫* 要求我将分享的 《vim、emacs双修之路》 内容详细的写出来分享给大家，找个借口是工作太忙没有时间写，但是真不敢提笔的真实原因是个人 `vim` `emacs` 的修为实是不足，也刚刚是日常使用而已。但是实在抵挡不了 *猫猫* 的多次催稿，只能写下些东西。

缘起
-----------
当然我也会使用一些其他的编辑器比如sublime，有时也会使用一下IDE，比如pycharm，qtcreator等。然而占据我主要时间的仍旧是vim和emacs。为什么会这样呢？加入有这样的场景，我gcc编译失败，查看输出信息是xx.cpp的第n行出现一个单词拼写错误。如果使用IDE去修改这一个错误结果是打开IDE的时间比我要修改错误的时间要多的多。如果我使用记事本这样的工具，我需要右键打开文件选择记事本打开，然后拖动滚动条到错误行，拖动的时间要看你的运气了，然后鼠标或者方向键选择错误修改。而我使用 `vim xx.cpp +n`打开文件直接定位到错误行，然后 `f` 到错误位置，cw修改错误单词，`:wq` 退出。我的修改已经完成并开始编译，你的eclipse或者VS是否已经打开？


修习vim
------------
第一次使用vim绝对是无奈的，因为安装的linux上只有vi，并且我当时真的是没有找到 `输入` 和 `退出` 的方法。  

多方搜寻终于知道输入要先按 `i`，退出的方法是 `:q` 或者 `ZZ`。现在已经可以使用vim了。

### 激活 vim
到目前为止vim仅仅是一个编辑器，并且没有记事本好用。传说中的 `编辑器之神` 你在何方？

### 神器 vim
vim通过插件插件将会从 `一个超强的编辑器` 变为 `编辑器之神`

### vi的三种经典模式

+ 普通模式
+ 插入模式
+ 命令模式

另外还有 增强命令模式，可视化模式，选择模式。

vi的思想就是在某种模式下去做特定的事情，比如我们在插入模式下就是去输入我们的文字，不要管修改的问题。当我们需要修改时切换到普通模式下，快定位到修改位置快速修改。

三种常用的模式中，如果你不是长期处于普通模式下，那你要思考你的工作状态了。我们70%的时间都是在修改代码而不是去写代码。

### 总结
vi不是一个编辑器，而是一种编辑模式，它已经被大量的IDE所认可并集成到其中成为内置的一种编辑模式。
### vi终极秘籍

+ `pacman -R emacs`
+ `yum remove emacs`
+ `apt-get remove emacs`

修习emacs
--------------
我们先来一个暗黑科技，在emacs中安装 *evil* ，使emacs支持vi的三种模式和基本的快捷键，通过 *evil-leader* 使其支持vi的leadermap。

### 伪装成编辑器的操作系统
历史上曾经有一个操作系统是用lisp写的，当然它不是emacs，并且它早已经被所有人所遗忘。但是emacs不会，它太善于伪装自己，在编辑器的背后，你可以发邮件，刷微博，写wiki，看日历，打俄罗斯方块。终于有一天你会进行下面的操作:

> `emacs --daemon`
> `alias e="emacsclient -t"`
> `alias em="emacsclient -c"`

好吧，你赢了，emacs作为daemon加到服务中，随系统一起启动，和系统一起关闭。

### 不会有第二个同样的emacs
快捷键绑定

### emacs 终极秘籍

+ `pacman -R vim`
+ `yum remove vim`
+ `apt-get remove vim`

为什么会双修
----------------
当然我没有机会使用vim或者emacs的终极秘籍。我作为异教徒和骑墙派，在工作中都会用到他们。

### vim使用的场景

+ 远程ssh
+ 快速修改文件
+ rst文档
+ 环境与emacs快捷键冲突的
+ 新安装的系统
+ 手机（vimTouch）
+ emacs不能做好的
+ 没有emacs

### emacs使用场景

+ 工程项目
+ markdown文档
+ gdb调试
+ tty中需要输入中文的
+ woman
+ 环境与vim快捷键冲突的
+ vim不能做好的
+ 没有vim

- - -
欢迎群成员自荐自己的blog文章和收集的资源，发[邮件](mailto:cnsworder@gmail.com)给我，如果有意见或建议都可以mail我。  
如果无法直接在邮件内查看，请访问[github上的页面](https://github.com/cnsworder/publication/blob/master/alpha8.md)或[网站](http://ssh.cnsworder.com/alpha8.html)。  
如果查看历史订阅请到[readthedocs](http://linux.readthedocs.org/zh_CN/latest/)。  
我们在github上开放编辑希望大家能参与到其中。
