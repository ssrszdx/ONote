# Java 枚举(enum) 详解7种常见的用法

JDK1.5引入了新的类型——枚举。在 Java 中它虽然算个“小”功能，却给我的开发带来了“大”方便。

**用法一：常量**

在JDK1.5 之前，我们定义常量都是： public static final.... 。现在好了，有了枚举，可以把相关的常量分组到一个枚举类型里，而且枚举提供了比常量更多的方法。

Java代码

```java
public enum Color {  
  RED, GREEN, BLANK, YELLOW  
} 
```

## **用法二：switch**

JDK1.6之前的switch语句只支持int,char,enum类型，使用枚举，能让我们的代码可读性更强。

Java代码

```java
enum Signal {  
    GREEN, YELLOW, RED  
}  
public class TrafficLight {  
    Signal color = Useful , Fun and Inspiring Products;  
    public void change() {  
        switch (color) {  
        case RED:  
            color = Signal.GREEN;  
            break;  
        case YELLOW:  
            color = Useful , Fun and Inspiring Products;  
            break;  
        case GREEN:  
            color = Signal.YELLOW;  
            break;  
        }  
    }  
}  
```

## **用法三：向枚举中添加新方法**

如果打算自定义自己的方法，那么必须在enum实例序列的最后添加一个分号。

