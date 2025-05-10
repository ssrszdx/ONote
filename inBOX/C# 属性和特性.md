# C# 属性和特性

首先要说的是，可能一些刚接触C#的朋友常常容易把属性（Property）跟特性（Attribute）弄混淆，其实这是两种不同的东西。属性就是面向对象思想里所说的封装在类里面的数据字段，其形式为：

   1: public class HumanBase

   2: {

   3:     public string Name { get; set; }

   4:     public int Age { get; set; }

   5:     public int Gender { get; set; }

   6: }

在HumanBase这个类里出现的字段都叫属性（Property），而特性（Attribute）又是怎样的呢？

   1: [Serializable]

   2: public class HumanBase

   3: {

   4:     public string Name { get; set; }

   5:     public int Age { get; set; }

   6:     public int Gender { get; set; }

   7: }

简单地讲，我们在HumanBase类声明的上一行加了一个[Serializable]，这就是特性（Attribute），它表示HumanBase是可以被序列化的，这对于网络传输是很重要的，不过你不用担心如何去理解它，如何理解就是我们下面要探讨的。

C#的特性可以应用于各种类型和成员。前面的例子将特性用在类上就可以被称之为“类特性”，同理，如果是加在方法声明前面的就叫方法特性。无论它们被用在哪里，无论它们之间有什么区别，特性的最主要目的就是自描述。并且因为特性是可以由自己定制的，而不仅仅局限于.NET提供的那几个现成的，因此给C#程序开发带来了相当大的灵活性和便利。

我们还是借用生活中的例子来介绍C#的特性机制吧。

假设有一天你去坐飞机，你就必须提前去机场登机处换登机牌。登机牌就是一张纸，上面写着哪趟航班、由哪里飞往哪里以及你的名字、座位号等等信息，其实，这就是特性。它不需要你生理上包含这些属性（人类出现那会儿还没飞机呢），就像上面的HumanBase类没有IsSerializable这样的属性，特性只需要在类或方法需要的时候加上去就行了，就像你不总是在天上飞一样。

当我们想知道HumanBase是不是可序列化的，可以通过：

   1: static void Main(string[] args)

   2: {

   3:     Console.WriteLine(typeof(HumanBase).IsSerializable);

   4:

   5:     Console.ReadLine();

   6: }

拿到了登机牌，就意味着你可以合法地登机起飞了。但此时你还不知道你要坐的飞机停在哪里，不用担心，地勤人员会开车送你过去，但是他怎么知道你是哪趟航班的呢？显然还是通过你手中的登机牌。所以，特性最大的特点就是自描述。

既然是起到描述的作用，那目的就是在于限定。就好比地勤不会把你随便拉到一架飞机跟前就扔上去了事，因为标签上的说明信息就是起到限定的作用，限定了目的地、乘客和航班，任何差错都被视为异常。如果前面的HumanBase不加上Serializable特性就不能在网络上传输。

我们在顺带来介绍一下方法特性，先给HumanProperty加上一个Run方法：

   1: [Serializable]

   2: public class HumanPropertyBase

   3: {

   4:     public string Name { get; set; }

   5:     public int Age { get; set; }

   6:     public int Gender { get; set; }

   7:

   8:     public void Run(int speed)

   9:     {

  10:         // Running is good for health.

  11:     }

  12: }

只要是个四肢健全、身体健康的人就可以跑步，那这么说，跑步就是有前提条件的，至少是四肢健全，身体健康。由此可见，残疾人和老年人如果跑步就会出问题。假设一个HumanBase的对象代表的是一位耄耋老人，如果让他当刘翔的陪练，那就直接光荣了。如何避免这样的情况呢，我们可以在Run方法中加一段逻辑代码，先判断Age大小，如果小于2或大于60直接抛异常，但是2-60岁之间也得用Switch来分年龄阶段地判断speed参数是否合适，那么逻辑就相当臃肿。简而言之，如何用特性表示一个方法不能被使用呢？OK, here we go：

   1: [Obsolete("I'm so old, don't kill me!", true)]

   2: public virtual void Run(int speed)

   3: {

   4:     // Running is good for health.

   5: }

上面大致介绍了一下特性的使用与作用，接下来我们要向大家展示的是如何通过自定义特性来提高程序的灵活性，如果特性机制仅仅能使用.NET提供的那几种特性，不就太不过瘾了么。

首先，特性也是类。不同于其它类的是，特性都必须继承自System.Attribute类，否则编译器如何知道谁是特性谁是普通类呢。当编译器检测到一个类是特性的时候，它会识别出其中的信息并存放在元数据当中，仅此而已，编译器并不关心特性说了些什么，特性也不会对编译器起到任何作用，正如航空公司并不关心每个箱子要去哪里，只有箱子的主人和搬运工才会去关心这些细节。假设我们现在就是航空公司的管理人员，需要设计出前面提到的登机牌，那么很简单，我们先看看最主要的信息有哪些：

   1: public class BoardingCheckAttribute : Attribute

   2: {

   3:     public string ID { get; private set; }

   4:     public string Name { get; private set; }

   5:     public int FlightNumber { get; private set; }

   6:     public int PostionNumber { get; private set; }

   7:     public string Departure { get; private set; }

   8:     public string Destination { get; private set; }

   9: }

我们简单列举这些属性作为航空公司登机牌上的信息，用法和前面的一样，贴到HumanBase上就行了，说明此人具备登机资格。这里要简单提一下，你可能已经注意到了，在使用BoardingCheckAttribute的时候已经把Attribute省略掉了，不用担心，这样做是对的，因为编译器默认会自己加上然后查找这个属性类的。哦，等一下，我突然想起来他该登哪架飞机呢？显然，在这种需求下，我们的特性还没有起到应有的作用，我们还的做点儿工作，否则乘客面对一张空白的机票一定会很迷茫。

于是，我们必须给这个特性加上构造函数，因为它不仅仅表示登机的资格，还必须包含一些必要的信息才行：

   1: public BoardingCheckAttribute(string id, string name, string flightNumber, string positionNumber, string departure, string destination)

   2: {

   3:     this.ID = id;

   4:     this.Name = name;

   5:     this.FlightNumber = flightNumber;

   6:     this.PositionNumber = positionNumber;

   7:     this.Departure = departure;

   8:     this.Destination = destination;

   9: }

OK，我们的乘客就可以拿到一张正式的登机牌登机了，have a good flight！

   1: static void Main(string[] args)

   2: {

   3:     BoardingCheckAttribute boardingCheck = null;

   4:     object[] customAttributes = typeof(HumanPropertyBase).GetCustomAttributes(true);

   5:

   6:     foreach (var attribute in customAttributes)

   7:     {

   8:         if (attribute is BoardingCheckAttribute)

   9:         {

  10:             boardingCheck = attribute as BoardingCheckAttribute;

  11:

  12:             Console.WriteLine(boardingCheck.Name

  13:                         + "'s ID is "

  14:                         + boardingCheck.ID

  15:                         + ", he/she wants to "

  16:                         + boardingCheck.Destination

  17:                         + " from "

  18:                         + boardingCheck.Departure

  19:                         + " by the plane "

  20:                         + boardingCheck.FlightNumber

  21:                         + ", his/her position is "

  22:                         + boardingCheck.PositionNumber

  23:                         + ".");

  24:         }

  25:     }

  26:

  27:     Console.ReadLine();

  28: }

来源： [https://www.cnblogs.com/wangluochong/p/3733879.html](https://www.cnblogs.com/wangluochong/p/3733879.html)
