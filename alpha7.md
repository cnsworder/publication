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
C是面向过程的语言这是大家都知道的，但是我们同样可以使用C来写出面向对象风格的代码来，最典型的例子就是GTK+了，个人认为它通过模拟面向对象的风格已经到了极致。当然kernel中也有很多这样的例子，但是我不想强加 `OOP` 的概念给它，因为它还有更深的设计理念。

OK, 面向对象编程的三个基本概念 `封装` ， `继承` ，`多态`。我会简单的结合GTK+的实现来说明如何用C 来实现 OOP。

### 继承

大家都懂的，继承具体要表达的内容是 `聚合` 和 `组合` 。而设计模式中曾经告诫大家优先使用组合方式来表达集成某些属性而不是继承。C 

``` C
typedef struct Base
{
    int id;
}base_t;

typedef struct Super
{
    Base;
    char data[];
}super_t;

super_t super;
super.id = 12;

``` 

### 多态

``` C
typedef struct Mu
{
    void (* test)();
}mu_t;

void test_a()
{
    printf("testA");
}

void test_b()
{
    printf("testB");
}

mu_t my_mu;
my_mu.test = test_a;

my_mu.test();

my_mu.test = test_b;

```
多态就这么容易的实现了，没有虚函数表的性能损耗。


### OOP

类的概念和对象的概念在很多OOP语言中感觉已经被很多人所混淆，而C来实现却需要分清楚，并且基于此将OOP的更低层次的一个概念隔离变化



现代化C++
---------------
**作者：江湖郎中** 

什么是现代的C++? 

C++标准化之前的C++自然不是现代化的C++，C++98也不是，而C++03引入了模板机制，并被发现是具有 `图灵完备` 的，为现代的C++奠定了基础，C++11和C++14逐步添加了泛函编程的特性，使得C++真正成为现代化的编程语言，而C++最终成为了现代化的C++。

### 模板元

模板元编程

写一个简单的std::function实现，当然他很简陋。

``` c++
template<typename T>
class function_base
{
public:
    function_base(T &&fn){fun = std::forward(fn);}
    ~function_base(){}

    template<typename ...Args>
    void operator()(Args... args){fun_(args...);}

    void operator()(){fun_();}

private:
    T fun_;
};
```



模板元是编译期确定的，所以他不会损耗性能，这一点要优于有些语言比如C#，java所实现的泛型。

如何高效的实现一个Fabic表达式？好吧, C++11就这么简单。

``` c++
```

### 泛函

泛函数编程的最主要的表现是函数是第一类对象 `First-Class Object` 。

auto, dectltype, result_of借助编译器的推导能力，添加lambda表达式，更好的实现闭包的特性。

#### 函数可以存入变量

``` C++
auto fun = [=](int a)->int{return a;};
```

#### 可以作为参数传递为其他函数

``` C++
void todo(auto fun)
{
    fun();
}
```

#### 可以被作为函数的返回值

``` C++
auto get_fun(auto fun) -> dectltype(fun)
{
    return fun;
}
```

#### 即使没有名称也可以存在 && 运行期来创建

``` C++
[](){return 0;}();
```

在原始的C,C++中可以把函数通过函数指针将函数作为参数或者返回值，也可以传递给一个指针左值，但是无法创建匿名函数和通过运行期来创建函数。而C++11加入了lambda表达式后这一些成为了可能。


个人参考C++11标准废弃的很多特性足以彻底改变C++语言(比如模块、概念)，但是正是因为改变所以被废弃掉了。C++真正的活力不在于它是多么的现代化，而在于它是多范式的，你既可以用面向过程的C来写代码（对C99的支持还有待提高啊），也可以用模板机制来写泛型代码，更可以在C++11以后写出泛函的代码。八卦一下，golang就是因为它爹因为要把很多特性提交到C++标准中没有被通过所有就写了一个golang~~~。嗯，下面我们来看一下golang。

大家学golang
-------------- 
首先说golang有些语法是反C的，不过始终没有脱离C。

### 关键字

>>

### 变量

### 函数

### 语句

### 结构体

### 接口

### 

资源推荐
----------


一段代码
--------
``` python

```

Tip
-------


 
- - -
欢迎群成员自荐自己的blog文章和收集的资源，发[邮件](mailto:cnsworder@gmail.com)给我，如果有意见或建议都可以mail我。  
如果无法直接在邮件内查看，请访问[github上的页面](https://github.com/cnsworder/publication/blob/master/alpha7.md)或[网站](http://ssh.cnsworder.com/alpha7.html)。  
我们在github上开放编辑希望大家能参与到其中。
