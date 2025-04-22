# Java中关于static语句块的理解

## 一、static块会在类被加载的时候执行且仅会被执行一次，一般用来初始化静态变量和调用静态方法。

### 实例一

```
public class A{
    String name;
    public A(String name){
        this.name = name;
    }
  
    //静态块
    static{
        System.out.println("static语句块执行");
    }
  
    public static void main(String args[]){
        A a = new A("张三");
        A b = new A("李四");
        A c = new A("王五");
    }
}
```

程序运行后只输出了一条“static语句块执行”语句，因为在Java虚拟机的生命周期中一个类只被加载一次，又因为静态块是伴随A类加载执行的，所以不管构建多少次A类的实例对象，静态块都会被执行且只执行一次。

---

### 实例二

```
class A{
    public A(){
        System.out.println("A在构造体内");
    }
    {System.out.println("A在普通块内");}
    static{
        System.out.println("A在static静态块内");
    }
}
​
public class B extends A{
    public B(){
        System.out.println("B在构造体内");
    }
    {System.out.println("B在普通块内");}
    static{
        System.out.println("B在static静态块内");
    }
  
    public static void main(String args[]){
        new B();
    }
}
```

输出结果如下：

```
A在static静态块内
B在static静态块内
A在普通块内
A在构造体内
B在普通块内
B在构造体内
```

## 二、Java 静态语句块、语句块、构造函数的执行顺序

```
class Parent{    
    static String name = "hello";    
    
    {    
        System.out.println("3 parent  block");    
    }    
    static {    
        System.out.println("1 parent static block");    
    }    
    
    public Parent(){    
        System.out.println("4 parent constructor");    
    }    
}    
    
class Child extends Parent{    
    static String childName = "hello";    
    
    {    
        System.out.println("5 child  block");    
    }    
    static {    
        System.out.println("2 child static block");    
    }  
    
    public Child(){    
        System.out.println("6 child constructor");    
    }    
}    
    
public class StaticIniBlockOrderTest {    
    
    public static void main(String[] args) {    
        new Child();//语句(*)    
    }    
}    
```

结果：

1 parent static block 2 child static block 3 parent block 4 parent constructor 5 child block 6 child constructor

执行 顺序：

1）执行父类静态的内容，父类静态的内容执行完毕后，接着去执行子类的静态的内容；

2）当子类的静态内容执行完毕之后，再去看父类有没有非静态代码块，如果有就执行父类的非静态代码块，父类的非静态代码块执行完毕，接着执行父类的构 造方法；

3）父类的构造方法执行完毕之后，它接着去看子类有没有非静态代码块，如果有就执行子类的非静态代码块。子类的非静态代码块执行完毕再去执行子类的构造方法。

总之一句话，静态代码块内容先执行，接着执行父类非静态代码块和构造方法，然后执行子类非静态代码块和构造方法。 **而且子类的构造方法，不管这个构造方法带不带参数，默认的它都会先去寻找父类的不带参数的构造方法。如果父类没有不带参数的构造方法，那么子类必须用supper关键子来调用父类带参数的构造方法，否则编译不能通过。**
