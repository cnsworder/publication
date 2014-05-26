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

git仅仅是一个分布式版本管理系统，但是它却改进的却不仅仅是版本管理的由中央仓库转换成了分布式。

传统的svn管理方式由于采用整体复制的方式来生成branch和tag，所以成本太高。所以只有在重要阶段才会去使用branch和tag。
而git object模型在生成branch和git是成本却很小，branch成为了常态，合并与快速冲突成了必然。
如果我们以敏捷开发的观点来看这个问题，git的模型更加适合敏捷的快速响应的问题。

使用git进行管理最佳的实践模型有很多，而git flow是最著名的一个。

### git flow

![gitflow](http://ihower.tw/blog/wp-content/uploads/2011/02/Screen-shot-2009-12-24-at-11.32.03.png)

我们看图说话。

主干是master和developer分支，所有的代码以这两个分支为基准。

git-flow这个工具可以帮助你完成这个过程。`git flow`命令帮可以你完成分支构建与合并的一系列的工作。

使用方法：

```
git flow <command> start [OPT]
git flow <command> finish [OPT]
```

你的使用过程如下：


![gitflow-command](http://danielkummer.github.io/git-flow-cheatsheet/img/git-flow-commands.png)


### 个人对git flow的实践的改进

#### 个人工作分支
#### 添加测试分支
#### 添加自动构建分支
#### 添加自动化部署分支

### git走查代码

通过强制的限制可以在代码通过审核之前不能进入主干分支。你可以使用git


### 开源社区使用git协作

github上对代码进行管理，review，通知;
邮件和irc进行即使交流;
trello上做任务管理;
google doc进行文档的协同编辑;
产品可以快速的发布到云服务平台上;
距离不是问题，问题是交流产生了问题。目前有很多的免费的小团队的协作工具，国内比较有名的是tower.im


资源推荐
----------

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

#### 运维

#### 使用




- - - -
欢迎群成员自荐自己的blog文章和收集的资源，发[邮件](mailto:cnsworder@gmail.com)给我，如果有意见或建议都可以mail我。  
如果无法直接在邮件内查看，请访问[github上的页面](https://github.com/cnsworder/publication/blob/master/alpha6.md)或[网站](http://docs.cnsworder.com)。  
我们在github上开放编辑希望大家能参与到其中。
