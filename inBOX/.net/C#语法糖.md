# C#语法糖

[语法糖](https://link.jianshu.com/?t=http://baike.baidu.com/view/1805428.htm)(Syntactic sugar)，也译为糖衣语法，是由英国计算机科学家彼得·约翰·兰达（Peter J. Landin）发明的一个术语，指计算机语言中添加的某种语法，这种语法对语言的功能并没有影响，但是更方便程序员使用。通常来说使用语法糖能够增加程序的可读性，从而减少程序代码出错的机会。需要声明的是“语法糖”这个词绝非贬义词，它可以给我们带来方便，是一种便捷的写法，编译器会帮我们做转换；而且可以提高开发编码的效率，在性能上也不会带来损失。

* 1.简化属性  
  未简化的写法

```csharp
        private string _Name;
        public string Name
        {
            get { return _Name; }
            private set { _Name = value; }
        }

        private int _Age;
        public int Age
        {
            get { return _Age; }
            private set { _Age = value; }
        }
```

简化之后的的写法

```csharp
    public string Name { get; set; }
    public int Age { get; private set; }
```

* 2.委托  
  在.net 1.1时我们不得不声明方法后才在委托中使用，在.net 2.0之后我们可以使用匿名委托，他不单可以简化写法，还可以在匿名委托中访问范围内的变量；再后来Lambda表达式来了，写法就更简便了。

```csharp
  class MyClass
    {
        //定义委托
        public delegate void TestDelegate(string str);
        //义委托方法
        public void Method(string str)
        {
            Console.WriteLine(str);
        }

        public void UseDelegate(TestDelegate d, string str)
        {
            d(str);
        }
    }
//调用委托
            MyClass mc = new MyClass();
            //调用定义的委托方法
            mc.UseDelegate(new MyClass.TestDelegate(mc.Method), "Hello!");
            //使用匿名委托
            mc.UseDelegate(delegate(string str)
            {
                Console.WriteLine(str);
            }, "Hello!");
            //使用Lambda表达式
            mc.UseDelegate(s =>
            {
                Console.WriteLine(s);
            }, "Hello!");
```

* 3.集合  
  1.初始化List集合的值  
  未简化的写法

```cpp
            List<string> ListString = new List<string>();
            ListString.Add("a");
            ListString.Add("b");
            ListString.Add("c");
```

简化之后的写法

```cpp
            List<string> ListString = new List<string>() 
            {
                "a",
                "b",
                "c"
            };
```

2.取List中的值  
未简化的写法

```csharp
      foreach (string str in ListString)
            {
                Console.WriteLine(str);
            }
```

简化之后的写法

```jsx
ListString.ForEach(s => Console.WriteLine(s));
```

* 4.使用完毕后自动释放资源(Using || try finally)  
  为了节约资源，每次使用完毕后都要释放掉资源，其中可以使用Using和try finally来进行释放资源操作。需要注意的是使用Using释放资源的对象都必须继承IDisposable接口（[MSDN](https://link.jianshu.com/?t=http://msdn.microsoft.com/zh-cn/library/bb383973.aspx)）。  
  使用try Finally写法

```csharp
            SqlConnection conn = null;
            try
            {
                conn = new SqlConnection("数据库连接字符串");
                conn.Open();
            }
            finally
            {
                conn.Close();
                conn.Dispose();
            }
```

使用Using写法

```cpp
 using (SqlConnection conn=new SqlConnection("数据库连接字符串"))
{
      conn.Open();
}
```

* 5.var隐式类型  
  从 Visual C# 3.0 开始，在方法范围中声明的变量可以具有隐式类型 var.隐式类型的本地变量是强类型变量(就好像您已经声明该类型一样)，但由编译器确定类型。([MSDN](https://link.jianshu.com/?t=http://msdn.microsoft.com/zh-cn/library/bb383973.aspx))

```csharp
foreach (var item in collection) 
{ 
}
```

* 6.问号(?)表达式  
  (?:)问号加冒号的形式

```cpp
string sex = "男";
int result = sex == "男" ? 1 : 0;//如果sex等于“男”result等于1，否则result等于0.
```

(??)两个问号的形式

```csharp
string sex = null;
string s = sex ?? "未知";//左边的变量如果为null则值为右边的变量，否则就是左边的变量值
```

* 7.类型实例化

```csharp
class User
    {
        public int ID { get; set; }
        public string Name { get; set; }
    }
```

未简化的写法

```cpp
User u = new User();
u.ID = 1;
u.Name = "PanPan";
```

简化之后的写法

```cpp
User u = new User() 
{
    ID=1,
    Name="PanPan"
};
```

* 8.扩展方法  
  扩展方法使你能够向现有类型“添加”方法，而无需创建新的派生类型、重新编译或以其他方式修改原始类型。 扩展方法是一种特殊的静态方法，但可以像扩展类型上的实例方法一样进行调用。 对于用 C# 和 Visual Basic 编写的客户端代码，调用扩展方法与调用在类型中实际定义的方法之间没有明显的差异。 ([MSDN](https://link.jianshu.com/?t=http://msdn.microsoft.com/zh-cn/library/bb383977.aspx))  
  <b>注意：定义扩展方法的类必须和使用地方的类命名空间相同(如果不同命名空间记得先引入命名空间)</b>

```cpp
//定义扩展方法
    public static class StringExtensions
    {
        public static bool IsEmpty(this string str)
        {
            return string.IsNullOrEmpty(str);
        }
    }
//调用扩展方法
     string temp="123";
     bool result = temp.IsEmpty();
```

* 9.匿名类  
  匿名类型提供了一种方便的方法，可用来将一组只读属性封装到单个对象中，而无需首先显式定义一个类型。 类型名由编译器生成，并且不能在源代码级使用。 每个属性的类型由编译器推断。可通过使用 new 运算符和对象初始值创建匿名类型。([MSDN](https://link.jianshu.com/?t=https://msdn.microsoft.com/zh-cn/library/bb397696(v=vs.110).aspx))

```csharp
var NoName = new { Name="PanPan",Age=20 };
```

* 10.参数默认值  
  定义方法时设置参数默认值；调用方法时指定参数赋值；

```cpp
//定义方法
private void haha(bool bol=false, int ab=1)
{
}
//调用方法
haha(bol: true);
```

* 11.Dictionary初始化赋值的新语法

```cpp
Dictionary<string, string> dic = new Dictionary<string, string>() 
{
      {"1","admin"},{"2","PanPan"}
};
```

* 12.dynamic动态对象  
  .net4.0中引入了一个新类型 dynamic.该类型是一种静态类型，但类型为 dynamic 的对象会跳过静态类型检查.大多数情况下，该对象就像具有类型 object 一样.在编译时，将假定类型化为 dynamic 的元素支持任何操作([MSDN](https://link.jianshu.com/?t=https://msdn.microsoft.com/zh-cn/library/dd264736.aspx))。

```csharp
dynamic dy = "string";
Console.WriteLine(dy.GetType());
//输出：System.String
dy = 100;
Console.WriteLine(dy.GetType());
//输出：System.Int32
```

> END
