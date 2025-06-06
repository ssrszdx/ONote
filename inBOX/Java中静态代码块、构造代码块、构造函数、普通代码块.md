# Java中静态代码块、构造代码块、构造函数、普通代码块

**目录**

* [1、静态代码块](https://www.cnblogs.com/ysocean/p/8194428.html#_label0)

  * [　　①、格式](https://www.cnblogs.com/ysocean/p/8194428.html#_label0_0)
  * [　　②、执行时机](https://www.cnblogs.com/ysocean/p/8194428.html#_label0_1)
  * [　　③、静态代码块的作用](https://www.cnblogs.com/ysocean/p/8194428.html#_label0_2)
  * [　　④、静态代码块不能存在任何方法体中](https://www.cnblogs.com/ysocean/p/8194428.html#_label0_3)
  * [　　⑤、静态代码块不能访问普通变量](https://www.cnblogs.com/ysocean/p/8194428.html#_label0_4)
* [2、构造代码块](https://www.cnblogs.com/ysocean/p/8194428.html#_label1)

  * [　　①、格式](https://www.cnblogs.com/ysocean/p/8194428.html#_label1_0)
  * [　　②、执行时机](https://www.cnblogs.com/ysocean/p/8194428.html#_label1_1)
  * [　　③、构造代码块的作用](https://www.cnblogs.com/ysocean/p/8194428.html#_label1_2)
* [3、构造函数 ](https://www.cnblogs.com/ysocean/p/8194428.html#_label2)
* [4、普通代码块](https://www.cnblogs.com/ysocean/p/8194428.html#_label3)
* [5、执行顺序](https://www.cnblogs.com/ysocean/p/8194428.html#_label4)
* [6、父类和子类执行顺序](https://www.cnblogs.com/ysocean/p/8194428.html#_label5)

---

　　在Java中，静态代码块、构造代码块、构造函数、普通代码块的执行顺序是一个笔试的考点，通过这篇文章希望大家能彻底了解它们之间的执行顺序。

[回到顶部](https://www.cnblogs.com/ysocean/p/8194428.html#_labelTop)

### 1、静态代码块

#### 　　①、格式

　　在java类中（方法中不能存在静态代码块）使用static关键字和{}声明的代码块：

|12345|`public` `class` `CodeBlock {``    ``static``{``        ``System.out.println(``"静态代码块"``);``    ``}``}`|
| -------| ----|

#### 　　②、执行时机

　　静态代码块在类被加载的时候就运行了，而且只运行一次，并且优先于各种代码块以及构造函数。如果一个类中有多个静态代码块，会按照书写顺序依次执行。后面在比较的时候会通过具体实例来证明。

#### 　　③、静态代码块的作用

　　一般情况下，如果有些代码需要在项目启动的时候就执行，这时候就需要静态代码块。比如一个项目启动需要加载的很多配置文件等资源，我们就可以都放入静态代码块中。

#### 　　④、静态代码块不能存在任何方法体中

　　这个应该很好理解，首先我们要明确静态代码块是在类加载的时候就要运行了。我们分情况讨论：

　　对于普通方法，由于普通方法是通过加载类，然后new出实例化对象，通过对象才能运行这个方法，而静态代码块只需要加载类之后就能运行了。

　　对于静态方法，在类加载的时候，静态方法也已经加载了，但是我们必须要通过类名或者对象名才能访问，也就是说相比于静态代码块，静态代码块是主动运行的，而静态方法是被动运行的。

　　不管是哪种方法，我们需要明确静态代码块的存在在类加载的时候就自动运行了，而放在不管是普通方法还是静态方法中，都是不能自动运行的。

#### 　　⑤、静态代码块不能访问普通变量

　　这个理解思维同上，普通变量只能通过对象来调用，是不能放在静态代码块中的。

[回到顶部](https://www.cnblogs.com/ysocean/p/8194428.html#_labelTop)

### 2、构造代码块

#### 　　①、格式

　　在java类中使用{}声明的代码块（和静态代码块的区别是少了static关键字）:

|12345678|`public` `class` `CodeBlock {``    ``static``{``        ``System.out.println(``"静态代码块"``);``    ``}``    ``{``        ``System.out.println(``"构造代码块"``);``    ``}``}`|
| ----------| ----|

#### 　　②、执行时机

　　构造代码块在创建对象时被调用，每次创建对象都会调用一次，但是优先于构造函数执行。需要注意的是，听名字我们就知道，构造代码块不是优先于构造函数执行，而是依托于构造函数，也就是说，如果你不实例化对象，构造代码块是不会执行的。怎么理解呢？我们看看下面这段代码：

|123456789101112|`public` `class` `CodeBlock {``    ``{``        ``System.out.println(``"构造代码块"``);``    ``}``    ` `    ``public` `CodeBlock(){``        ``System.out.println(``"无参构造函数"``);``    ``}``    ``public` `CodeBlock(String str){``        ``System.out.println(``"有参构造函数"``);``    ``}``}`|
| -----------------| -------|

　　我们反编译生成的class文件：

　　![](0.09789123758639873-20220901153753-8qh56jb.png)​

　　如果存在多个构造代码块，则执行顺序按照书写顺序依次执行。

#### 　　③、构造代码块的作用

 　　和构造函数的作用类似，都能对对象进行初始化，并且只要创建一个对象，构造代码块都会执行一次。但是反过来，构造函数则不一定每个对象建立时都执行（多个构造函数情况下，建立对象时传入的参数不同则初始化使用对应的构造函数）。

　　利用每次创建对象的时候都会提前调用一次构造代码块特性，我们可以做诸如统计创建对象的次数等功能。

[回到顶部](https://www.cnblogs.com/ysocean/p/8194428.html#_labelTop)

### 3、构造函数

　　1.构造函数的命名必须和类名完全相同。在java中普通函数可以和构造函数同名，但是必须带有返回值；

　　2.构造函数的功能主要用于在类的对象创建时定义初始化的状态。它没有返回值，也不能用void来修饰。这就保证了它不仅什么也不用自动返回，而且根本不能有任何选择。而其他方法都有返回值，即使是void返回值。尽管方法体本身不会自动返回什么，但仍然可以让它返回一些东西，而这些东西可能是不安全的；

　　3.构造函数不能被直接调用，必须通过new运算符在创建对象时才会自动调用；而一般的方法是在程序执行到它的时候被调用的；

　　4.当定义一个类的时候，通常情况下都会显示该类的构造函数，并在函数中指定初始化的工作也可省略，不过Java编译器会提供一个默认的构造函数.此默认构造函数是不带参数的。而一般的方法不存在这一特点；

[回到顶部](https://www.cnblogs.com/ysocean/p/8194428.html#_labelTop)

### 4、普通代码块

　　普通代码块和构造代码块的区别是，构造代码块是在类中定义的，而普通代码块是在方法体中定义的。且普通代码块的执行顺序和书写顺序一致。

|12345|`public` `void` `sayHello(){``    ``{``        ``System.out.println(``"普通代码块"``);``    ``}``}`|
| -------| ----|

[回到顶部](https://www.cnblogs.com/ysocean/p/8194428.html#_labelTop)

### 5、执行顺序

* 　**静态代码块&gt;构造代码块&gt;构造函数&gt;普通代码块**

|12345678910111213141516171819202122232425|`public` `class` `CodeBlock {``    ``static``{``        ``System.out.println(``"静态代码块"``);``    ``}``    ``{``        ``System.out.println(``"构造代码块"``);``    ``}``    ``public` `CodeBlock(){``        ``System.out.println(``"无参构造函数"``);``    ``}``    ` `    ``public` `void` `sayHello(){``        ``{``            ``System.out.println(``"普通代码块"``);``        ``}``    ``}``    ` `    ``public` `static` `void` `main(String[] args) {``        ``System.out.println(``"执行了main方法"``);``        ` `        ``new` `CodeBlock().sayHello();;``        ``System.out.println(``"---------------"``);``        ``new` `CodeBlock().sayHello();;``    ``}``}`|
| -------------------------------------------| ---------------|

**　　反编译生成的class文件：**

　　![](0.9842207481808447-20220901153753-uf8h4v0.png)​

　　**执行结果：**

 　　![](0.7610934353950174-20220901153753-8h2nz24.png)​

　　我们创建了两个匿名对象，但是静态代码块只是调用了一次。

[回到顶部](https://www.cnblogs.com/ysocean/p/8194428.html#_labelTop)

### 6、父类和子类执行顺序

　　对象的初始化顺序：

　　首先执行父类静态的内容，父类静态的内容执行完毕后，接着去执行子类的静态的内容，当子类的静态内容执行完毕之后，再去看父类有没有构造代码块，如果有就执行父类的构造代码块，父类的构造代码块执行完毕，接着执行父类的构造方法；父类的构造方法执行完毕之后，它接着去看子类有没有构造代码块，如果有就执行子类的构造代码块。子类的构造代码块执行完毕再去执行子类的构造方法。

　　总之一句话，静态代码块内容先执行，接着执行父类构造代码块和构造方法，然后执行子类构造代码块和构造方法。

　　父类：SuperClass.java

|12345678910111213|`package` `com.ys.extend;` `public` `class` `SuperClass {``    ``static``{``        ``System.out.println(``"父类静态代码块"``);``    ``}``    ``{``        ``System.out.println(``"父类构造代码块"``);``    ``}``    ``public` `SuperClass(){``        ``System.out.println(``"父类构造函数"``);``    ``}``}`|
| -------------------| -------|

　　子类：SubClass.java

|12345678910111213141516|`package` `com.ys.extend;` `public` `class` `SubClass<span> </span>``extends` `SuperClass{``    ``static``{``        ``System.out.println(``"子类静态代码块"``);``    ``}``    ` `    ``{``        ``System.out.println(``"子类构造代码块"``);``    ``}``    ` `    ``public` `SubClass(){``        ``System.out.println(``"子类构造函数"``);``    ``}``    ` `}`|
| -------------------------| -----------|

　　测试：

|12345|`public` `static` `void` `main(String[] args) {``    ``SubClass sb =<span> </span>``new` `SubClass();``    ``System.out.println(``"------------"``);``    ``SubClass sb1 =<span> </span>``new` `SubClass();``}`|
| -------| -------|

　　打印结果：

　　![](0.8112426043572917-20220901153753-3penxcq.png)​
