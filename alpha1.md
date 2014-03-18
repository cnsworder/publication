<link rel="stylesheet" href="http://ssh.cnsworder.com/styles/monokai_sublime.css" />
<script type="text/javascript" src="http://ssh.cnsworder.com/highlight.pack.js"></script>
<script type="text/javascript">
    hljs.initHighlightingOnLoad();
</script>

![GNU/Linux](https://www.kernel.org/theme/images/logos/tux.png)GNU/Linux Developer
======
kernel stable:3.12.5  
QQ群号：20506135  
本期编辑：江湖郎中


很荣幸的向各位Linux开发QQ群的成员宣布《GNU/Linux Developer》第**alpha1**期发布了！    
我曾怀着虔诚的心朝拜[GNU](http://www.gnu.org)神圣殿堂，它的光芒始终照耀着我前行。  
自由软件与开源软件是为了更好的尊重我们的劳动成果，消除版权所保护所不能适应的更迅速发展的软件行业的一种机制，通过适当的条款来保护合作与分享，这正在形成一种社会化开发的新趋势。[编程一小时](http://code.org/)更促使社会化开发的范围由[github](http://www.github.com)等code群体扩展为整个社会群体的行为，商业软件行为本身没有错误，只是原有的开发模式已经不适应未来的发展趋势。  
自由/开源不等于免费，参与你所使用的软件的开发，为你使用的软件贡献代码、捐赠或付费，尊重授权协议，尊重我们共同的劳动成果。  



本期专题：Archlinux安装
--------
![Archlinux](http://distrowatch.com/images/yvzhuwbpy/arch.png)  
作者：[__猫猫__](#成员推荐)  
#### [为了自由的名义](http://www.wangxiaomao.net/?p=734)  
> 在一个应该考虑晚上吃蹄花还是羊肉汤的时候，郎中在小窗口里说让我做第一期订阅的掌勺，我勒个去，亚历山大啊。不过，这样安排也是有一定道理的，按年龄来的呗……  
说起来其实真心的很仓促，一点准备都没有，想话题的时候，嗖的一声脑子都不知道去哪了。上次帮人赢键鼠套的时候，改ppt还看了好几天《人月神话》呢，这回只想了一个晚上，好吧，上述言论的中心思想是：这期不准拍砖，留着砖下期拍郎中去。
没啥好说的，那就说说Archlinux吧...
[更多](http://www.wangxiaomao.net/?p=734)

#### [ArchLinux安装手记](http://www.wangxiaomao.net/?p=521)
> 基本上来说呢，自己手动搞一下ArchLinux，会对Linux有一个更为深入的理解。这货没有Fedora或者Ubuntu那样方便的安装程序用于安装，而是所有的步骤和操作都需要自己动手完成...
[更多](http://www.wangxiaomao.net/?p=521)

#### [通过SSH远程安装Archlinux](http://www.wangxiaomao.net/?p=589)
> 其实在引导起来以后，已经是一个功能比较完备的简装系统了，所以为何不启动完成后就打开SSH，然后远程来安装。至少，在SSH客户端里面，原来要通过敲键盘输入的东西，可以直接ctrl+c、ctrl+v了...
[更多](http://www.wangxiaomao.net/?p=589)

#### [把Archlinux安装到U盘](http://www.wangxiaomao.net/?p=594)
> 最近突然发现手头的笔记本硬盘突然变缺稀了，主要是之前又一次手贱买了个装两块笔记本硬盘的raid盒子，后来又把IDE的移动硬盘弄坏了，于是就悲剧了，扯来扯去就剩一个SSD和一个给小猫做看动画用的移动硬盘了，于是俩笔记本都陷入了要悲剧的边缘。
后来想，1501这朵奇葩，既然电池啥的都已经烈士了，而且这货又有着可以拿去铺地的身材，所以干脆了，直接买了俩ScanDisk的小魔豆，插到USB上也几乎看不出来，然后装Archlinux到魔豆里，然后这货就放那滚着玩了...
[更多](http://www.wangxiaomao.net/?p=594)

#### [给Archlinux安装中文字体](http://www.wangxiaomao.net/?p=616)
> 起来xfce4，打开chrome，我勒个去，中文变成口口版了，突然很怀念当初做wm的ROM的时候，把字体删了……
[更多](http://www.wangxiaomao.net/?p=612)

#### [Archlinux自动连接wifi和自动启动sshd和自动连接无线网络](http://www.wangxiaomao.net/?p=616)
> 自动连接wifi，前提是已经生成好了无线配置文件，位置在/etc/netctl，比如wlan0-SSID(这里的SSID需要替换成你的真实的SSID名字)...
[更多](http://www.wangxiaomao.net/?p=616)

资源推荐
----------
#### [基于VBOX的各版本Linux OVA](http://www.wangxiaomao.net/?p=495)  
作者：[__猫猫__](#mm)
> VirtualBox的OVA镜像文件

#### [c/c++资源（源码、开发工具、开发库）](http://blog.csdn.net/cnsword/article/details/4176636)
作者：江湖郎中
> c/c++开发工具，编译器，调试分析，构建，开发库，源码  

群内问答
--------
#### **Q:**  _aio_在网络上的应用？
**[济南]江湖郎中:** ___nginx___使用了_aio_，它使用的方式是_epoll+libaio+eventfd_的方式 

#### **Q:**  lamp怎么安装
**[湘]Livenux:** aptitude install lamp 

#### **Q:**  Openwrt是什么
**[绵阳]空如也:** openwrt是一个基于Linux的OS，主要用于路由器 

一段代码
--------
```bash
#!/usr/bin/env bash
#获取ip更新并发送邮件
ip_log=ip.log
now_ip=$(curl ifconfig.me)
old_ip=$(cat $ip_log)
if [[ "$now_ip" != "$old_ip" ]]; then
      echo "$now_ip" > $ip_log
      mutt -s "Ip changed" xxx@gmail.com < ip.log
fi
```
成员推荐
--------
<a name="mm"></a>
![Photo](http://www.wangxiaomao.net/mdphoto.png)  
网名：猫猫  
群ID：[济南]猫猫  
主页：<http://www.wangxiaomao.net>  
技术：杂食的  
简介：别人以为我是只企鹅，可是我希望自己做只猫
- - -
欢迎群成员自荐自己的blog文章和收集的资源，发[邮件](mailto:cnsworder@gmail.com)给我，如果有意见或建议都可以mail我。  
如果无法直接在邮件内查看，请访问[github上的页面](https://github.com/cnsworder/publication/blob/master/alpha1.md)或[网站](http://ssh.cnsworder.com/)。  
我们在github上开放编辑希望大家能参与到其中。

