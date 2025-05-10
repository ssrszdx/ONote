# C# 字段和属性

类成员包括变量和方法。如果希望其他类能够访问[成员变量](https://www.zhihu.com/search?q=%E6%88%90%E5%91%98%E5%8F%98%E9%87%8F&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A%22607302216%22%7D)的值，就必须定义成公有的，而将变量设为公有public，那这个成员变量的就可以被任意访问(包括修改，读取)，这样不利于数据安全。 C#通过属性特性读取和写入字段（成员变量），而不直接直接读取和写入，以此来提供对类中字段的保护。属性可用于类内部封装字段。属性是C#​[面向对象技术](https://www.zhihu.com/search?q=%E9%9D%A2%E5%90%91%E5%AF%B9%E8%B1%A1%E6%8A%80%E6%9C%AF&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A%22607302216%22%7D)中封装性的体现。

### 属性和字段的区别：

* 属性是[逻辑字段](https://www.zhihu.com/search?q=%E9%80%BB%E8%BE%91%E5%AD%97%E6%AE%B5&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A%22607302216%22%7D)，是字段的扩展，并不占用实际的内存；而字段占用内存空间。（这是很重要的区别）
* 属性可以被其他类访问；而非public的字段不能被直接访问。
* 属性可以对接受的数据在范围上做限定；而字段不能。

**使用属性的情况：**

* 要求字段只能读或者只能写；
* 需要限制字段的取值范围；
* 在改变一个字段的值的时候希望改变对象的其它一些状态；

**使用字段的情况：**

* 允许自由读写；
* 取值范围只受数据类型约束而无其他任何特定限制；
* 值的变动不需要引发类中其它任何成员的相应变化。

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;

namespace ConsoleApplication1
{
    class Person
    {
        private string _name;
        private string _identificationID;
        private string _phoneNumber;

        public string Name { get; set; }                        //可读，可写
        public string IdentificationID { get; private set; }    //只读
        public string PhoneNumber
        {
            get
            {
                return _phoneNumber;
            }
            set
            {
                if (value.Length != 11)
                {
                    Console.WriteLine("手机号码应该为11位!");
                }
                else
                {
                    _phoneNumber = value;
                }
            }
        }
    }
    class Program
    {
        static void Main(string[] args)
        {
            Person cherry = new Person();
            cherry.Name = "Cherry";
            cherry.PhoneNumber = "12345678910";
            cherry.IdentificationID = "320000000000000000";  //报错，由于定义的是只读属性
        }
    }
}
```

总结：虽然在实际项目的开发过程中，[公共字段](https://www.zhihu.com/search?q=%E5%85%AC%E5%85%B1%E5%AD%97%E6%AE%B5&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A%22607302216%22%7D)和属性在合适的条件下都可以使用，但是我们应该尽可能的使用属性（[property](https://www.zhihu.com/search?q=property&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A%22607302216%22%7D)），而不是数据成员（field）；把所有的字段都设置为私有字段，如果要暴露它们，则把它们封装成属性，这也是微软推荐的方式。
