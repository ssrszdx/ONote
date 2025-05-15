# Servlet

很多人可能和我当初一样，把Servlet和太多东西联系起来。其实Servlet本身在Tomcat中是“非常被动”的一个角色，处理的事情也很简单。网络请求与响应，不是他的主要职责，它其实更偏向于业务代码。所谓的Request和Response是Tomcat传给它，用来处理请求和响应的工具，但它本身不处理这些。

文章会比较长，但是看完会拔高你看待Servlet的视角。

主要内容：
====

* Servlet的前世今生
* 我所理解的JavaWeb三大组件
* 如何编写一个Servlet

---

## Servlet的前世今生

类似于Servlet是**Ser**ver App**let**（运行在服务端的小程序）等其他博文已经提过的内容，这里就不重复了。它就是用来处理请求的业务逻辑的。

之前在[Tomcat外传](https://zhuanlan.zhihu.com/p/54121733)中我们聊过，所谓Tomcat其实是Web服务器和[Servlet容器](https://www.zhihu.com/search?q=Servlet%E5%AE%B9%E5%99%A8&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A690289895%7D)的结合体。

什么是Web服务器？

比如，我当前在杭州，你能否用自己的电脑访问我桌面上的一张图片？恐怕不行。我们太习惯通过URL访问一个网站、下载一部电影了。一个资源，如果没有[URL映射](https://www.zhihu.com/search?q=URL%E6%98%A0%E5%B0%84&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A690289895%7D)，那么外界几乎很难访问。而Web服务器的作用说穿了就是：将某个主机上的资源映射为一个URL供外界访问。

什么是Servlet容器？

Servlet容器，顾名思义里面存放着Servlet对象。我们为什么能通过Web服务器映射的URL访问资源？肯定需要写程序处理请求，主要3个过程：

* 接收请求
* 处理请求
* 响应请求

任何一个应用程序，必然包括这三个步骤。其中接收请求和响应请求是共性功能，且没有差异性。访问淘宝和访问京东，都是接收[http://www.](https://link.zhihu.com/?target=http%3A//www.taobao.com/brandNo%3D1)​[**taobao.com/brandNo\=1**](https://link.zhihu.com/?target=http%3A//www.taobao.com/brandNo%3D1)，响应给浏览器的都是JSON数据。于是，大家就把接收和响应两个步骤抽取成Web服务器：

​![](https://pic1.zhimg.com/80/v2-3d86f470ec1dc31bbe93d1df2c30fa47_720w.webp?source=1940ef5c)​

所有题主问题描述的网络请求啥的，其实是Tomcat的工作

但**处理请求的逻辑**是不同的。没关系，**抽取出来做成Servlet，交给程序员自己编写。**

当然，随着后期互联网发展，出现了三层架构，所以一些逻辑就从Servlet抽取出来，分担到Service和Dao。

​![](https://picx.zhimg.com/80/v2-f41587429ebde63225029b0c235960c1_720w.webp?source=1940ef5c)​

JavaWeb开发经典三层架构：代码分层，逻辑清晰，还解耦

但是Servlet并不擅长往浏览器输出HTML页面，所以出现了JSP。JSP的故事，我已经在[怎样学习JSP？](https://www.zhihu.com/question/23984162/answer/689106407)讲过了。

等Spring家族出现后，Servlet开始退居幕后，取而代之的是方便的SpringMVC。SpringMVC的核心组件DispatcherServlet其实本质就是一个Servlet。但它已经自立门户，在原来HttpServlet的基础上，又封装了一条逻辑。

总之，很多新手程序员框架用久了，甚至觉得SpringMVC就是SpringMVC，和Servlet没半毛钱关系。

---

## 我所理解的JavaWeb三大组件

不知道从什么时候开始，我们已经不再关心、甚至根本不知道到底谁调用了我写的这个程序，反正我写了一个类，甚至从来没new过，它就跑起来了...

我们把模糊的记忆往前推一推，没错，就是在学了Tomcat后！从Tomcat开始，我们再也没写过main方法。以前，一个main方法启动，程序间的调用井然有序，我们知道程序所有流转过程。

但是到了Javaweb后，Servlet/Filter/Listener一路下来我们越学越沮丧。没有main，也没有new，写一个类然后在web.xml中配个标签，它们就这么兀自运行了。

其实，这一切的一切，简单来说就是“注入”和“回调”。想象一下吧朋友们，Tomcat里有个main方法，假设是这样的：

​![](https://picx.zhimg.com/80/v2-ce6e39bb02e3c6a2f4eb1e5afaa6e4e6_720w.webp?source=1940ef5c)​

​![](https://picx.zhimg.com/80/v2-14c18b69b5fb642f8d56698d2df20171_720w.webp?source=1940ef5c)​

​![](https://pica.zhimg.com/80/v2-d473a8662d758859e75c3f9afce9e982_720w.webp?source=1940ef5c)​

其实，编程学习越往后越是如此，我们能做的其实很有限。大部分工作，框架都已经帮我们做了。只要我们实现xxx接口，它会帮我们创建实例，然后搬运（接口注入）到它合适的位置，然后一套既定的流程下来，肯定会执行到。我只能用中国一个古老的成语形容这种开发模式：闭门造车，出门合辙（敲黑板，成语本身是夸技术牛逼，而不是说某人瞎几把搞）。

很多时候，框架就像一个傀儡师，我们写的程序是傀儡，顶多就是给傀儡化化妆、打扮打扮，实际的运作全是傀儡师搞的。

​![](https://pic1.zhimg.com/80/v2-ad197d60b26731c743e17423a5adf919_720w.webp?source=1940ef5c)​

了解到这个层面后，JavaWeb三大组件任何生命周期相关的方法、以及调用时Tomcat传入的形参，这里就不再强调。肯定是程序的某处，在创建实例后紧接着就传入参数调用了呗。没啥神秘的。

---

## 如何编写一个Servlet

首先，我们心里必须有一个信念：我们都是菜鸡，框架肯定不会让我们写很难的代码。

所以Servlet既然交给我们实现，肯定是很简单的！

**（没有网络请求和响应需要我们处理，都封装好了！）**

你别不服。进入Tomcat阶段后，我们开始全面面向接口编程。但是“面向接口编程”这个概念，最早其实出现在JDBC阶段。我就问你，JDBC接口是你自己实现的吗？别闹了，你导入MySQL的驱动包，它给你搞定了一切。

真正的连接过程太难写了，朋友们。底层就是TCP连接数据库啊，你会吗？写Socket，然后进行[数据库校验](https://www.zhihu.com/search?q=%E6%95%B0%E6%8D%AE%E5%BA%93%E6%A0%A1%E9%AA%8C&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A690289895%7D)，最后返回Connection？这显然超出我们的能力范围了。我们这么菜，JDBC不可能让我们自己动手的。所以各大数据库厂商体贴地推出了驱动包，里面有个Driver类，调用

```text
driver.connect(url, username, password);
```

即可得到Connection。

BUT，这一次难得Tomcat竟然这么瞧得起我黄某 ，仅仅提供了javax.servlet接口，这是打算让我自己去实现？

不，不可能的，肯定是因为太简单了。

查看接口方法：

​![](https://pic1.zhimg.com/80/v2-cbb35ec0c92292ea599108e643fd5183_720w.webp?source=1940ef5c)​

五个方法，最难的地方在于形参，然而Tomcat会事先把形参对象封装好传给我...除此以外，既不需要我写TCP连接数据库，也不需要我解析HTTP请求，更不需要我把结果转成HTTP响应，request对象和[response对象](https://www.zhihu.com/search?q=response%E5%AF%B9%E8%B1%A1&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A690289895%7D)帮我搞定了。

看吧，Tomcat是不是把我们当成智障啊。

Tomcat之所以放心地交给我们实现，是因为Servlet里主要写的代码都是业务逻辑代码。和原始的、底层的解析、连接等没有丝毫关系。最难的几个操作，人家已经给你封装成形参传进来了。

也就是说，**Servlet虽然是个接口，但实现类只是个空壳，我们写点业务逻辑就好了。**

​![](https://pica.zhimg.com/80/v2-1a911529c489ebdcb2a17a8e19d87290_720w.webp?source=1940ef5c)​

总的来说，Tomcat已经替我们完成了所有“菜鸡程序员搞不定的骚操作”，并且传入三个对象：ServletConfig、ServletRequest、ServletResponse。接下来，我们看看这三个传进来都是啥。

**ServletConfig**

翻译过来就是“Servlet配置”。我们在哪配置Servlet来着？web.xml嘛。请问你会用dom4j解析xml得到对象吗？

可能...会吧，就是不熟练，嘿嘿嘿。

所以，Tomcat还真没错怪我们，已经帮“菜鸡们”搞掂啦：

​![](https://picx.zhimg.com/80/v2-3dd656100783b3e9e62621ad8e2e9b04_720w.webp?source=1940ef5c)​

也就是说，servletConfig对象封装了servlet的一些参数信息。如果需要，我们可以从它获取。

**Request/Response**

两位老朋友，不用多介绍了。也是题主最在意的网络请求相关的内容，其实是Tomcat处理的并封装好了的，不需要Servlet操心。很多人看待HTTP和Request/Response的眼光过于分裂。它们的关系就像菜园里的大白菜和餐桌上的酸辣白菜一样。HTTP请求到了Tomcat后，Tomcat通过字符串解析，把各个请求头（Header），[请求地址](https://www.zhihu.com/search?q=%E8%AF%B7%E6%B1%82%E5%9C%B0%E5%9D%80&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A690289895%7D)（URL），请求参数（QueryString）都封装进了Request对象中。通过调用

```text
request.getHeader();
request.getUrl()；
request.getQueryString();
...
```

等等方法，都可以得到浏览器当初发送的请求信息。

至于Response，Tomcat传给Servlet时，它还是空的对象。Servlet逻辑处理后得到结果，最终通过response.write()方法，将结果写入response内部的缓冲区。Tomcat会在servlet处理结束后，拿到response，遍历里面的信息，组装成HTTP响应发给客户端。

​![](https://pic1.zhimg.com/80/v2-7405fb1912570c73de8dd76da725b17c_720w.webp?source=1940ef5c)​

Servlet接口5个方法，其中[init](https://www.zhihu.com/search?q=init&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A690289895%7D)、service、destroy是生命周期方法。init和destroy各自只执行一次，即servlet创建和销毁时。而service会在每次有新请求到来时被调用。也就是说，我们主要的业务代码需要写在service中。

但是，浏览器发送请求最基本的有两种：Get/Post，于是我们必须这样写：

​![](https://picx.zhimg.com/80/v2-94e6aac29c7bb1353020d2df7f422d58_720w.webp?source=1940ef5c)​

很烦啊。有没有办法简化这个操作啊？我不想直接实现javax.servlet接口啊。

于是，菜鸡程序员找了下，发现了GenericServlet，是个**抽象类**。

​![](https://picx.zhimg.com/80/v2-b9f65e77009de2832d721cb28d5ae6f1_720w.webp?source=1940ef5c)​

我们发现GenericServlet做了以下改良：

* 提升了init方法中原本是形参的servletConfig对象的作用域（成员变量），方便其他方法使用
* init方法中还调用了一个[init空参方法](https://www.zhihu.com/search?q=init%E7%A9%BA%E5%8F%82%E6%96%B9%E6%B3%95&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A690289895%7D)，如果我们希望在servlet创建时做一些什么初始化操作，可以继承GenericServlet后，覆盖init空参方法
* 由于其他方法内也可以使用servletConfig，于是写了一个getServletContext方法
* service竟然没实现...要它何用

放弃GenericServlet。

于是我们继续寻找，又发现了HttpServlet：

​![](https://picx.zhimg.com/80/v2-869e7ef9c2c7aaf4b90572ca140bd1b4_720w.webp?source=1940ef5c)​

它继承了GenericServlet。

GenericServlet本身是一个抽象类，有一个抽象方法service。查看源码发现，HttpServlet已经实现了service方法：

​![](https://pic1.zhimg.com/80/v2-73b703e690ce018ffe88280376a67dc0_720w.webp?source=1940ef5c)​

好了，也就是说HttpServlet的service方法已经替我们完成了复杂的请求方法判断。

但是，我翻遍整个HttpServlet源码，都没有找出一个抽象方法。所以为什么HttpServlet还要声明成抽象类呢？

看一下HttpServlet的文档注释：

​![](https://pic1.zhimg.com/80/v2-b6602e7e7c24862f54041e3998bd7ee0_720w.webp?source=1940ef5c)​

一个类声明成抽象类，一般有两个原因：

* 有抽象方法
* 没有抽象方法，但是不希望被实例化

HttpServlet做成抽象类，仅仅是为了不让new。

​![](https://pic1.zhimg.com/80/v2-1c6bf76d5f9510e8dcd58fb6b870c95a_720w.webp?source=1940ef5c)​

所以构造方法不做任何事

它为什么不希望被实例化，且要求子类重写doGet、doPost等方法呢？

我们来看一下源码：

​![](https://pic1.zhimg.com/80/v2-ab9504d35eab343a522926bf18c3167b_720w.webp?source=1940ef5c)​

protected修饰，希望子类能重写

如果我们没重写会怎样？

​![](https://picx.zhimg.com/80/v2-fb86edf9aaac3049028e25fa022a9811_720w.webp?source=1940ef5c)​

浏览器页面会显示：405（http.method_get_not_supported）

也就是说，HttpServlet虽然在service中帮我们写了请求方式的判断。但是针对每一种请求，业务逻辑代码是不同的，HttpServlet无法知晓子类想干嘛，所以就抽出七个方法，并且提供了默认实现：报405、400错误，提示请求不支持。

但这种实现本身非常鸡肋，简单来说就是等于没有。所以，不能让它被实例化，不然调用doXxx方法是无用功。

Filter用到了责任链模式，Listener用到了[观察者模式](https://www.zhihu.com/search?q=%E8%A7%82%E5%AF%9F%E8%80%85%E6%A8%A1%E5%BC%8F&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A690289895%7D)，Servlet也不会放过使用设计模式的机会：模板方法模式。上面的就是。这个模式我在JDBC的博文里讲过了：[JDBC（中）](https://zhuanlan.zhihu.com/p/64749458)

​![](https://pic1.zhimg.com/80/v2-b33c2c238803958be0bfa70d0f40c211_720w.webp?source=1940ef5c)​

小结：

* 如何写一个Servet？
* 不用实现javax.servlet接口
* 不用继承GenericServlet抽象类
* 只需继承HttpServlet并重写doGet()/doPost()
* 父类把能写的逻辑都写完，把不确定的业务代码抽成一个方法，调用它。当子类重写该方法，整个业务代码就活了。这就是[模板方法模式](https://www.zhihu.com/search?q=%E6%A8%A1%E6%9D%BF%E6%96%B9%E6%B3%95%E6%A8%A1%E5%BC%8F&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A690289895%7D)

​![](https://picx.zhimg.com/80/v2-250e370a7548fa65ed70d73ff82f2829_720w.webp?source=1940ef5c)​

想看看Servlet与SpringMVC关系的朋友请戳：

[bravo1988：Servlet（下）](https://zhuanlan.zhihu.com/p/65658315)​[**622 赞同 · 53 评论**](https://zhuanlan.zhihu.com/p/65658315)​[文章](https://zhuanlan.zhihu.com/p/65658315)

最近新写的小册，图文并茂，通俗易懂：

[bravo1988：中级Java程序员如何进阶](https://zhuanlan.zhihu.com/p/212191791)​[**824 赞同 · 127 评论**](https://zhuanlan.zhihu.com/p/212191791)​[文章](https://zhuanlan.zhihu.com/p/212191791)

​![](https://pic1.zhimg.com/80/v2-4793b80b1febde96a5dd08f6a4f08dcf_720w.webp?source=1940ef5c)​

​![](https://pica.zhimg.com/80/v2-9b82e892c759c0868f2f0a13b98a5da8_720w.webp?source=1940ef5c)​

‍
