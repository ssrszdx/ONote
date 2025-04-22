# virtual, abstract 抽象方法和虚方法

1、虚方法必须有实现部分，抽象方法不可以有实现部分；
2、虚方法可以在派生类中重写也可以不重写，抽象方法必须在派生类中重写
3、虚方法可以在任何非密封类中声明，抽象方法只能在[抽象类](https://zhidao.baidu.com/search?word=%B3%E9%CF%F3%C0%E0&fr=iknow_pc_qb_highlight)中声明。
4、如果类包含抽象方法，那么该类也必须为抽象的，不能实例化。

相比而言，虚方法倾向于代码复用，抽象方法更类似一种规约来约束子类必须实现某方法。

‍

**抽象类中的非抽象函数，非虚拟函数不能在子类中重写。**

举个例子（未必恰当、只为说明问题）：
比如有个基类“动物”；两个子类“狮子”、“青蛙”。
狮子捕猎：锁定目标、用牙齿和[利爪](https://zhidao.baidu.com/search?word=%C0%FB%D7%A6&fr=iknow_pc_qb_highlight)抓获；
狮子说话：噢呜；
青蛙捕猎：锁定目标、用舌头抓获；
青蛙说话：呱呱；

对于捕猎，他们有共性也有区别：
所以就可以把捕猎声明为虚方法，基类里实现共性部分、各子类实现个性部分；
对于说话，完全不同，但是又必须让他们说话——否则成植物了，呵呵：
所以就可以把说话声明为抽象方法，基类只声明此方法来作为约束，强制子类实现。

‍

最显著的不同是虚方法有默认的实现，而抽象方法没有任何实现，只有输入参数和返回类型的定义；而且抽象方法不可以定义在一般类里面，只能被定义在抽象类里，而虚方法可以定义在一般类，甚至抽象类里。

在子类的[方法限定符](https://www.zhihu.com/search?q=%E6%96%B9%E6%B3%95%E9%99%90%E5%AE%9A%E7%AC%A6&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A1969737287%7D)虽然都有override，但是虚方法的override是可选的，而抽象方法的override是必须的，否则会有编译错误；也就是说，子类可以不必重写父类的虚方法，但必须实现抽象父类的所有抽象方法。

用法上，抽象方法和抽象类用于类型和模板的定义，主要用于[大型项目](https://www.zhihu.com/search?q=%E5%A4%A7%E5%9E%8B%E9%A1%B9%E7%9B%AE&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A1969737287%7D)和团队协作的场景，用于定义统一的[编程接口](https://www.zhihu.com/search?q=%E7%BC%96%E7%A8%8B%E6%8E%A5%E5%8F%A3&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A1969737287%7D)，抽象类和接口类一般会混合使用；因为项目的复杂度要求，抽象和接口就变得必要了。如果是小型项目或固定范围的项目，由于项目的设计和管理比较容易控制，大可不必使用抽象和接口这种比较复杂的[设计模式](https://www.zhihu.com/search?q=%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A1969737287%7D)，我们可以直接通过使用类的继承和重载（甚至多态）来实现项目的灵活设计和应用，虚方法就是为了很好地使用类的继承特性而生的。

再来比较***虚方法和非虚方法的不同***。

虚方法的关键字是Virtual，虚方法的访问限制必须是public或protected，不可以是静态方法、[私有方法](https://www.zhihu.com/search?q=%E7%A7%81%E6%9C%89%E6%96%B9%E6%B3%95&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A1969737287%7D)和抽象方法。虚方法可以有实现代码，使用上也可以像一般方法那样。虚方法可以被重写(new)或者重载(override)。

虚方法被重写(new)，指的是父类中的虚方法默认实现被完全抛弃，只保留了父类中虚方法的参数定义(Siganature: parameter name and parameter type)和[返回类型](https://www.zhihu.com/search?q=%E8%BF%94%E5%9B%9E%E7%B1%BB%E5%9E%8B&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A1969737287%7D)定义。

虚方法被重载(override)，指的是子类实现可以加入自己的应用逻辑，但也保留了继续调用父类中虚方法的默认实现的可能性。也就是说，你可以通过调用base.虚方法()的语句来调用父类默认实现。

举例：我们定义一系列水果的类型，定义一个吃法的虚方法。

```csharp
// 父类，基础类，包含虚方法
class 水果 {
    public virtual string 吃法() {
        return "直接用牙咬";
    }
}
// 子类，虚方法重载
class 苹果 : 水果 {
    public override string 吃法() {
        return "先洗干净，然后" + base.吃法(); //这里有扩展父类虚方法，不是完全取代  
    }  
}
// 子类，虚方法重载
class 橙子 : 水果 {
    public override string 吃法() {
        return "先剥皮，然后" + base.吃法(); //这里有扩展父类虚方法，不是完全取代  
    }
}
// 子类，虚方法重写
class   翔: 水果{
    public new string 吃法() {
       // base.吃法(); // 这里不能调用父类方法，会有编译错误。
        return "别吃！除非属狗"; //这里有覆盖父类虚方法  
    }  
}
```
