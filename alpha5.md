<link rel="stylesheet" href="http://ssh.cnsworder.com/styles/monokai_sublime.css" />
<script type="text/javascript" src="http://ssh.cnsworder.com/highlight.pack.js"></script>
<script type="text/javascript">
    hljs.initHighlightingOnLoad();
</script>

![Tux](http://ssh.cnsworder.com/img/tux.png)

GNU/Linux Developer
==============================================================  

**Version: Aplha5**  
**kernel stable： 3.13**  
**QQ群号： 20506135**  
**微信号： linux_developer**  
**本期编辑： 江湖郎中**  

《GNU/Linux Developer》第**Aplha5**期和大家见面了，本期*我*将为大家带来专题**Linux init系统介绍**。  


本期专题：Linux init系统介绍
-----------
**作者：[江湖郎中](#tj)**  

我手上的版本有archlinux、fedora20、debian7、centos6我主要以以上这些版本为例来描述，BSD init以上版本默认都没有了，所以无法验证，描述很可能有漏洞。其中archlinux、fedora20使用systemd，debian7使用system V init，centOS6使用upstart。 

在谈init之前先说一下linux kernel的启动过程，在PC上和arm嵌入式开发板上会有所不同。

##### 系统启动

+ PC
设备在上电以后会在指定的位置来运行某段代码，这个位置`0xFFFF0`就是固化在主板上的BIOS（现在是UEFI了），BOIS自检后会从磁盘的某个位置加载程序，如果mbr分区会运行位于磁盘0道0柱1扇区上512字节的mbr程序，mbr程序会通过446+1字节的位置读取64字节的分区表，并在boot标志的分区上加载bootload，一般会使grub、lilo。UEFI引导的情况下，UEFI会直接到GPT分区的fat文件系统下找到efi的引导文件并加载，这个文件会加载grub或者其他引导（lilo是否支持有待验证）。grub本身是一个缩减版的内核，grub本身会包含stage1,stage1.5，stage2 三步。它通过kernel指令加载kernel(grub2是linux指令) vmlinuz文件并传递给内核启动参数，通过initrd指令加载ram disk文件，然后内核就被加载进内存中运行了。
efi启动模式下的boot目录如下图：  
![boot](http://ssh.cnsworder.com/img/boot_2.png)  
bios启动模式下的boot目录如下图：  
![boot](http://ssh.cnsworder.com/img/boot_5.png)  
vimlinuz-linux是kernel指令加载的kernel文件，initramfs-linux.img是initrd指令加载的ramfs文件。
efi目录如下图：  
![efi](http://ssh.cnsworder.com/img/boot_3.png)  
efi目录是挂载的一个fat文件系统的分区。包括了可以被UEFI加载的多个efi文件。
grub加载的配置如下图:  
![grub](http://ssh.cnsworder.com/img/boot_4.png)

+ 开发板
开发板上电后会从norflash或者nandflash上的某个位置来读取bootload，然后由bootload加载内核到内存，内核直接开始运行。在3.0以后的arm linux上kernel会从flash的某个位置读取LDS来加载板载资源的配置信息。  

在内核初始化完成后，需要和根文件系统建立关联，pc版的kernel会借助initrd来做桥梁加载根文件系统，而armlinux则直接根据配置加载跟文件系统。  

##### 早期init 系统简介
在内核所有的操作完成以后就会从根文件系统中加载地一个要执行的程序，他是所有程序的父进程，也是我们今天的主角**init**。恩，前面的废话太多了，现在才进入正题...

init的历史很久远，早期Linux使用的init有两个版本：`sysV`和`BSD`。现在他们仍旧没有完全被废弃，现在还有不少发行版本采用这两个系统比如dibian还采用system V init,archlinux在12年前后才放弃BSD init切换到systemd。废话不多说，分别描述一下。

+ BSD
       - 使用/etc目录下以rc.x作为文件名的文件来描述init的操作
       - rc.sysinit
       - rc.single单用户执行
       - rc.multi2～5执行
       - rc.local是杂项
       - init系统会安装以上顺序加载运行
       - rc.conf包含了相关的配置

+ sysV
       - 配置目录在`/etc/rc.d/rcx.d`其中x表示init号
       - 目录下的文件以S开头的是启动的服务，以K开头的是不启动的服务
       - 启动标识后紧跟的数字表示启动优先级，数值越小运行越早
       - 通过service命令来起停服务
       - 通过chkconf来管理服务

以上两种init系统加载的文件本身都是一些脚本文件，通过这些脚本可以加载相应的模块或者启动需要的程序。  
早期的init系统存在若干问题，比如，无法并行任务，任务之间缺乏有效的通信机制，对进程的监控只能通过PID来进行...

##### 现代init系统简介

systemd和upstart在这种情况下产生了，他们都是事件驱动的，不得不说的是由于systemd开始时间晚于upstart所以很多特性也借鉴了upstart，同时systemd还借鉴了Mac OS X的Launcher系统。

+ systemd  
>  systemd 是 Linux 下的一款系统和服务管理器，兼容 SysV 和 LSB 的启动脚本。systemd 的特性有：支持并行化任务；同时采用 socket 式与 D-Bus 总线式激活服务；按需启动守护进程（daemon）；利用 Linux 的 cgroups 监视进程；支持快照和系统恢复；维护挂载点和自动挂载点；各服务间基于依赖关系进行精密控制。  
                  --维基百科

systemctl来管理systemd，同时也兼容service命令
所有的systemd配置在`/usr/lib/systemd`目录下,当启动用后会被链接或者拷贝到/etc/systemd目录下

+ upstart
  使用initctl来管理upstart
  启动级别配置在`/etc/inittab`文件，与BSD和System V不同的是这个文件只有一行`id:3:initdefault`
  配置项在`/etc/init/`目录下

  *我印象中ubuntu的upstart的配置和centos的配置方法是有区别的，这里无法进一步验证。*


##### systemd和upstart的使用




我们可以通过`--init=xxx`内核参数来指定所使用的init系统。当然指定的这个init可以是任意的程序，你完全可以直接指定成为你想要的任何程序。
目前大部分Linux发行版本都已经采用systemd作为默认的init系统，ubuntu现在使用的是upstart系统，而Debain也在投票选择中。

资源推荐
----------

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
如果无法直接在邮件内查看，请访问[github上的页面](https://github.com/cnsworder/publication/blob/master/alpha5.md)或[网站](http://ssh.cnsworder.com/alpha5.html)。  
我们在github上开放编辑希望大家能参与到其中。
