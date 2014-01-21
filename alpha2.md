<link rel="stylesheet" href="http://ssh.cnsworder.com/styles/monokai_sublime.css" />
<script type="text/javascript" src="http://ssh.cnsworder.com/highlight.pack.js"></script>
<script type="text/javascript">
    hljs.initHighlightingOnLoad();
</script>

![Tux](http://ssh.cnsworder.com/img/tux.png)

GNU/Linux Developer
==============================================================  

**Aplha2**  
**kernel stable： 3.13**  
**QQ群号： 20506135**  
**微信号： linux_developer**  
**本期编辑： 猫猫**  

《GNU/Linux Developer》第**Aplha2**期在春节前和大家见面了，本期*唯一*将为大家带来专题**使用Python构建简易推荐系统**。  


本期专题：使用Python简易推荐系统的构建
-----------
**作者：[唯一](#tj)**  


很惭愧的，被**猫猫**给坑了，让我一个半桶水的家伙，跟大家分享个挺好玩的东西。尽管不是挺深入，但是绝对够科普。


个性化推荐是根据用户的兴趣特点和购买行为，向用户推荐用户感兴趣的信息和商品。
随着电子商务规模的不断扩大，商品个数和种类快速增长，顾客需要花费大量的时间才能找到自己想买的商品。
这种浏览大量无关的信息和产品过程无疑会使淹没在信息过载问题中的消费者不断流失。为了解决这些问题，个性化推荐系统应运而生。
个性化推荐系统是建立在海量数据挖掘基础上的一种高级商务智能平台，以帮助电子商务网站为其顾客购物提供完全个性化的决策支持和信息服务。[参考](http://baike.baidu.com/link?url=gbQqn-cunVUepgu9tUmsvTDSTm_goZZTJfgBgB1Yj8OJ8T4xtB_D_kt3GAaqCbY8Qgijl9GmR88KdiUXbXYKj_)


说白了，推荐系统就是你在淘宝或者亚马逊购买东西的时候，应该可以看到页面中，会出现由网站向你推荐的商品，往往那些商品会让你觉得很适合你,据说，推荐系统为亚马逊带来的的利润是30%，也就是意味着有30%的成交量是用户因为网站的推荐而购买的。

接下来会以代码加讲解的形式来讲述这个专题。
OK,首先我们今天是用最简单的方式来构建推荐系统，这个方式被称之为协同过滤，意思就是大家一起协同来过滤出一些有用的信息（我猜的）。
这种方式是基于用户的（还有基于内容之类的方式，由于比较深入就不讲究了）。
大家对电影可能比较熟悉，也有去过豆瓣之类的站点为自己看过的电影打分吧。接下来，我们就自己模拟一堆数据，来做演示。

``` json
perfers = {
    'Tom': {'Movie1': 2.5, 'Movie2': 3.5, 'Movie3': 3.0, 'Movie4': 3.5, 'Movie5': 2.5, 'Movie6': 3.0},
    'Jackson': {'Movie1': 3.0, 'Movie2': 3.5, 'Movie3': 1.5, 'Movie4': 5.0, 'Movie6': 3.0, 'Movie5': 3.5},
    'Scotte': {'Movie1': 2.5, 'Movie2': 3.0, 'Movie4': 3.5, 'Movie6': 4.0},
    'Abby': {'Movie2': 3.5, 'Movie3': 3.0, 'Movie6': 4.5, 'Movie4': 4.0, 'Movie5': 2.5},
    'Aimee': {'Movie1': 3.0, 'Movie2': 4.0, 'Movie3': 2.0, 'Movie4': 3.0, 'Movie6': 3.0, 'Movie5': 2.0},
    'Angelia': {'Movie1': 3.0, 'Movie2': 4.0, 'Movie6': 3.0, 'Movie4': 5.0, 'Movie5': 3.5},
    'Jack': {'Movie2': 4.5, 'Movie5': 1.0, 'Movie4': 4.0}
}
```

在这里，我们用perfers这个字典来保存Tom、Jackson...Jack等用户对不同的电影的评分，如果你们接下来有兴趣去试一下的，可以去调用豆瓣的接口收集一些用户对不同的电影的评分。
这里有一些用户会有其他用户没有评分过的电影的评分，这里假设没有评分就是没有看过。
OK，接下来，我们要引入一个概念，那个概念就是用户的相似度，我们如何判断两个用户之间的相似度呢？

我们在数学上应该学过两条点之间的距离，也就是欧几里得距离[参考](http://baike.baidu.com/view/2869924.htm?fromtitle=%E6%AC%A7%E5%87%A0%E9%87%8C%E5%BE%97%E8%B7%9D%E7%A6%BB&fromid=2701459&type=syn),欧几里得距离会等于 `sqrt(sum(xs-ys,2))`
因此我们定义一个函数：
`相似度 = 1 /（1 + 欧式距离）`   
加1 是为了防止距离为0  

``` python
def sim_distance(prefs, person1, person2):
    si = {}
    for it in prefs[person1]: #找出共同点
        if it in prefs[person2]:
            si[it] = 1
    if len(si) == 0:
        return 0
    pSum = math.sqrt(sum(pow(prefs[person1][it] - prefs[person2][it],2) for it in si))
    return 1.0 / (1 + pSum)
```

运行下面的结果得到 
> `print sim_distance(perfers,"Tom","Jackson")`  
> `0.294298055086`

欧几里得距离评价法是一种比较简单的方法。但是由于存在一些用户总是倾向于评分过高或过低（相对平均值），
这时兴趣相似的用户并不能通过此方法计算出来。Pearson相关系数是根据两组数据与某一直线的拟合程度来衡量的。  
OK,Pearson相关系数，又叫做[皮尔逊相关系数](http://zh.wikipedia.org/wiki/%E7%9A%AE%E5%B0%94%E9%80%8A%E7%A7%AF%E7%9F%A9%E7%9B%B8%E5%85%B3%E7%B3%BB%E6%95%B0),（我也看不懂，直接扔代码...）  
``` python
def sim_pearson(prefer, person1, person2):
    sim = {}
    #查找双方都评价过的项
    for item in prefer[person1]:
        if item in prefer[person2]:
            sim[item] = 1           #将相同项添加到字典sim中
    #元素个数
    n = len(sim)
    if len(sim) == 0:
        return 0
    # 所有偏好之和
    sum1 = sum([prefer[person1][item] for item in sim])  #1.sum([1,4,5,,,])  2.list的灵活生成方式!
    sum2 = sum([prefer[person2][item] for item in sim])
    #求平方和
    sum1Sq = sum( [pow(prefer[person1][item], 2) for item in sim] )
    sum2Sq = sum( [pow(prefer[person2][item], 2) for item in sim] )
    #求乘积之和 ∑XiYi
    sumMulti = sum([prefer[person1][item] * prefer[person2][item] for item in sim])
    num1 = sumMulti - (sum1*sum2/n)
    num2 = math.sqrt((sum1Sq-pow(sum1,2) / n) * (sum2Sq - pow(sum2, 2) / n))
    if num2 == 0:
        return 0
    return num1 / num2
```

**测试下**
> print sim_pearson(perfers, "Tom", "Jackson")  
> 0.396059017191

看到了吧，通过上述的方式我们可以计算出一个两个用户之间的相似度（也就是对同一种东西的看法的相似度，那所谓的推荐系统是不是呼之欲出了呢）。没错，刚刚开始最简单的推荐系统就是通过计算每一个用户跟其他用户的相似度，然后按照相似度排序完之后，将相似度高的A向B推荐B没有接触过而A已经接触过的东西。  
**注：**这种方式也就是基于用户的协同过滤，此时用于物品基本上跟用户之间的比例差不大的情况下才适合。如果用户多了呢，此时怎么办，留给大家的思考  
OK，老规矩，继续贴代码。此时定义一个函数名字叫做 *topMatches* 用来得到某个人的排序过的用户匹配度，代码相当简单就不解释了。  
``` python
def topMatches(prefs, person, n = 5, similarity = sim_pearson):
    scores=[(similarity(prefs, person, other),other)
            for other in prefs if other != person]
    scores.sort()
    scores.reverse()
    return scores[0:n] 
```
**测试下**  	
> print topMatches(perfers, "Tom")  
> print topMatches(perfers, "Jack")  

哈哈，你们看到Jack跟Tom不愧是一对好基友吧...  

> [(0.9912407071619299, 'Jack'), (0.7470178808339965, 'Angelia'), (0.5940885257860044, 'Aimee'), (0.5669467095138396, 'Abby'), (0.40451991747794525, 'Scotte')]  
> [(0.9912407071619299, 'Tom'), (0.9244734516419049, 'Aimee'), (0.8934051474415647, 'Abby'), (0.66284898035987, 'Angelia'), (0.38124642583151164, 'Jackson')]

那接下来，进入最后一步了，请问，我想得到推荐给Tom的东西要怎么做...  
``` python
def getRecommendations(prefs,person,similarity = sim_pearson):
    totals = {}
    simSums = {}
    for other in prefs:
        if other == person: continue
        
        sim = similarity(prefs, person, other)

        if sim <= 0: continue
        
        for item in prefs[other]:
            if item not in prefs[person] or prefs[person][item] == 0:
                totals.setdefault(item, 0)
                totals[item] += prefs[other][item] * sim
                simSums.setdefault(item, 0)
                simSums[item] += sim
    
    rankings = [(total / simSums[item], item) for item, total in totals.items()]

    rankings.sort()
    rankings.reverse()
    return rankings
```
**测试下** 
> print getRecommendations(perfers,"Tom")  
> print getRecommendations(perfers,"Jack")  
> []  
> [(3.3477895267131013, 'Movie6'), (2.832549918264162, 'Movie1'), (2.530980703765565, 'Movie3')]  

这个时候因为Tom已经看过所有的电影了，所以没得推荐了...

行吧，本期的献丑也到此为止了，由于本人也是因为工作需要刚刚接触，所以有兴趣的一起交流哈。
另外鄙视下坑我的**猫猫**。。。大家一起鄙视下，同时期待**猫猫**带来的**Cubieboard**开发板专题。

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
