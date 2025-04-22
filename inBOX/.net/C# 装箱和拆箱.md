装箱（boxing）和拆箱（unboxing）是C#类型系统的核心概念.是不同于C与C++的新概念！,通过装箱和拆箱操作，能够在值类型和引用类型中架起一做桥梁.换言之,可以轻松的实现值类型与引用类型的互相转换,装箱和拆箱能够统一考察系统,任何类型的值最终都可以按照对象进行处理.   C#语言中的所有类型都是由基类System.Object继承过来的，包括最常用的基础类型：int, byte, short，bool等等，就是说所有的事物都是对象。如果申明这些类型得时候都在堆(HEAP)中分配内存，会造成极低的效率！(个中原因以及关于堆和栈得区别会在另一篇里单独得说说！).NET如何解决这个问题得了？正是通过将类型分成值型(value)和引用型(regerencetype)，C#中定义的值类型包括原类型（Sbyte、Byte、Short、Ushort、Int、Uint、Long、Ulong、Char、Float、Double、Bool、Decimal）、枚举(enum)、结构(struct)，引用类型包括：类、数组、接口、委托、字符串等。值型就是在栈中分配内存，在申明的同时就初始化，以确保数据不为NULL；引用型是在堆中分配内存，初始化为null，引用型是需要GARBAGE COLLECTION来回收内存的，值型不用，超出了作用范围，系统就会自动释放！下面就来说装箱和拆箱的定义！装箱就是隐式的将一个值型转换为引用型对象。比如：int i=0;Syste.Object obj=i;这个过程就是装箱！就是将i装箱！拆箱就是将一个引用型对象转换成任意值型！比如：int i=0;System.Object obj=i;int j=(int)obj;这个过程前2句是将i装箱，后一句是将obj拆箱！再写个代码，看看进行了几次装拆箱！int i=0;System.Object obj=i;Console.WriteLine(i+&quot;,&quot;+(int)obj);其中共发生了3次装箱和一次拆箱！##_##，看出来了吧？！第一次是将i装箱，第2次是输出的时候将i转换成string类型，而string类型为引用类型，即又是装箱，第三次装箱就是(int)obj的转换成string类型，装箱！拆箱就是(int)obj，将obj拆箱！！在C#中，将类和数组等都归为了引用型的，那么值类型和引用型有什么区别呢？  值类型的变量包含自身的数据，而引用类型的变量是指向数据的内存块的，并不是直接存放数据。对于值类型，每个变量都有一份自己的数据复制，对另一个值类型变量的操作并不影响这一个变量的值。

  而对于引用类型，两个变量有可能引用同一对象，因此对一个变量的操作会影响到另一个变量。

 Eg: 值类型

     (1) int  a=0;     

     (2) int  b=a;

     (3) int  b=10;       

（2）之后，a,b均为0，但是（3）之后，b=10, a=0; 对b的重新附值并不影响a

    引用类型：

    using System;

   class valueclass

  {

   public int value=0;

}

class text{

public static void main()

{

valueclass a=new  valueclass()

valueclass a=b;

b.value=10;

Console.WriteLine(“{0},{1}”,a.value,b.value);

}

}

输出结果：10，10             

就相当于指针，两个变量指向同一块内存数据，当一个变量对内存区数据改变之后，另一个变量指向的数据当然也会改变。
