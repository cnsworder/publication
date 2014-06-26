<link rel="stylesheet" href="http://ssh.cnsworder.com/styles/monokai_sublime.css" />
<script type="text/javascript" src="http://ssh.cnsworder.com/highlight.pack.js"></script>
<script type="text/javascript">
    hljs.initHighlightingOnLoad();
</script>

GNU/Linux Developer
==============================================================  

**Version: Aplha7** 
**QQ群号: 20506135**  
**微信号: linux_developer**  
**主编: 猫猫**  

《GNU/Linux Developer》第 **Aplha7** 期发布了，本期将为大家带来专题 **面向对象的C** 和 **现代化C++** 。  

专题分享
==============

面向对象的C
-------------------
**作者：江湖郎中**  
C是面向过程的语言这是大家都知道的，但是我们同样可以使用C来写出面向对象风格的代码来，最典型的例子就是GTK+了，个人认为它通过模拟面向对象的风格已经到了极致。当然kernel中也有很多这样的例子，但是我不想强加 `OOP` 的概念给它，因为它还有更深的设计理念。

OK, 面向对象编程的三个基本概念 `封装` ， `继承` ，`多态`。我会简单的结合GTK+的实现来说明如何用C 来实现 OOP。

先看一下段Gtk写的代码，再次说明一下这不是一篇Gtk的教程，更不是GObject的解析，我只是会借Gtk的一些实现例子来说明问题，并且个人感觉Gtk的GObject实现确实是C来写面向对象的很好的示例。

``` C
#include <gtk/gtk.h>
#include <libintl.h>

#define _(x) gettext(x)

int main(int argc, char *argv[])
{
    GtkWidget *window, *button, *vbox;

    gtk_init(&argc, &argv);

    window = gtk_window_new(GTK_WINDOW_TOPLEVEL);
    button = gtk_button_new_with_label(_("Hello World"));
    vbox = gtk_vbox_new(FALSE, 0);

    gtk_container_add(GTK_CONTAINER(window), vbox);
    gtk_container_add(GTK_CONTAINER(vbox), button);

    gtk_widget_show_all(window);
    gtk_main();
    return 0;
}

```
刚才我说过这我们不去看Gtk的内容，而是关注面向对象的部分。  
`gtk_windows_new` 、 `gtk_vbox_new` 和 `gtk_button_new_with_label` 来实例化了一个GtkWidget对象。   
Gtk 是怎么实现的呢？好吧，那我们一步一步的来吧。

### 继承

大家都懂的，继承具体要表达的内容是 `聚合` 和 `组合` 。而GOF设计模式中曾经告诫大家优先使用组合方式来表达集成某些属性而不是继承。C中是没有直接继承的语法的，那就用组合和聚合来实现复用，但更自然的表述这一特性组合更加合适。

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

C++的多态分为运行时多态和编译期多态，而大部分其他语言的多态主要是运行时多态，依赖语言的运行时环境、虚拟机、解释器等。C++的运行时多态是依赖虚函数表的，性能要甩那些实现一条街，因为虚函数是指针直接定位，要优于Objective-C的Selector选择器的字符串查找SEL函数表性能。C中既没有底层解释环境也没有虚函数表和SEL表，如何来实现多态呢？

>> 函数指针！万能的指针！


``` C
typedef struct Mu
{
    void (* func)();
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
my_mu.func = test_a;

my_mu.func();

my_mu.func = test_b;

```
多态就这么容易的实现了，没有虚函数表的性能损耗, 世界是那么的清爽。


### OOP

类的概念和对象的概念在很多OOP语言中感觉已经被很多人所混淆，而C来实现却需要分清楚，并且基于此将OOP的更低层次的一个概念隔离变化。

基于对象和面向对象的区别也许是在于是否有类的定义和实例化吧。好吧，那我们来实现类和对象，然后来实例化它们。

#### 定义类

``` C
typedef struct Class
{
   GObjectClass base;  
}base_class_t;
```

#### 定义对象

``` C
typedef struct Object
{
    base_class_t *class;
}base_object_t;
```

#### 实例化

``` C
base_object_t *Object_new()
{
   static base_class_t *class = (base_class_t *)malloc(sizeof(base_class_t));
   base_object_t *object = (base_object_t *)malloc(sizeof(base_object_t));
   object->class = class;

   return object;
}
```

当第一实例化时会分配Class一次，然后每次实例化都会分配Objct。Gtk 有更优雅的实现，但是封装层级有点高，就不展开了，直接展示自己理解的部分。

### 封装

其他面向对象的特性实现起来感觉很简单，但是 `封装` 这个概念如果说他是 `public`, `private`, `protected`的话，那真的不好去实现，我们顶多是不把对应的struct定义放到.h中来达到隐藏的效果。但是 `封装` 的概念不仅仅是隐藏，更是包裹不变，提供对变化的扩展。Gtk通过各种奇巧淫技来达到private的目的，个人感觉问题复杂化了。

Gtk 提供了不少的宏来简化我们的工作，当然为了更好的理解C来写面向对象的原理就不深究了。

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

如何高效的实现一个Fabic表达式？好吧, C++11就这么简单,并且结果是在编译期就已经推演出来了，运行时直接得到结果，这运行期的性能～～～

``` c++
template<>
class Fibonacci<1> {
public:
     static int value;
};
int Fibonacci<1>::value = 1 ;

template<>
class Fibonacci<0> {
public:
    static int value;
};
int (Fibonacci<0>::value) = 0;

template<int N>
class Fibonacci {
public:
    static int value;
};
template<int N> int Fibonacci<N>::value = Fibonacci<N-1>::value +  Fibonacci<N-2>::value;

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


个人参考C++11标准废弃的很多特性足以彻底改变C++语言(比如模块、概念)，但是正是因为改变所以被废弃掉了。C++真正的活力不在于它是多么的现代化，而在于它是多范式的，你既可以用面向过程的C来写代码（对C99的支持还有待提高啊），也可以用模板机制来写泛型代码，更可以在C++11以后写出泛函的代码。八卦一下，golang就是因为它爹因为要把很多特性提交到C++标准中没有被通过所有就写了一个golang~~~。


资源推荐
----------
[cpp reference](http://www.cppreference.com): c++函数查询  
[cplusplus](http://www.cplusplus.com): c++官网

一段代码
--------

``` c++
pthread_t current_thread;
std::function<void *(void *)> func = std::bind(fu_, this, _1);
pthread_create(&current_thread, NULL, func, NULL);

```

Tip
-------

### vim

`Ctrl+o`、`Ctrl+[`、`Ctrl+c` 都可以替代 `Esc` 来退出插入模式

### emacs

`speedbar`默认是在frame意外打开的，`sr-speedbar`插件可以将其集成到frame中

### shell

`Ctrl+x Ctrl+e` 可以快速打开编辑器

 
- - -
欢迎群成员自荐自己的blog文章和收集的资源，发[邮件](mailto:cnsworder@gmail.com)给我，如果有意见或建议都可以mail我。  
如果无法直接在邮件内查看，请访问[github上的页面](https://github.com/cnsworder/publication/blob/master/alpha7.md)或[网站](http://ssh.cnsworder.com/)。  
我们在github上开放编辑希望大家能参与到其中。
