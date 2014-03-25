<link rel="stylesheet" href="http://ssh.cnsworder.com/styles/monokai_sublime.css" />
<script type="text/javascript" src="http://ssh.cnsworder.com/highlight.pack.js"></script>
<script type="text/javascript">
    hljs.initHighlightingOnLoad();
</script>

GNU/Linux Developer
==============================================================  
![Tux](http://ssh.cnsworder.com/img/tux.png)  
**Alpha4**   
**主编: 猫猫**  

**微信号: linux_developer**   
**QQ群号: 20506135**   

《GNU/Linux Developer》第Alpha4期在春风中来了，本期**九州**有**大数据**、**android系统编译和定制**两个专题和大家分享，另外由于*ownone*工作太忙了**web.py**的专题将会在4月份继续和大家见面，这期 **猫猫** 会给大家带来HD2 **不死鸟传说** 的专题分享。
因为群成员发展迅速，大家商量了一下决定，分设四个专题群来分别讨论问题:

+ Linux开发1群[内核] 287465634 
+ Linux开发2群[服务] 20506196
+ Linux开发3群[应用] 19443596
+ Linux开发4群[基础] 48619264     

另外为了更好的和大家进行答疑互动，群新开设了[答疑网站](http://cnsworder.com),大家如果有问题无法及时解决可以发到网站上进行解决。如果大家感觉自己问题比较有代表性也可以发送到上面以方便其他人。

往期订阅的内容可以在[readthedocs](http://linux.readthedocs.org/zh_CN/latest/)上看到，几乎是和github上是同步的。(用Markdown排完然后还得再用reStructuredText再排一次版真的好累哦~~)

下期专题预告一下，郎中会给大家带来`Linux init系统介绍`，ownone应该会给大家继续`web.py`的内容，敬请大家期待吧~~~

哦，忘了还有另外的惊喜哦，暂时保密吧 :p 

专题分享
================
说来惭愧，郎中让我做此次专题并非我的水平有过人之处————菜鸟一枚，只是机缘巧合。说好的**《大数据》**内容因为才疏学浅加上公司事务占据太多时间，在此次专题中只做介绍性描述，以免贻笑大方.作为补充,此次专题会详细介绍**《安卓系统编译和定制》**内容。废话不多说，直入正题。

大数据
-------------------
**作者: 九州**

要理解大数据这一概念，首先要从”大”入手，”大”是指数据规模，大数据一般指在10TB(1TB=1024GB)规模以上的数据量。大数据同过去的海量数据有所区别，其基本特征为**体量大、多样性、价值密度低、速度快**。

### 大数据特点
+ 数据体量巨大。从TB级别，跃升到PB级别。
+ 数据类型繁多，如网络日志、视频、图片、地理位置信息，等等。
+ 价值密度低。以视频为例，连续不间断监控过程中，可能有用的数据仅仅有一两秒。
+ 处理速度快。1秒定律。最后这一点也是和传统的数据挖掘技术有着本质的不同。物联网、云计算、移动互联网、车联网、手机、平板电脑、PC以及遍布地球各个角落的各种各样的传感器，无一不是数据来源或者承载的方式。


### 大数据处理
大数据技术是指从各种各样类型的巨量数据中，快速获得有价值信息的技术。解决大数据问题的核心是大数据技术。目前所说的”大数据”不仅指数据本身的规模，也包括采集数据的工具、平台和数据分析系统。大数据研发目的是发展大数据技术并将其应用到相关领域，通过解决巨量数据处理问题促进其突破性发展。因此，大数据时代带来的挑战不仅体现在如何处理巨量数据从中获取有价值的信息，也体现在如何加强大数据技术研发，抢占时代发展的前沿。

下面来介绍一下通用的大数据处理流程。

#### 1. 采集

大数据的采集是指利用多个数据库来接收发自客户端（Web、App或者传感器形式等）的数据，并且用户可以通过这些数据库来进行简单的查询和处理工作。比如，电商会使用传统的关系型数据库`MySQL`和`Oracle`等来存储每一笔事务数据，除此之外，`Redis`和`MongoDB`这样的`NoSQL`数据库也常用于数据的采集

在大数据的采集过程中，其主要特点和挑战是并发数高，因为同时有可能会有成千上万的用户来进行访问和操作，比如火车票售票网站和淘宝，它们并发的访问量在峰值时达到上百万，所以需要在采集端部署大量数据库才能支撑。并且如何在这些数据库之间进行负载均衡和分片的确是需要深入的思考和设计。

#### 2. 导入/预处理

虽然采集端本身会有很多数据库，但是如果要对这些海量数据进行有效的分析，还是应该将这些来自前端的数据导入到一个集中的大型分布式数据库，或者分布式存储集群，并且可以在导入基础上做一些简单的清洗和预处理工作。也有一些用户会在导入时使用来自Twitter的Storm来对数据进行流式计算，来满足部分业务的实时计算需求。

导入与预处理过程的特点和挑战主要是导入的数据量大，每秒钟的导入量经常会达到百兆，甚至千兆级别。

#### 3. 统计/分析

统计与分析主要利用分布式数据库，或者分布式计算集群来对存储于其内的海量数据进行普通的分析和分类汇总等，以满足大多数常见的分析需求，在这方面，一些实时性需求会用到EMC的GreenPlum、Oracle的Exadata，以及基于MySQL的列式存储Infobright等，而一些批处理，或者基于半结构化数据的需求可以使用Hadoop。

统计与分析这部分的主要特点和挑战是分析涉及的数据量大，其对系统资源，特别是I/O会有极大的占用。

#### 4. 挖掘

与前面统计和分析过程不同的是，数据挖掘一般没有什么预先设定好的主题，主要是在现有数据上面进行基于各种算法的计算，从而起到预测（Predict）的效果，从而实现一些高级别数据分析的需求。比较典型算法有用于聚类的Kmeans、用于统计学习的SVM和用于分类的NaiveBayes，主要使用的工具有Hadoop的Mahout等。该过程的特点和挑战主要是用于挖掘的算法很复杂，并且计算涉及的数据量和计算量都很大，常用数据挖掘算法都以单线程为主。

整个大数据处理的普遍流程至少应该满足这四个方面的步骤，才能算得上是一个比较完整的大数据处理。


### 大数据作用

1. 营销
营销的本质是找出潜在顾客，向其发布信息，最终达成交易。
收集海量的消费者信息，然后利用大数据建模技术，按消费者属性（如所在地区、性别）和兴趣、购买行为等维度，挖掘目标消费者，然后进行分类，再根据这些，对个体消费者进行营销信息推送。目前概念火热的精准营销就是如此
2. 内部运营
相比营销，大数据在内部运营中的应用更深入，对于企业内部的信息化水平，以及数据采集和分析能力的要求更高。本质上，是将企业外部海量消费者数据与企业内部海量运营数据联系起来，在分析中得到新的洞察，提升运营效率。
3. 大数据用于决策
在大数据时代，企业面对众多新的数据源和海量数据，能否基于对这些数据的洞察，进行决策，进而将其变成一项企业竞争优势的来源？同大数据营销和大数据内部运营相比，运用大数据决策难度最高，因为它需要一种依赖数据的思维习惯。

安卓编译定制初步
--------------------------------
**作者: 九州**

Android 开源代码的特性使我们能够非常方便的定制，满足各种不同的需要。下面介绍怎么编译、定制android 代码满足个人需要。

### 确定需求

恶意应用在后台悄悄发送、屏蔽短信订购SP业务已成为安卓一大危害， 而需求在此产生——我希望手机系统能够详细记录: **手机内哪个应用在什么时候向谁发送了什么内容的短信**，简称`4W`信息


### 初步设计

恶意应用一般使用`sendTextMessage`函数后台发送短信，那么解决方案看起来很直接——在函数实现内插桩，桩代码将函数调用信息输出到`Log`。那么，查看`Log`文件自然就知道短信的`4W`信息。

### 实践操作

#### 下载源代码

直接使用Google提供的源代码有个问题就是编译出来的系统只适用于特定的几款手机。所以这里使用`CyanogenMod`项目代码。可以简单认为`CyanogenMod`是在Goole原生代码基础上适配了更多的手机机型。[项目地址]

**下载源代码的过程**

1. 下载并添加 repo 文件到用户环境变量。  
> https://code.google.com/p/git-repo/downloads/list?can=1&q=

2. 建立代码存放目录
> cd ~ 　
> mkdir androisource 

3. 在代码存放目录内执行  
> cd androidsource 
> repo init -u git://github.com/CyanogenMod/android.git -b [版本]  
以“gingerbread-release”（对应android2.3.7 ) 版本为例完整命令格式为:
> repo init -u git://github.com/CyanogenMod/android.git -b gingerbread-release  

4. 初始化完成后执行下载源代码
> repo sync
或
> repo sync -j [n]

区别在于前者使用单进程，后者使用了 n 进程下载。

#### 初始化编译环境
 整个android的编译依赖关系比较简单，安装好指定的包就即可，这里不做详细介绍 ，具体参见:<http://source.android.com/source/initializing.html>。但有一点需要指出的是编译 2.3以上 androd 版本必须使用sun java 1.6 

#### 添加系统服务
虽然在 “**初步设计**”中我们描述的方案是桩代码直接记录信息到*log*文件，但此设计不便于扩展，在实践中我们采用系统服务代理模式。

Android本身提供了`isms`, `search`, `network_management`等系统服务实现不同的功能。`sendTextMessage`函数实际上就是使用`isms`服务发送短信。

```java
//frameworks/base/telephony/java/android/telephony/SmsManager.java
    
public void sendTextMessage(
        String destinationAddress, String scAddress, String text,
        PendingIntent sentIntent, PendingIntent deliveryIntent) {
    if (TextUtils.isEmpty(destinationAddress)) {
        throw new IllegalArgumentException("Invalid destinationAddress");
    }
    if (TextUtils.isEmpty(text)) {
        throw new IllegalArgumentException("Invalid message body");
    }
    try {
        ISms iccISms = ISms.Stub.asInterface(ServiceManager.getService("isms"));
        if (iccISms != null) {
            iccISms.sendText(destinationAddress, scAddress, text, sentIntent, deliveryIntent); 
        }
    } 
    catch (RemoteException ex) {
        // ignore it
    }
}
```

借鉴于此，我们可以自定义一个` ilog`系统服务 ，并在`sendTextMessag`函数内插桩 ，代码如下：

```java    
public void sendTextMessage(
         String destinationAddress, String scAddress, String text,
         PendingIntent sentIntent, PendingIntent deliveryIntent) {
    if (TextUtils.isEmpty(destinationAddress)) {
        throw new IllegalArgumentException("Invalid destinationAddress");
    }
    if (TextUtils.isEmpty(text)) {
        throw new IllegalArgumentException("Invalid message body");
    }
    try {
        ILog ilog = ILog.Stub.asInterface(ServiceManager.getService("ilog"));
        if (ilog != null) {
            String[] logInfo=new String[3];
            logInfo[0]=destinationAddress;
            logInfo[1]=scAddress;
            logInfo[2]=text;
            ilog.log("sendTextMessage", logInfo);
        }
     } 
    catch (RemoteException ex) {
         // ignore it
    }
    try {
        ISms iccISms = ISms.Stub.asInterface(ServiceManager.getService("isms"));
        if (iccISms != null) {
            iccISms.sendText(destinationAddress, scAddress, text, sentIntent, deliveryIntent);
        }
    } catch (RemoteException ex) {
         // ignore it
    }
}
```

在`log(String, String[])`函数中，可以定制自己想要的效果，比如记录到文件，弹出通知栏提示等。

添加安卓系统服务需要一个接口文件（aidl）和一个实现文件（java），关系类似于 c++ 类的头文件与定义文件。参见: <http://processors.wiki.ti.com/index.php/Android-Adding_SystemService>

具体的添加或修改代码如下：

**`frameworks/base/core/java/android/os/ILog.aidl`**
```java
/*
* aidl file : frameworks/base/core/java/android/os/ILog.aidl
* This file contains definitions of functions which are exposed by service 
*/
package android.os;
interface ILog {
    /**
    * {@hide}
    */
    void log(String function ,in String[] logInfo);
}
```     
 

**`frameworks/base/services/java/com/android/server/LogService.java`**

```java
package com.android.server;
import android.app.ActivityManager;
import android.content.Context;
import android.content.pm.PackageManager;
import android.os.*;
import android.os.ILog;
import java.io.*;
import java.text.SimpleDateFormat;
import java.util.Date;
import java.util.List;

public class LogService extends ILog.Stub {

    public LogService(Context context) {
        super();
        mContext = context;
    }
    
    //获取调用该服务的应用包名
    private String getPackageName(int pid, int uid) {
        PackageManager mPkgMgr = mContext.getPackageManager();
        String[] pkgs = new String[0];
        if (mPkgMgr != null) {
            pkgs = mPkgMgr.getPackagesForUid(uid);
        }
        if (pkgs != null && pkgs.length == 1) {
            return pkgs[0];
        }
        ActivityManager am = (ActivityManager) mContext.getSystemService(Context.ACTIVITY_SERVICE);
        List<ActivityManager.RunningAppProcessInfo> apps = am.getRunningAppProcesses();
        if (apps != null) {
            for (ActivityManager.RunningAppProcessInfo info : apps) {
               if (info.pid == pid) {
                    return info.processName;
                }
            }
        }
        return "unknown";
    }

    //将信息写入文件
    private int writeToFile(String funciton ,String[] logInfo ,String packageName) {
        File ilogWorkDir = mContext.getDir("/data/data/ilog", 0);
        if (!ilogWorkDir.exists()) {
           ilogWorkDir.mkdir();
        }
        File ilogOutFile = new File("/data/data/ilog", "smsLog.txt");
        FileOutputStream fos = null;
        try {
            fos = new FileOutputStream(ilogOutFile, true);
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        }
        DataOutputStream dos=new DataOutputStream(fos);
        StringBuffer stringBuffer=new StringBuffer();
        stringBuffer.append("Time:")
                    .append(new SimpleDateFormat("yyyy-MM-dd HH:mm:ss:SSS")
                    .format(new Date())).append("\r\n");
        stringBuffer.append(String.format("FunName:%s", logInfo[0])).append("\r\n");
        stringBuffer.append("Info:").append("\r\n");
        for (int i = 1; i < logInfo.length; ++i) {
           stringBuffer.append("    ").append(logInfo[i]).append("\r\n");
        }
        stringBuffer.append("\r\n\r\n");
        try {
           dos.write(stringBuffer.toString().getBytes());
        } catch (IOException e) {
            e.printStackTrace();
        }
        return 0;
    }
    
    public void log(String function, String info[]) {
        String packageName = null;
            packageName = getPackageName(Binder.getCallingPid(), Binder.getCallingUid());
            writeToFile(function ,info,packageName);
        }
    
    final private Context mContext;
}
```    

**`frameworks/base/services/java/com/android/server/SystemServer.java`**

```java        
/*
 * go to function "@Override public void run()"
* ........ 
* Add following block after line "if (factoryTest != SystemServer.FACTORY_TEST_LOW_LEVEL) " 
*/ 
try {
    Slog.i(TAG, "ilog");
    ServiceManager.addService("ilog", new LogService(context));
} catch (Throwable e) { 
    Slog.e(TAG, "Failure starting LogService Service", e);
} 
```
     
**`frameworks/base/Android.mk`**

```make
/*
 * open frameworks/base/Android.mk and add following line
 */
...
core/java/android/os/IPowerManager.aidl \
core/java/android/os/ILog.aidl \
core/java/android/os/IRemoteCallback.aidl \
...
```
 
### 编译
`CyanogenMod gingerbread-release`版本适配了60多款手机[1][2]。

为官方支持的手机编译出ROM比较简单，命令格式如下：

```bash
cd device/[厂商］/[手机别名]
./extract-files.sh
./setup-makefiles.sh
cd ../../..

cd vendor/cyanogen
./get-rommanager
cd ../..

source ./build/envsetup.sh
lunch cyanogen_[手机别名]-eng
make clean
brunch [手机别名]
```

以我手上的测试机`htc G9`(别名 liberty)为例：

```bash
cd device/htc/liberty
./extract-files.sh
./setup-makefiles.sh
cd ../../..

cd vendor/cyanogen
./get-rommanager
cd ../..

source ./build/envsetup.sh
lunch cyanogen_liberty-eng
make clean
brunch liberty
```

编译期间出现的问题大多为依赖包未安装，根据提示安装好即可

**编译完成后会在`/out/target/product/[手机别名]目录生成cm-7-[日期]-UNOFFICIAL-[手机别名].zip`，可以使用刷机精灵之类的软件刷机入对应的手机当有应用调 sendTextMessage函数时，就会记录到  /data/data/ilog/smsLog.txt。需求满足**


[项目地址]: https://github.com/CyanogenMod/android
[1]: http://wiki.cyanogenmod.org/w/Devices#type="phone";cmversions="7"
[2]: http://wiki.cyanogenmod.org/w/Devices#type=%22phone%22;cmversions=%227%22


不死鸟传说
--------------------------------------------
**作者: 猫猫**

其实原本这是郎中的地盘，后来我看过了九州关于定制android的内容后，灵机一动，就给自己挖了个坑。我是真心的没想到今天会回家这么晚滴……

这一段的本意隆重的推介一下HTC的HD2，也就是Loe，手机界的第一神机。HD2现在还是我调试android程序的不二选择，目前这货里面共存了六个系统，包括一个FFOS和一个WP7.8，外加四个不同版本的android……

#### [不死鸟传说](http://www.wangxiaomao.net/?p=1139) 
>就在HD2价格落到最底点的时候，就在更多人把眼光高高的仰望到硬件越来越眼花缭乱的安卓机的时候，XDA的大神们默默的发布了可以用在HD2上的安卓ROM。其实吧，说实话，能在HD2彻底死亡之前及时的出来安卓ROM，私以为与HTC后续的几款手机，比如G5、G7，用的都是和HD2一样的处理器不无关系。
[更多](http://www.wangxiaomao.net/?p=1139) 

#### [Wp7加无限制android（NativeSD）刷机方法](http://www.wangxiaomao.net/?p=122) 
>个人认为，NativeSD是不死鸟最炫丽的羽毛
>NativeSD也是xda的妖物们弄出来的一套HD2刷机方法，原理上基本就是在tf卡中划分出一个ext4的分区，然后把android的系统解包到这个ext4分区的目录中，再挂载这个目录从而实现启动android的目的。虽然听上去和卡模版的android区别不大，不过NativeSD是直接解包到卡上运行的，理论上说只要卡的速度够快，android的运行速度会超过直刷到ROM中的速度的。[更多](http://www.wangxiaomao.net/?p=122) 

**下面数篇是我自己做的或者改的HD2的NativeSD ROM**
#### [crane--sense--androi2.3.5](http://www.wangxiaomao.net/?p=1178) 
>Sense在所有的安卓UI中一直是我的最爱。说不清楚为啥，也许是从WM时代带过来的习惯，也许是因为Sense真的很好用。不过，似乎Sense在伴随着HTC一起沉沦吗？[更多](http://www.wangxiaomao.net/?p=1178)

#### [peacock--MIUI--android2.3.7](http://www.wangxiaomao.net/?p=1086) 
>MIUI这个系统一出场就给1.x年代的安卓世界眼前一亮的感觉。那时候安卓的UI着实的丑，被IOS死死地压制，但是走对IOS大规模山寨之路的MIUI算是当年安卓界的异类了——至少用起来简单，不那么难用，响应速度也快。不过正如IOS的UI一样，MIUI这种风格的UI，由于可随意定制性性对差一些，用时间久了会些许有些腻味了。[更多](http://www.wangxiaomao.net/?p=1086)

#### [swan--CM10.1--androi4.2.2](http://www.wangxiaomao.net/?p=1093) 
>CM的出现很多大程度上改变了安卓界的格局。原本各手机厂商为了多卖新机型，对老机型系统的支持和更新翻脸就不认账，但是自从有了CM，情况就变成了用户可以不卖手机厂商的帐了——反正不管啥版本的系统，几乎没有CM找不到的。
[更多](http://www.wangxiaomao.net/?p=1093)

#### [sparrow--deepin--androi4.1.2](http://www.wangxiaomao.net/?p=1112) 
>深度曾经也算是盗版windows很有地位的一员，自从番茄入狱以后，深度也干起了洗白的大潮。后起的DeepinLinux和SenduOS也算是中规中矩吧，不过似乎用户群双双都不大，另外感觉效果上似乎离MIUI还是有不小的距离。简单的用了下，还算是流畅吧，不过没装什么软件(CM10在不装软件的时候也算得上比较流畅了，装了软件就卡卡卡卡卡……)。[更多](http://www.wangxiaomao.net/?p=1112)

#### [ostrich--FireFoxOS](http://www.wangxiaomao.net/?cat=6) 
>FireFoxOS也算是Linux系手机操作系统的异类了，不过它毕竟是Linux。由于有了各种安卓的前车之鉴，只要是Linux系统的手机操作系统，在HD2这里都可以做成NativeSD的，SO，XDA的大神们果然就做了。[更多](http://www.wangxiaomao.net/?p=1123)

**外三篇——掘完HD2的坟回来掘G6**
#### [Legend：制作金卡](http://www.wangxiaomao.net/?p=8) 
>为什么需要金卡、金卡的作用、是不是可以不用金卡……这些问题我都不想讨论了，说实话，我还真不知道。但是我对金卡的认识是，这东西在某些情况下确实有用，所以就找一张体质好的容量小的卡做一个放那吧，反正小容量的卡一般来说也没多大用处了。OK，我不会告诉你这一章是可以跳过的。[更多](http://www.wangxiaomao.net/?p=8)

#### [Legend：刷RUU](http://www.wangxiaomao.net/?p=20) 
>RUU，是ROM Upgrade Utility英文缩写，意思是ROM升级工具包(即ROM更新实用程序)，它一般由HTC官方发布，在电脑端简单快速地升级手机固件(ROM)的套件。即所谓的官方ROM，官方到不能再官方的ROM。[更多](http://www.wangxiaomao.net/?p=20)

#### [Legend：从RUU中提取ROM](http://www.wangxiaomao.net/?p=24) 
>[更多](http://www.wangxiaomao.net/?p=24)

**番外之番外**

其实在提取完G6 ROM以后，原本是要写怎么裁剪系统的，后来因为种种原因一直都没写。偶一直都以为偶食言了，今天看了看，原来那时候根本就没有预告要写这个呀，万幸万幸，偶还是个讲信用滴人……
其实裁剪手机系统是灰常简单的事情，把zip解压缩，把里面的/system/app下面或者/data/app里面的各种不需要的让人恶心的apk删掉，然后这个世界就清净了。
如果从网上找教程的话，会有很多教程都说最后一步要签名，签名不对无法刷入云云，其实，据我观察，似乎现在的很多手机由于用的recovery并不是有那么严格的限制，所以rom包其实根本就不需要在意签名了，只要rom里面的内容正确，刷机脚本没问题就一切OK……这到底是进步还是倒退？需要签名好像也就是HTC刚开始的机型这么干过吧，后来刷G12的时候，根本就没有G6那样的签名障碍呢。
android已经让这个世界疯了。一切似乎都变得廉价和触手可得了。

资源推荐
=============

[python入门](http://pythontutor.com/)

[docker入门](https://www.docker.io/gettingstarted/#)

[golang入门](http://tour.golang.org/#1)


code block
===========
上期**ownone**给出了函数方法定义修饰器的方法，偶尔看到了皓哥写的通过类方式定义的方法感觉眼前一亮，现分享给大家

```python
class Dec(object):

    def __init__(self, tag):
        self._tag = tag

    def __call__(self, fun):

        def wrapped(*args, **args):
            fun()
            return "called"
        
        return wrapped

@Dec(tag="b")
def function():
    return "functed"
    
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
![Photo](http://ssh.cnsworder.com/img/jz.jpg)  
网名：九州 
群ID: [广州]九州  
微博：<http://t.qq.com/adu_na>   
技术：偏好c/c++ , 快忘干净的python ，以及工作偶尔用到的 java  
简介：广州低阶IT人士，做过安卓安全研究，目前从事网络协议分析 ，希望以后能专职开发

- - -
欢迎群成员自荐自己的blog文章和收集的资源，发[邮件](mailto:cnsworder@gmail.com)给我，如果有意见或建议都可以mail我。  
如果无法直接在邮件内查看，请访问[github上的页面](https://github.com/cnsworder/publication/blob/master/alpha4.md)或[网站](http://ssh.cnsworder.com/alpha4.html)。  
如果查看历史订阅请到[readthedocs](http://linux.readthedocs.org/zh_CN/latest/)。  
我们在github上开放编辑希望大家能参与到其中。