[Java 枚举(enum) 详解7种常见的用法](https://link.zhihu.com/?target=https%3A//blog.csdn.net/qq_27093465/article/details/52180865)

而且 Java 要求必须先定义 enum 实例。

Java代码

```java
public enum Color {  
    RED("红色", 1), GREEN("绿色", 2), BLANK("白色", 3), YELLO("黄色", 4);  
    // 成员变量  
    private String name;  
    private int index;  
    // 构造方法  
    private Color(String name, int index) {  
        this.name = name;  
        this.index = index;  
    }  
    // 普通方法  
    public static String getName(int index) {  
        for (Color c : Color.values()) {  
            if (c.getIndex() == index) {  
                return c.name;  
            }  
        }  
        return null;  
    }  
    // get set 方法  
    public String getName() {  
        return name;  
    }  
    public void setName(String name) {  
        this.name = name;  
    }  
    public int getIndex() {  
        return index;  
    }  
    public void setIndex(int index) {  
        this.index = index;  
    }  
}  
```

## **用法四：覆盖枚举的方法**

下面给出一个toString()方法覆盖的例子。

Java代码

```java
public enum Color {  
    RED("红色", 1), GREEN("绿色", 2), BLANK("白色", 3), YELLO("黄色", 4);  
    // 成员变量  
    private String name;  
    private int index;  
    // 构造方法  
    private Color(String name, int index) {  
        this.name = name;  
        this.index = index;  
    }  
    //覆盖方法  
    @Override  
    public String toString() {  
        return this.index+"_"+this.name;  
    }  
}  
```

## **用法五：实现接口**

所有的枚举都继承自java.lang.Enum类。由于Java 不支持多继承，所以枚举对象不能再继承其他类。

Java代码

```java
public interface Behaviour {  
    void print();  
    String getInfo();  
}  
public enum Color implements Behaviour{  
    RED("红色", 1), GREEN("绿色", 2), BLANK("白色", 3), YELLO("黄色", 4);  
    // 成员变量  
    private String name;  
    private int index;  
    // 构造方法  
    private Color(String name, int index) {  
        this.name = name;  
        this.index = index;  
    }  
//接口方法  
    @Override  
    public String getInfo() {  
        return this.name;  
    }  
    //接口方法  
    @Override  
    public void print() {  
        System.out.println(this.index+":"+this.name);  
    }  
}  
```

## 用法六：使用接口组织枚举

Java代码

```java
public interface Food {  
    enum Coffee implements Food{  
        BLACK_COFFEE,DECAF_COFFEE,LATTE,CAPPUCCINO  
    }  
    enum Dessert implements Food{  
        FRUIT, CAKE, GELATO  
    }  
}  
    /**
     * 测试继承接口的枚举的使用（by 大师兄 or 大湿胸。）
     */
    private static void testImplementsInterface() {
        for (Food.DessertEnum dessertEnum : Food.DessertEnum.values()) {
            System.out.print(dessertEnum + "  ");
        }
        System.out.println();
        //我这地方这么写，是因为我在自己测试的时候，把这个coffee单独到一个文件去实现那个food接口，而不是在那个接口的内部。
        for (CoffeeEnum coffee : CoffeeEnum.values()) {
            System.out.print(coffee + "  ");
        }
        System.out.println();
        //搞个实现接口，来组织枚举，简单讲，就是分类吧。如果大量使用枚举的话，这么干，在写代码的时候，就很方便调用啦。
        //还有就是个“多态”的功能吧，
        Food food = Food.DessertEnum.CAKE;
        System.out.println(food);
        food = CoffeeEnum.BLACK_COFFEE;
        System.out.println(food);
    }
```

用法七：关于枚举集合的使用

java.util.EnumSet和java.util.EnumMap是两个枚举集合。EnumSet保证集合中的元素不重复；EnumMap中的 key是enum类型，而value则可以是任意类型。关于这个两个集合的使用就不在这里赘述，可以参考JDK文档。

关于枚举的实现细节和原理请参考：

参考资料：《ThinkingInJava》第四版

[http://](https://link.zhihu.com/?target=http%3A//softbeta.iteye.com/blog/1185573)​[**softbeta.iteye.com/blog**]()​[/1185573](https://link.zhihu.com/?target=http%3A//softbeta.iteye.com/blog/1185573)

## **例子解析：**

```java
package com.lxk.enumTest;
 
/**
 * Java枚举用法测试
 * <p>
 * Created by lxk on 2016/12/15
 */
public class EnumTest {
    public static void main(String[] args) {
        forEnum();
        useEnumInJava();
    }
 
    /**
     * 循环枚举,输出ordinal属性；若枚举有内部属性，则也输出。(说的就是我定义的TYPE类型的枚举的typeName属性)
     */
    private static void forEnum() {
        for (SimpleEnum simpleEnum : SimpleEnum.values()) {
            System.out.println(simpleEnum + "  ordinal  " + simpleEnum.ordinal());
        }
        System.out.println("------------------");
        for (TYPE type : TYPE.values()) {
            System.out.println("type = " + type + "    type.name = " + type.name() + "   typeName = " + type.getTypeName() + "   ordinal = " + type.ordinal());
        }
    }
 
    /**
     * 在Java代码使用枚举
     */
    private static void useEnumInJava() {
        String typeName = "f5";
        TYPE type = TYPE.fromTypeName(typeName);
        if (TYPE.BALANCE.equals(type)) {
            System.out.println("根据字符串获得的枚举类型实例跟枚举常量一致");
        } else {
            System.out.println("大师兄代码错误");
        }
 
    }
 
    /**
     * 季节枚举(不带参数的枚举常量)这个是最简单的枚举使用实例
     * Ordinal 属性，对应的就是排列顺序，从0开始。
     */
    private enum SimpleEnum {
        SPRING,
        SUMMER,
        AUTUMN,
        WINTER
    }
 
 
    /**
     * 常用类型(带参数的枚举常量，这个只是在书上不常见，实际使用还是很多的，看懂这个，使用就不是问题啦。)
     */
    private enum TYPE {
        FIREWALL("firewall"),
        SECRET("secretMac"),
        BALANCE("f5");
 
        private String typeName;
 
        TYPE(String typeName) {
            this.typeName = typeName;
        }
 
        /**
         * 根据类型的名称，返回类型的枚举实例。
         *
         * @param typeName 类型名称
         */
        public static TYPE fromTypeName(String typeName) {
            for (TYPE type : TYPE.values()) {
                if (type.getTypeName().equals(typeName)) {
                    return type;
                }
            }
            return null;
        }
 
        public String getTypeName() {
            return this.typeName;
        }
    }
}
```

enum这个关键字，可以理解为跟class差不多，这也个单独的类。可以看到，上面的例子里面有属性，有构造方法，有getter，也可以有setter，但是一般都是构造传参数。还有其他自定义方法。那么在这些东西前面的，以逗号隔开的，最后以分号结尾的，这部分叫做，这个枚举的实例。也可以理解为，class new 出来的实例对象。这下就好理解了。只是，class，new对象，可以自己随便new，想几个就几个，而这个enum关键字，他就不行，他的实例对象，只能在这个enum里面体现。也就是说，他对应的实例是有限的。这也就是枚举的好处了，限制了某些东西的范围，举个栗子：一年四季，只能有春夏秋冬，你要是字符串表示的话，那就海了去了，但是，要用枚举类型的话，你在enum的大括号里面把所有的选项，全列出来，那么这个季节的属性，对应的值，只能在里面挑。不能有其他的。

上面的例子，就是根据typeName，你可以从那些例子里面挑选到唯一的一个TYPE类型的枚举实例--TYPE.BALANCE。注意方法

TYPE type = TYPE.fromTypeName(typeName);

这个方法的返回类型就是这个TYPE枚举类型的。

枚举类型对象之间的值比较，是可以使用==，直接来比较值，是否相等的，不是必须使用equals方法的哟。

具体，请参考下面的链接：

java 枚举类比较是用==还是equals？

## 枚举例子：switch case

```text
    private static void testSwitchCase() {
        String typeName = "f5";
        //这几行注释呢，你可以试着三选一，测试一下效果。
        //String typeName = "firewall";
        //String typeName = "secretMac";
        TypeEnum typeEnum = TypeEnum.fromTypeName(typeName);
        if (typeEnum == null) {
            return;
        }
        switch (typeEnum) {
            case FIREWALL:
                System.out.println("枚举名称(即默认自带的属性 name 的值)是：" + typeEnum.name());
                System.out.println("排序值(默认自带的属性 ordinal 的值)是：" + typeEnum.ordinal());
                System.out.println("枚举的自定义属性 typeName 的值是：" + typeEnum.getTypeName());
                break;
            case SECRET:
                System.out.println("枚举名称(即默认自带的属性 name 的值)是：" + typeEnum.name());
                System.out.println("排序值(默认自带的属性 ordinal 的值)是：" + typeEnum.ordinal());
                System.out.println("枚举的自定义属性 typeName 的值是：" + typeEnum.getTypeName());
                break;
            case BALANCE:
                System.out.println("枚举名称(即默认自带的属性 name 的值)是：" + typeEnum.name());
                System.out.println("排序值(默认自带的属性 ordinal 的值)是：" + typeEnum.ordinal());
                System.out.println("枚举的自定义属性 typeName 的值是：" + typeEnum.getTypeName());
                break;
            default:
                System.out.println("default");
        }
    }
```

看完这个枚举，你要懂个概念，那就是，这个枚举，他是个对象，就像你定义的Student类，Person类，等等一些个类一样。

要有这么个概念。只要是个类，他就可以有构造函数，可以有属性，可以有方法。

然后，你就看到，这个地方有2个默认的属性，一个是name，一个是ordinal，这2个属性就像你定义Student类和Person类的name和age一样，只不过，这2个是系统自带的属性，不用你自己去定义啦。你也可以给这个枚举类，也就是你自己声明的枚举，随便加属性。上面代码例子里面的那个TypeEnum那个枚举，就是这么干的，就简单的添加了个自定义属性typeName，虽然他有自己的name了，那姑且叫我这个自定义的属性叫别名吧。可以看到例子里面就是通过自己写的那个构造方法给我这个自定义的属性初始化值的。

还有，这个构造方法是不可以，也不被运行public的。

还有，你不能对系统自带的name属性，在构造函数里面赋值，没有为什么。

还有，这个枚举类型，一旦创建，且被使用（比如，存数据库啥的）之后，持久化后的对象信息里面就保存了这个枚举信息。这个时候你的需求或者要求啥的，需要变更这个枚举名称。应当禁止这个变更的操作，只能重新创建，用新的代替旧的，不能直接把旧的给改了，因为，就数据在逆转成对象的时候，如果，旧的枚举不在了，那么就会400还是500的报错或者是空指针的bug。这也是需要关注的一个问题。希望注意下，不然等到出bug了再想到这个问题，就不好了。

版权声明：本文为CSDN博主「李学凯」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。

原文链接：[Java 枚举(enum) 详解7种常见的用法](https://link.zhihu.com/?target=https%3A//blog.csdn.net/qq_27093465/article/details/52180865)
