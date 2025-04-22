# Byte和byte

写程序时，误把byte写作Byte，调试了许久，便将二者的区别及用法详细理解一遍

1：byte和Byte详解
byte是java的基本数据类型，存储整型数据，占据1个字节(8 bits)，能够存储的数据范围是-128～+127。

Byte是java.lang中的一个类，目的是为基本数据类型byte进行封装。

2：二者关系：
Byte是byte的包装类,就如同Integer和int的关系，

一般情况包装类用于泛型或提供静态方法，用于基本类型或字符串之间转换，建议尽量不要用包装类和基本类型之间运算,因为这样运算效率会很差的

3：封装的好处
封装有几种好处，比如：
3.1. Byte可以将对象的引用传递，使得多个function共同操作一个byte类型的数据，而byte基本数据类型是赋值之后要在stack(栈区域)进行存储的；
3.2. 在java中包装类，比较多的用途是用在于各种数据类型的转化中。
比如，现在byte要转为String
byte a=0;
String result=Integer.toString(a);
3.3使用泛型时

 List<Integer> nums;
1
这里<>需要类。如果你用int。它会报错的
————————————————
版权声明：本文为CSDN博主「国平's BLOG」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/weixin_44537194/article/details/87900610
