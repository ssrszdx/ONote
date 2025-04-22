# const readonly  static

平时在开发时经常会用到 const,readonly,static 关键字，可以肯定这些关键词是完全不同的概念，但有时候他们在用法上很相似以至于在场景中不知道选择哪一个，这篇文章我们就来讨论 C# 中的 const，static 和 readonly 关键词，放在一起比较一下看看如何选择。

## 理解 const

const 常用来定义一个常量，什么意思呢？就是这个常量在你程序的生命周期内都不会被改变，因此，必须在声明常量时为其赋值，从技术角度上来说：这个常量值又被称为 `编译时`​ 值，用 const 定义的变量又被称为 `编译时`​ 常量，值得注意的是: 这个常量是不可变的，也就是一旦被定义好之后不可以对其进行修改。

下面的代码片段展示了如何使用 const 去定义这个 `编译时`​ 常量。

```text
const string connectionString = "Specify your database connection string here.";
```

一定要记住，常量必须在定义的时候给它赋值，同时也要记住不可以用 const 定义 object 类型，因为它只支持 C# 的基元类型，比如：ints, floats, chars, booleans 和 strings，接下来通过一个例子来了解以下为啥不能用 object,考虑下面的 Author 类。

```text
    public class Author
    {
        public int Id { get; set; }
        public string FirstName { get; set; }
        public string LastName { get; set; }
        public string Address { get; set; }
    }
```

如果用 const 将 Author 定义为常量的话，编译器肯定是不同意的，如下图：

​![](https://pic1.zhimg.com/80/v2-cdab1607117a00b7bbeca9ab5ab2254c_1440w.webp)​

## 理解 readonly

只读关键词 readonly 常用于将一个变量或者一个对象设置为只读，意味着这个变量或者对象只能在 `类作用域`​ 或者 `构造函数`​ 中被第一次赋值，一旦被赋值后，你就不能通过任何方法对其修改，除了构造函数，接下来看一个例子，考虑下面的 `DbManager`​ 类。

```text
    public class DbManager
    {
        public readonly string connectionString ="Specify your database connection string here.";

        public DbManager()
        {
            connectionString = "You can reassign a value here.";
        }

        public void ReAssign()
        {
            connectionString = "This is not allowed";
        }
    }
```

上面的代码会编译报错，错误信息如下：

​![](https://pic3.zhimg.com/80/v2-dad8e5eed6acef528a37519c6095ee8a_1440w.webp)​

## 理解 static

static 关键词可用于 变量，方法，对象。不过值得注意的是： 类中的 static 成员只归属于类，而不是类实例，换句话说，可以直接使用类名来访问静态属性或者静态方法，而不是通过类实例访问,接下来考虑一下 Utility 类。

```text
    public class Utility
    {
        public static void SomeMethod()
        {
            //Write your code here
        }
    }
```

你不可以通过 类实例 去调用，否则编译器是不会放行的，如下图：

​![](https://pic1.zhimg.com/80/v2-36e2ebc644b3eb0e4075962cd88fcfdc_1440w.webp)​

正确的做法如下：

```text
Utility.SomeMethod();
```

同样的规则也适用于 类中的属性和字段，要想引用类中的静态成员，参考如下语法：

```text
ClassName.Member;
```

或者

```text
ClassName.Member();
```

构造函数也可以是静态的，它通常用于初始化类中的静态成员，但要注意静态构造函数中不接受任何参数。

## 总结

使用 const,readonly,static 的一些经验法则如下：

* const

如果`变量`​在应用程序的生命周期内不会被改变，请用 const。

* readonly

如果你不确定这个`变量`​后期是否要被修改，但又不希望其他的类碰它，请用 readonly。

* static

如果你希望类成员是属于类型而不是类型的实例，请用 static。
