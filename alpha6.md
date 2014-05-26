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

其他功能性分支

+ feature分支进行新功能开发;
+ hotfix分支进行bug修复
+ release分支进行发布

git-flow这个工具可以帮助你完成这个过程。`git flow`命令帮可以你完成分支构建与合并的一系列的工作。

使用方法：

```
git flow <command> start [OPT]
git flow <command> finish [OPT]
```

你的使用过程如下：


![gitflow-command](http://danielkummer.github.io/git-flow-cheatsheet/img/git-flow-commands.png)

### 使用git来完成敏捷开发的闭环

敏捷的实践的问题一直很困扰我，我一方面认为敏捷是一个好的实践模型，但是却偏偏在实际工作过程中不断的发现敏捷不能够真正的达到目标，甚至连传统的管理效果都无法达到。

在实践中往往敏捷缺少某一个或几个工具而无法实现闭环，导致敏捷的效率并不 `敏捷` 。

在实践过程中发现借助git可以很好的完成这个闭环，并使用 `git` 来穿插整个敏捷过程或者穿插到现有的过程模型中。

`git`在过程管理中成为推动敏捷的工作流程的引擎。借助 `git` 的push动作自动化触发构建流程，走差流程，测试流程，发布流程。

微观方面的`快速冲突`、`及时响应` 和 `结对编程` 等过程在 `git` 的分支和并模型上能够达到很好的吻合。


### 个人对git flow的实践的改进
gitflow虽好，但不是万能的，且有很多的局限性能，并且如果希望穿插进现有的

#### 个人工作分支

每个人在本地工作状态的保存有时会出现一些问题，比如，你需要在两个机器上进行开发，你需要跨平台工作。

本地工作的版本库需要挂载分区来工作，或者需要远程连接你的另一台工作主机来fetch你的代码。这样可以达到目的，但是总不是那么顺畅。

而将工作分支同步的到中央仓库，带来了一个便捷性，和管理的统一性。

#### 添加测试、自动构建、自动部署分支

在发布前的测试环境，gitflow模型并没有提及，而在管理流程中测试往往很重要，并且是过程管理中的重要环节。而自动构建是自动化测试的前提。

+ 在 `git push` 时通过hook直接触发构建流程，并在构建结束后触发自动化测试。
+ 在成员 `git push` 后，测试人员pull代码进行测试。 

当你将代码push到中央仓库的release分支时，触发的自动构建流程相应的结束后被自动化工具push到响应的服务器。


### 文本化过程数据

版本管理工具对非文本化的版本管理能力很弱完全可以忽略不计，甚至于无法达到你通过修改非文本文档的命名的最原始的方式的效率。

通过一系列的工具将传统的pdf、ppt等使用markdown，rest等语法以源码方式管理，在需要的时候借助工具编译并发布出来。

### git走查代码

通过强制的限制可以在代码通过审核之前不能进入主干分支。你可以使用git

而达到每一行都能查找到对应的提交说明的方式，在走差过程中成为了有效的利器。由 `git blame` 查看响应行的提交HASH，由`git log HASH`来查看提交记录，从而使走查跟踪变得有效。


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
