# Spring学习总结

目录

* [一、Spring框架概述](https://www.cnblogs.com/best/p/5727935.html#_label0)

  * [1.1、资源](https://www.cnblogs.com/best/p/5727935.html#_lab2_0_0)
  * [1.2、Spring历史](https://www.cnblogs.com/best/p/5727935.html#_lab2_0_1)
  * [1.3、框架特征与功能](https://www.cnblogs.com/best/p/5727935.html#_lab2_0_2)
  * [1.4、Spring组成](https://www.cnblogs.com/best/p/5727935.html#_lab2_0_3)
  * [1.5、Spring Boot与Spring Cloud](https://www.cnblogs.com/best/p/5727935.html#_lab2_0_4)
* [二、IoC基础](https://www.cnblogs.com/best/p/5727935.html#_label1)
* [三、使用XML配置的方式实现IOC](https://www.cnblogs.com/best/p/5727935.html#_label2)
* [3.1、使用无参构造方法创建对象](https://www.cnblogs.com/best/p/5727935.html#_lab2_2_0)
* [3.2、使用有参构造方法创建对象](https://www.cnblogs.com/best/p/5727935.html#_lab2_2_1)
* [3.3、通过属性赋值](https://www.cnblogs.com/best/p/5727935.html#_lab2_2_2)
* [3.4、对象引用](https://www.cnblogs.com/best/p/5727935.html#_lab2_2_3)
* [3.5、对象作用域](https://www.cnblogs.com/best/p/5727935.html#_lab2_2_4)
* [3.6、延迟初始化bean](https://www.cnblogs.com/best/p/5727935.html#_lab2_2_5)
* [四、使用Spring注解配置IOC](https://www.cnblogs.com/best/p/5727935.html#_label3)
* [4.1、修改BookDAO](https://www.cnblogs.com/best/p/5727935.html#_lab2_3_0)
* [4.2、修改BookService](https://www.cnblogs.com/best/p/5727935.html#_lab2_3_1)
* [4.3、修改IOC配置文件IOCBeans02.xml](https://www.cnblogs.com/best/p/5727935.html#_lab2_3_2)
* [4.4、简单示例](https://www.cnblogs.com/best/p/5727935.html#_lab2_3_3)
* [4.5、作用域用scope来指定](https://www.cnblogs.com/best/p/5727935.html#_lab2_3_4)
* [4.5 、Lazy延迟初始化Bean](https://www.cnblogs.com/best/p/5727935.html#_lab2_3_5)
* [4.6、初始化回调注解与销毁回调注解](https://www.cnblogs.com/best/p/5727935.html#_lab2_3_6)
* [4.6.1、@PostConstruct](https://www.cnblogs.com/best/p/5727935.html#_label3_3_6_0)
* [4.6.2、@PreDestroy](https://www.cnblogs.com/best/p/5727935.html#_label3_3_6_1)
* [4.7、默认名称](https://www.cnblogs.com/best/p/5727935.html#_lab2_3_7)
* [4.6、小结](https://www.cnblogs.com/best/p/5727935.html#_lab2_3_8)
* [五、自动装配](https://www.cnblogs.com/best/p/5727935.html#_label4)
* [5.1、修改BookDAO](https://www.cnblogs.com/best/p/5727935.html#_lab2_4_0)
* [5.2、修改BookService](https://www.cnblogs.com/best/p/5727935.html#_lab2_4_1)
* [5.3、简单示例](https://www.cnblogs.com/best/p/5727935.html#_lab2_4_2)
* [5.4、@Qualifier指定名称](https://www.cnblogs.com/best/p/5727935.html#_lab2_4_3)
* [5.5、@Resource](https://www.cnblogs.com/best/p/5727935.html#_lab2_4_4)
* [5.6、多种注入方式](https://www.cnblogs.com/best/p/5727935.html#_lab2_4_5)
* [六、零配置实现IOC](https://www.cnblogs.com/best/p/5727935.html#_label5)
* [6.2、零配置，由注解指定实例](https://www.cnblogs.com/best/p/5727935.html#_lab2_5_0)
* [6.3、零配置，由方法指定实例](https://www.cnblogs.com/best/p/5727935.html#_lab2_5_1)
* [七、通过注解@Value获取properties配置](https://www.cnblogs.com/best/p/5727935.html#_label6)
* [7.1、使用XML实现](https://www.cnblogs.com/best/p/5727935.html#_lab2_6_0)
* [7.2、使用零配置注解方式实现](https://www.cnblogs.com/best/p/5727935.html#_lab2_6_1)
* [7.3、其它方式](https://www.cnblogs.com/best/p/5727935.html#_lab2_6_2)
* [八、示例下载](https://www.cnblogs.com/best/p/5727935.html#_label7)
* [九、视频](https://www.cnblogs.com/best/p/5727935.html#_label8)
* [十、作业](https://www.cnblogs.com/best/p/5727935.html#_label9)
* [十一、总结](https://www.cnblogs.com/best/p/5727935.html#_label10)

# 一、Spring框架概述

Spring是一个开源免费的框架，为了解决企业应用开发的复杂性而创建。Spring框架是一个轻量级的解决方案，可以一站式地构建企业级应用。Spring是模块化的，所以可以只使用其中需要的部分。可以在任何web框架上使用控制反转（IoC），也可以只使用Hibernate集成代码或JDBC抽象层。它支持声明式事务管理、通过RMI或web服务实现远程访问，并可以使用多种方式持久化数据。它提供了功能全面的MVC框架，可以透明地集成AOP到软件中。

![](0.022139952817137987-20220831215811-zhxo2qz.png)​

Spring被设计为非侵入式的，这意味着你的域逻辑代码通常不会依赖于框架本身。在集成层（比如数据访问层），会存在一些依赖同时依赖于数据访问技术和Spring，但是这些依赖可以很容易地从代码库中分离出来。

Spring框架是基于Java平台的，它为开发Java应用提供了全方位的基础设施支持，并且它很好地处理了这些基础设施，所以你只需要关注你的应用本身即可。

Spring可以使用POJO（普通的Java对象，plain old java objects）创建应用，并且可以将企业服务非侵入式地应用到POJO。这项功能适用于Java SE编程模型以及全部或部分的Java EE。

那么，做为开发者可以从Spring获得哪些好处呢？

不用关心事务API就可以执行数据库事务；

不用关心远程API就可以使用远程操作；

不用关心JMX API就可以进行管理操作；

不用关心JMS API就可以进行消息处理。

①JMX，Java Management eXtension，Java管理扩展，是一个为应用程序、设备、系统等植入管理功能的框架。JMX可以跨越一系列异构操作系统平台、系统体系结构和网络传输协议，灵活的开发无缝集成的系统、网络和服务管理应用。

②JMS，Java Message Service，Java消息服务，是Java平台上有关面向消息中间件(MOM)的技术规范，它便于消息系统中的Java应用程序进行消息交换,并且通过提供标准的产生、发送、接收消息的接口简化企业应用的开发。

一句话概括：**Spring是一个轻量级的控制反转(IoC)和面向切面(AOP)的容器（框架）。**

## 1.1、资源

官网： [http://spring.io](http://spring.io/)

文档： [https://docs.spring.io/spring/docs/current/spring-framework-reference/](https://docs.spring.io/spring/docs/current/spring-framework-reference/)、 [https://github.com/waylau/spring-framework-4-reference](https://github.com/waylau/spring-framework-4-reference)

中文帮助： [http://spring.cndocs.ml/](http://spring.cndocs.ml/)

框架下载地址： [http://repo.springsource.org/libs-release-local/org/springframework/spring/](http://repo.springsource.org/libs-release-local/org/springframework/spring/)

教程： [http://www.yiibai.com/spring](http://www.yiibai.com/spring)

Git： [https://github.com/spring-projects](https://github.com/spring-projects)

源码： [https://github.com/spring-projects/spring-framework](https://github.com/spring-projects/spring-framework)

Jar包: [https://github.com/spring-projects/spring-framework/releases](https://github.com/spring-projects/spring-framework/releases)

## 1.2、Spring历史

2002年，Rod Jahnson在《Expert One-on-One J2EE Design and Development》书中首次推出了Spring框架雏形interface21框架。

2004年3月24日，Spring框架以interface21框架为基础，经过重新设计，发布了1.0正式版。

从2004年3月到现在，已经经历了1.0、1.1、1.2、2.0、2.5、3.0、3.1几个主要的版本

3.2.0版发布 2013年5月5日13:53

3.2.10版发布 2014年7月15日23:58

3.2.9版发布 2014年5月20日12:22

4.0.0版发布 2013年12月12日07:50

4.0.1版发布 2014年1月28日20:55

4.1.6版发布 2015年3月25日16:40

4.2.2版发布 2015年10月15日12:57

4.2.5版发布 2016年2月25日09:28

4.3.5版发布 2016年12月21日11:34

4.3.6版发布 2017年1月25日14:05

4.3.8版发布 2017年4月18日13:49

4.3.9版发布 2017年6月7日19:29

5.0.0版发布 2017年9月28日11:28

5.0.1版发布 2017年10月24日15:14

## 1.3、框架特征与功能

 **轻量：** 从大小与开销两方面而言Spring都是轻量的。完整的Spring框架可以在一个大小只有1MB多的JAR文件里发布。并且Spring所需的处理开销也是微不足道的。此外，Spring是非侵入式的：典型地，Spring应用中的对象不依赖于Spring的特定类。

 **控制反转Ioc：** Spring通过一种称作控制反转（IoC）的技术促进了低耦合。当应用了IoC，一个对象依赖的其它对象会通过被动的方式传递进来，而不是这个对象自己创建或者查找依赖对象。你可以认为IoC与JNDI相反——不是对象从容器中查找依赖，而是容器在对象初始化时不等对象请求就主动将依赖传递给它。

 **面向切面Aop：** Spring提供了面向切面编程的丰富支持，允许通过分离应用的业务逻辑与系统级服务（例如审计（auditing）和事务（transaction）管理）进行内聚性的开发。应用对象只实现它们应该做的——完成业务逻辑——仅此而已。它们并不负责（甚至是意识）其它的系统级关注点，例如日志或事务支持。

![](0.7095767058197922-20220831215811-tmftz0f.png)​

 **容器：** Spring包含并管理应用对象的配置和生命周期，在这个意义上它是一种容器，你可以配置你的每个bean如何被创建——基于一个可配置原型（prototype），你的bean可以创建一个单独的实例或者每次需要时都生成一个新的实例——以及它们是如何相互关联的。然而，Spring不应该被混同于传统的重量级的EJB容器，它们经常是庞大与笨重的，难以使用。

 **框架：** Spring可以将简单的组件配置、组合成为复杂的应用。在Spring中，应用对象被声明式地组合，典型地是在一个XML文件里。Spring也提供了很多基础功能（事务管理、持久化框架集成等等），将应用逻辑的开发留给了你。

 **MVC：** Spring的作用是整合，但不仅仅限于整合，Spring 框架可以被看做是一个企业解决方案级别的框架，Spring MVC是一个非常受欢迎的轻量级Web框架。

所有Spring的这些特征使你能够编写更干净、更可管理、并且更易于测试的代码。它们也为Spring中的各种模块提供了基础支持。

## 1.4、Spring组成

Spring 框架是一个分层架构，由 7 个定义良好的模块组成。Spring 模块构建在核心容器之上，核心容器定义了创建、配置和管理 bean 的方式

![](0.15746354061179013-20220831215811-o8qbk5v.png)​

组成 Spring 框架的每个模块（或组件）都可以单独存在，或者与其他一个或多个模块联合实现。每个模块的功能如下：

* **核心容器** ：核心容器提供 Spring 框架的基本功能。核心容器的主要组件是 `BeanFactory`，它是工厂模式的实现。`BeanFactory` 使用 *控制反转* （IOC） 模式将应用程序的配置和依赖性规范与实际的应用程序代码分开。
* **Spring 上下文** ：Spring 上下文是一个配置文件，向 Spring 框架提供上下文信息。Spring 上下文包括企业服务，例如 JNDI、EJB、电子邮件、国际化、校验和调度功能。
* **Spring AOP** ：通过配置管理特性，Spring AOP 模块直接将面向方面的编程功能集成到了 Spring 框架中。所以，可以很容易地使 Spring 框架管理的任何对象支持 AOP。Spring AOP 模块为基于 Spring 的应用程序中的对象提供了事务管理服务。通过使用 Spring AOP，不用依赖 EJB 组件，就可以将声明性事务管理集成到应用程序中。
* **Spring DAO** ：JDBC DAO 抽象层提供了有意义的异常层次结构，可用该结构来管理异常处理和不同数据库供应商抛出的错误消息。异常层次结构简化了错误处理，并且极大地降低了需要编写的异常代码数量（例如打开和关闭连接）。Spring DAO 的面向 JDBC 的异常遵从通用的 DAO 异常层次结构。
* **Spring ORM** ：Spring 框架插入了若干个 ORM 框架，从而提供了 ORM 的对象关系工具，其中包括 JDO、Hibernate 和 iBatis SQL Map。所有这些都遵从 Spring 的通用事务和 DAO 异常层次结构。
* **Spring Web 模块** ：Web 上下文模块建立在应用程序上下文模块之上，为基于 Web 的应用程序提供了上下文。所以，Spring 框架支持与 Jakarta Struts 的集成。Web 模块还简化了处理多部分请求以及将请求参数绑定到域对象的工作。
* **Spring MVC 框架** ：MVC 框架是一个全功能的构建 Web 应用程序的 MVC 实现。通过策略接口，MVC 框架变成为高度可配置的，MVC 容纳了大量视图技术，其中包括 JSP、Velocity、Tiles、iText 和 POI。

Spring 框架的功能可以用在任何 J2EE 服务器中，大多数功能也适用于不受管理的环境。Spring 的核心要点是：支持不绑定到特定 J2EE 服务的可重用业务和数据访问对象。毫无疑问，这样的对象可以在不同 J2EE 环境 （Web 或 EJB）、独立应用程序、测试环境之间重用。

![](0.7907315524648784-20220831215811-3amv0l1.png)​

Spring是一个开源的框架，现在的Spring框架构成了一个体系平台，通过Spring的官方网站http://www.springsource.org可以了解到，围绕着Spring框架本身，还有许多其他优秀的项目:

SpringFramework(Core)：核心项目

Spring Web Flow：工作流项目

Spring Security：安全项目

Spring Batch：批量数据处理项目

Spring Android：Android系统支持项目

Spring Social：社交项目

## 1.5、Spring Boot与Spring Cloud

Spring Boot 是 Spring 的一套快速配置脚手架，可以基于Spring Boot 快速开发单个微服务，Spring Cloud是一个基于Spring Boot实现的云应用开发工具；Spring Boot专注于快速、方便集成的单个微服务个体，Spring Cloud关注全局的服务治理框架；Spring Boot使用了约束优于配置的理念，很多集成方案已经帮你选择好了，能不配置就不配置，Spring Cloud很大的一部分是基于Spring Boot来实现，Spring Boot可以离开Spring Cloud独立使用开发项目，但是Spring Cloud离不开Spring Boot，属于依赖的关系。

SpringBoot在SpringClound中起到了承上启下的作用，如果你要学习SpringCloud必须要学习SpringBoot。

![](0.6076079062190436-20220831215811-o7211ci.png)​

# 二、IoC基础

控制反转IoC(Inversion of Control)，是一种设计思想，DI(依赖注入)是实现IoC的一种方法，也有人认为DI只是IoC的另一种说法。没有IoC的程序中我们使用面向对象编程对象的创建与对象间的依赖关系完全硬编码在程序中，对象的创建由程序自己控制，控制反转后将对象的创建转移给第三方，个人认为所谓控制反转就是：获得依赖对象的方式反转了。

![](0.8172970960563752-20220831215811-wn5pqmf.png)​

IoC是Spring框架的核心内容，使用多种方式完美的实现了IoC，可以使用XML配置，也可以使用注解，新版本的Spring也可以零配置实现IoC。Spring容器在初始化时先读取配置文件，根据配置文件或元数据创建与组织对象存入容器中，程序使用时再从Ioc容器中取出需要的对象。  
​![](0.6726291959687456-20220831215811-i6nyotp.png)​

采用XML方式配置Bean的时候，Bean的定义信息是和实现分离的，而采用注解的方式可以把两者合为一体，Bean的定义信息直接以注解的形式定义在实现类中，从而达到了零配置的目的。

控制反转是一种通过描述（XML或注解）并通过第三方去生产或获取特定对象的方式。在Spring中实现控制反转的是IoC容器，其实现方法是依赖注入（Dependency Injection,DI）。

# **三、使用XML配置的方式实现IOC**

假设项目中需要完成对图书的数据访问服务，我们定义好了IBookDAO接口与BookDAO实现类

创建maven项目：

![](0.8132013671755829-20220831215811-7n3zgav.png)​

IBookDAO接口如下：

![复制代码](0.4830628130566632-20220831215811-kdz27gj.png)[&quot;复制代码&quot;]("复制代码")

```
package com.zhangguo.Spring051.ioc01;

/**
 * 图书数据访问接口
 */
public interface IBookDAO {
    /**
     * 添加图书
     */
    public String addBook(String bookname);
}
```

![复制代码](0.15885722206797093-20220831215811-wpgbk39.png)[&quot;复制代码&quot;]("复制代码")

BookDAO实现类如下：

![复制代码](0.5816299291160023-20220831215811-ay6bvzl.png)[&quot;复制代码&quot;]("复制代码")

```
package com.zhangguo.Spring051.ioc01;

/**
 * 图书数据访问实现类
 */
public class BookDAO implements IBookDAO {

    public String addBook(String bookname) {
        return "添加图书"+bookname+"成功！";
    }
}
```

![复制代码](0.3923636779102593-20220831215811-4edovn7.png)[&quot;复制代码&quot;]("复制代码")

Maven项目的pom.xml如下：

![复制代码](0.3287202507102238-20220831215811-7bxv8yo.png)[&quot;复制代码&quot;]("复制代码")

```
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>com.zhangguo</groupId>
  <artifactId>Spring051</artifactId>
  <version>0.0.1-SNAPSHOT</version>
  <packaging>jar</packaging>

  <name>Spring051</name>
  <url>http://maven.apache.org</url>

<properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <spring.version>4.3.0.RELEASE</spring.version>
    </properties>
    <dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <scope>test</scope>
            <version>4.10</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjweaver</artifactId>
            <version>1.8.9</version>
        </dependency>
        <dependency>
            <groupId>cglib</groupId>
            <artifactId>cglib</artifactId>
            <version>3.2.4</version>
        </dependency>
    </dependencies>
</project>
```

![复制代码](0.2611770139622762-20220831215811-kx73b2u.png)[&quot;复制代码&quot;]("复制代码")

业务类BookService如下：

![复制代码](0.64723127903492-20220831215811-ccc68ky.png)[&quot;复制代码&quot;]("复制代码")

```
package com.zhangguo.Spring051.ioc01;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

/**
 * 图书业务类
 */
public class BookService {
    IBookDAO bookDAO;
  
    public BookService() {
        //容器
        ApplicationContext ctx=new ClassPathXmlApplicationContext("IOCBeans01.xml");
        //从容器中获得id为bookdao的bean
        bookDAO=(IBookDAO)ctx.getBean("bookdao");
    }
  
    public void storeBook(String bookname){
        System.out.println("图书上货");
        String result=bookDAO.addBook(bookname);
        System.out.println(result);
    }
}
```

![复制代码](0.5562793067129741-20220831215811-tsui7ml.png)[&quot;复制代码&quot;]("复制代码")

 容器的配置文件IOCBeans01.xml如下：

![](0.4064969033955801-20220831215811-nbem1qd.png)​

![复制代码](0.04534612677828265-20220831215811-ftkbqe3.png)[&quot;复制代码&quot;]("复制代码")

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
    xmlns:p="http://www.springframework.org/schema/p"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="bookdao" class="com.zhangguo.Spring051.ioc01.BookDAO"></bean>
</beans>
```

![复制代码](0.3538633685379773-20220831215811-fi8f8nk.png)[&quot;复制代码&quot;]("复制代码")

测试类Test如下：

![复制代码](0.1865936995656603-20220831215811-pjbiak2.png)[&quot;复制代码&quot;]("复制代码")

```
package com.zhangguo.Spring051.ioc01;

public class Test {
    @org.junit.Test
    public void testStoreBook()
    {
        BookService bookservice=new BookService();
        bookservice.storeBook("《Spring MVC权威指南 第一版》");
    }
}
```

![复制代码](0.8772888703539501-20220831215811-tm3da2t.png)[&quot;复制代码&quot;]("复制代码")

运行结果：

![](0.5749176533708635-20220831215811-clgk6ut.png)​

## 3.1、使用无参构造方法创建对象

如下所示，则上下文会使用无参构造方法创建对象

![复制代码](0.14391027163442405-20220831215811-q0s7imp.png)[&quot;复制代码&quot;]("复制代码")

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="tom" class="spring02.Student"></bean>
</beans>
```

![复制代码](0.5204236834428344-20220831215811-yr2xvgk.png)[&quot;复制代码&quot;]("复制代码")

## **3.2、使用有参构造方法创建对象**

Person.java

![复制代码](0.6041442521027951-20220831215811-f4xz05q.png)[&quot;复制代码&quot;]("复制代码")

```
package spring02;

/**人*/
public abstract class Person {
    public String name;
}
```

![复制代码](0.8269221951979644-20220831215811-akxyp9j.png)[&quot;复制代码&quot;]("复制代码")

Student.java

![复制代码](0.5735861793363957-20220831215811-83ibxil.png)[&quot;复制代码&quot;]("复制代码")

```
package spring02;

/**学生*/
public class Student extends Person {
    /**身高*/
    public int height;
    /**有参构造方法*/
    public Student(String name,int height){
        this.name=name;
        this.height=height;
    }
    @Override
    public String toString() {
        return "Student{" + "height=" + height+",name="+name +'}';
    }
}
```

![复制代码](0.6504207799242538-20220831215811-ck6x931.png)[&quot;复制代码&quot;]("复制代码")

School.java

![复制代码](0.21229452986606168-20220831215811-48idcyy.png)[&quot;复制代码&quot;]("复制代码")

```
package spring02;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class School {
    public static void main(String[] args) {
        //IoC容器
        ApplicationContext ctx=
                new ClassPathXmlApplicationContext("bookbean01.xml","beans02.xml");
        //从容器中获取对象
        Person tom=ctx.getBean("tom",Person.class);

        System.out.println(tom);
    }
}
```

![复制代码](0.6040566087275596-20220831215811-8oboi6w.png)[&quot;复制代码&quot;]("复制代码")

配置文件beans02.xml

![复制代码](0.011157363120217534-20220831215811-u5um998.png)[&quot;复制代码&quot;]("复制代码")

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="tom" class="spring02.Student">
        <constructor-arg name="name" value="张柏川"></constructor-arg>
        <constructor-arg name="height" value="195"></constructor-arg>
    </bean>
</beans>
```

![复制代码](0.31608774118805294-20220831215811-f00clys.png)[&quot;复制代码&quot;]("复制代码")

运行结果：

![](0.09146423708473561-20220831215811-hj4su1h.png)​

注意：如果在使用构造方法时不想通过参数名称指定参数则可以直接使用索引，如：

```
    <bean id="rose" class="spring02.Student">
        <constructor-arg index="0" value="张柏芝"></constructor-arg>
        <constructor-arg index="1" value="196"></constructor-arg>
    </bean>
```

## 3.3、通过属性赋值

![](0.6755484689185698-20220831215811-q03no5p.png)​

Address地址类：

![复制代码](0.6906236084411401-20220831215811-a54osx7.png)[&quot;复制代码&quot;]("复制代码")

```
package spring02;

/**地址*/
public class Address {
    /**国家*/
    private String country;
    /**城市*/
    private String city;

    public Address() {
    }
    public Address(String country, String city) {
        this.country = country;
        this.city = city;
    }

    @Override
    public String toString() {
        return "Address{" +
                "country='" + country + '\'' +
                ", city='" + city + '\'' +
                '}';
    }

    public String getCountry() {
        return country;
    }

    public void setCountry(String country) {
        this.country = country;
    }

    public String getCity() {
        return city;
    }

    public void setCity(String city) {
        this.city = city;
    }
}
```

![复制代码](0.33182620318031764-20220831215811-u1t4e36.png)[&quot;复制代码&quot;]("复制代码")

配置文件beans02.xml：

```
    <bean name="zhuhai" class="spring02.Address">
        <property name="country" value="中国"></property>
        <property name="city" value="珠海"></property>
    </bean>
```

测试代码：

![复制代码](0.8894183928465522-20220831215811-jtt3x0i.png)[&quot;复制代码&quot;]("复制代码")

```
package spring02;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class School {
    public static void main(String[] args) {
        //IoC容器
        ApplicationContext ctx=
                new ClassPathXmlApplicationContext("bookbean01.xml","beans02.xml");
        //从容器中获取对象
        Person tom=ctx.getBean("rose",Person.class);
        Address zhuhai=ctx.getBean("zhuhai",Address.class);
        System.out.println(zhuhai);
    }
}
```

![复制代码](0.47125448797032954-20220831215811-ttjobnn.png)[&quot;复制代码&quot;]("复制代码")

运行结果：

![](0.3560923747956519-20220831215811-p0rr7z2.png)​

便捷方式：

![复制代码](0.9592887797236396-20220831215811-f933ur3.png)[&quot;复制代码&quot;]("复制代码")

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="mark" class="entities.Person" p:height="100" p:name="mark">
    </bean>
</beans>
```

![复制代码](0.29956197627764936-20220831215811-nbqj01w.png)[&quot;复制代码&quot;]("复制代码")

## 3.4、对象引用

你可以使用id 或（和） name 属性来指定bean的标识符

Person

![复制代码](0.8059405237622399-20220831215811-y2xh4jn.png)[&quot;复制代码&quot;]("复制代码")

```
package spring02;

/**人*/
public abstract class Person {
    /**姓名*/
    public String name;
    /**地址*/
    public Address address;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Address getAddress() {
        return address;
    }

    public void setAddress(Address address) {
        this.address = address;
    }
}
```

![复制代码](0.48785818480641807-20220831215811-pr3m5gi.png)[&quot;复制代码&quot;]("复制代码")

Student

![复制代码](0.5599732161122044-20220831215811-0ilxf2g.png)[&quot;复制代码&quot;]("复制代码")

```
package spring02;

/**学生*/
public class Student extends Person {
    /**身高*/
    public int height;
    /**有参构造方法*/
    public Student(String name,int height){
        this.name=name;
        this.height=height;
    }
    /**有参构造方法*/
    public Student(String name,int height,Address address){
        this.name=name;
        this.height=height;
        this.address=address;
    }
    @Override
    public String toString() {
        return "Student{" + "height=" + height+",name="+name +'}'+address;
    }
}
```

![复制代码](0.8862760388612729-20220831215811-6lznk8z.png)[&quot;复制代码&quot;]("复制代码")

配置文件

![复制代码](0.7621864231569266-20220831215811-x9i04x6.png)[&quot;复制代码&quot;]("复制代码")

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean name="zhuhai" class="spring02.Address">
        <property name="country" value="中国"></property>
        <property name="city" value="珠海"></property>
    </bean>
    <bean id="tom" class="spring02.Student">
        <constructor-arg name="name" value="张柏川"></constructor-arg>
        <constructor-arg name="height" value="195"></constructor-arg>
        <constructor-arg name="address" ref="zhuhai"></constructor-arg>
    </bean>
    <bean id="rose" class="spring02.Student">
        <constructor-arg index="0" value="张柏芝"></constructor-arg>
        <constructor-arg index="1" value="196"></constructor-arg>
        <constructor-arg index="2" ref="zhuhai"></constructor-arg>
    </bean>
</beans>
```

![复制代码](0.10632918720900486-20220831215811-xe0tydq.png)[&quot;复制代码&quot;]("复制代码")

测试代码：

![复制代码](0.9741060337262377-20220831215811-i11cqzd.png)[&quot;复制代码&quot;]("复制代码")

```
package spring02;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class School {
    public static void main(String[] args) {
        //IoC容器
        ApplicationContext ctx=
                new ClassPathXmlApplicationContext("bookbean01.xml","beans02.xml");
        //从容器中获取对象
        Person tom=ctx.getBean("tom",Person.class);
        Person rose=ctx.getBean("rose",Person.class);
        //Address zhuhai=ctx.getBean("zhuhai",Address.class);
        System.out.println(tom);
        System.out.println(rose);
    }
}
```

![复制代码](0.021165660328331004-20220831215811-pk8e66z.png)[&quot;复制代码&quot;]("复制代码")

运行结果： ![](0.3590657434356539-20220831215811-hybb7de.png)​

## 3.5、对象作用域

从容器中取回的对象默认是单例的：

![复制代码](0.7953564412500997-20220831215811-j9af5ak.png)[&quot;复制代码&quot;]("复制代码")

```
        Person roseA=ctx.getBean("rose",Person.class);
        Person roseB=ctx.getBean("rose",Person.class);
        //Address zhuhai=ctx.getBean("zhuhai",Address.class);
        System.out.println(tom);
        System.out.println(roseA==roseB);
```

![复制代码](0.1884227169245709-20220831215811-io53cbx.png)[&quot;复制代码&quot;]("复制代码")

运行结果：

![](0.8538780003472584-20220831215811-10n3a7e.png)​

使用scope属性可以指定作用域

![](0.1136754196569516-20220831215811-11as7pi.png)​

![复制代码](0.17117843196533045-20220831215811-id4epez.png)[&quot;复制代码&quot;]("复制代码")

```
    <bean id="rose" class="spring02.Student" scope="prototype">
        <constructor-arg index="0" value="张柏芝"></constructor-arg>
        <constructor-arg index="1" value="196"></constructor-arg>
        <constructor-arg index="2" ref="zhuhai"></constructor-arg>
    </bean>
```

![复制代码](0.6506096614019259-20220831215811-cqfgtys.png)[&quot;复制代码&quot;]("复制代码")

测试代码：

![复制代码](0.23872318978693152-20220831215811-ds90ciq.png)[&quot;复制代码&quot;]("复制代码")

```
        //从容器中获取对象
        Person tom=ctx.getBean("tom",Person.class);
        Person roseA=ctx.getBean("rose",Person.class);
        Person roseB=ctx.getBean("rose",Person.class);
        //Address zhuhai=ctx.getBean("zhuhai",Address.class);
        System.out.println(tom);
        System.out.println(roseA==roseB);
```

![复制代码](0.7388281718715404-20220831215811-rn59i4f.png)[&quot;复制代码&quot;]("复制代码")

 运行结果：

![](0.757851233895493-20220831215811-qoxm4sp.png)​

## 3.6、延迟初始化bean

`ApplicationContext`实现的默认行为就是再启动时将所有 [singleton](http://spring.cndocs.ml/beans.html#beans-factory-scopes-singleton "5.5.1 单例作用域") bean提前进行实例化。 通常这样的提前实例化方式是好事，因为配置中或者运行环境的错误就会被立刻发现，否则可能要花几个小时甚至几天。如果你不想 这样，你可以将单例bean定义为延迟加载防止它提前实例化。延迟初始化bean会告诉Ioc容器在第一次需要的时候才实例化而不是在容器启动时就实例化。

在XML配置文件中，延迟初始化通过`<bean/>`元素的`lazy-init`属性进行控制，比如：

```
<bean id="lazy" class="com.foo.ExpensiveToCreateBean" lazy-init="true"/>
<bean name="not.lazy" class="com.foo.AnotherBean"/>
```

XML：

```
    <bean id="mark" class="entities.Person" lazy-init="true" scope="prototype">
        <property name="name" value="mark"></property>
        <property name="height" value="185"></property>
    </bean>
```

用例：

![复制代码](0.12036931360511716-20220831215811-17ie1of.png)[&quot;复制代码&quot;]("复制代码")

```
    @Test
    public void testMethod3() throws Exception {
        //通过spring配置文件初始化一个容器
        ApplicationContext ctx=new ClassPathXmlApplicationContext("springCfg.xml");
        Thread.sleep(2000);
        Person mark1=ctx.getBean("mark",Person.class);
        Thread.sleep(2000);
        Person mark2=ctx.getBean("mark",Person.class);
        System.out.println(mark1==mark2);
    }
```

![复制代码](0.1720891703342915-20220831215811-ip1rou0.png)[&quot;复制代码&quot;]("复制代码")

结果：

![](0.056617168902823645-20220831215811-oo3o9bz.png)​

## **3.7、回调方法**

### **3.7.1、初始化回调函数**

配置文件：

![复制代码](0.5334036818116743-20220831215811-pek4j7i.png)[&quot;复制代码&quot;]("复制代码")

```
    <bean id="tom" class="spring02.Student" init-method="init" destroy-method="over">
        <constructor-arg name="name" value="张柏川"></constructor-arg>
        <constructor-arg name="height" value="195"></constructor-arg>
        <constructor-arg name="address" ref="zhuhai"></constructor-arg>
    </bean>
```

![复制代码](0.1707480468065965-20220831215811-1amrf9s.png)[&quot;复制代码&quot;]("复制代码")

Student类：

![复制代码](0.029836598114651114-20220831215811-djzvfh6.png)[&quot;复制代码&quot;]("复制代码")

```
package spring02;

import java.io.File;

/**学生*/
public class Student extends Person {
    /**身高*/
    public int height;
    /**有参构造方法*/
    public Student(String name,int height){
        this.name=name;
        this.height=height;
    }
    /**有参构造方法*/
    public Student(String name,int height,Address address){
        this.name=name;
        this.height=height;
        this.address=address;
    }
    @Override
    public String toString() {
        return "Student{" + "height=" + height+",name="+name +'}'+address;
    }

    public void init(){
        System.out.println("对象被创建");
    }
    public void over(){
        System.out.println("对象被回收");
    }

}
```

![复制代码](0.5524530783476009-20220831215811-rpprvph.png)[&quot;复制代码&quot;]("复制代码")

运行结果：

![](0.032070493529084754-20220831215811-966fzj6.png)​

### 3.7.2、析构回调函数

实现 org.springframework.beans.factory.DisposableBean 接口，允许一个bean当容器需要其销毁时获得一次回调。 DisposableBean 接口也只规定了一个方法：

```
void destroy() throws Exception;
```

建议不使用 DisposableBean 回调接口，因为会与Spring耦合。使用@PreDestroy 注解或者指定一个普通的方法，但能由bean定义支持。基于XML配置的元数据，使用 <bean/> 的 destroy-method 属性。例如，下面的定义：

```
<bean id="exampleInitBean" class="examples.ExampleBean" destroy-method="cleanup"/>
```

示例

![复制代码](0.2908835554615816-20220831215811-c919ru1.png)[&quot;复制代码&quot;]("复制代码")

```
public class ExampleBean {

public void cleanup() {
   // do some destruction work (like releasing pooled connections)
  }
}
```

![复制代码](0.0879471853700986-20220831215811-5d7mecg.png)[&quot;复制代码&quot;]("复制代码")

与下面效果相同：

```
<bean id="exampleInitBean" class="examples.AnotherExampleBean"/>
```

示例：

![复制代码](0.6723458625796581-20220831215811-x5j2g78.png)[&quot;复制代码&quot;]("复制代码")

```
public class AnotherExampleBean implements DisposableBean {

  public void destroy() {
  // do some destruction work (like releasing pooled connections)
  }

}
```

![复制代码](0.1770927946423495-20220831215811-bh1hbuj.png)[&quot;复制代码&quot;]("复制代码")

但是不与Spring耦合。

# **四、使用Spring注解配置IOC**

 上一个示例是使用传统的xml配置完成IOC的，如果内容比较多则配置需花费很多时间，通过注解可以减轻工作量，但注解后修改要麻烦一些，偶合度会增加，应该根据需要选择合适的方法。

## 4.1、修改BookDAO

![复制代码](0.42767057358208027-20220831215811-paafet1.png)[&quot;复制代码&quot;]("复制代码")

```
package com.zhangguo.Spring051.ioc02;

import org.springframework.stereotype.Component;
import org.springframework.stereotype.Repository;

/**
 * 图书数据访问实现类
 */
@Component("bookdaoObj")
public class BookDAO implements IBookDAO {

    public String addBook(String bookname) {
        return "添加图书"+bookname+"成功！";
    }
}
```

![复制代码](0.19728245757197294-20220831215811-h5sikmy.png)[&quot;复制代码&quot;]("复制代码")

在类上增加了一个注解Component，在类的开头使用了@Component注解，它可以被Spring容器识别，启动Spring后，会自动把它转成容器管理的Bean。

除了@Component外，Spring提供了3个功能基本和@Component等效的注解，分别对应于用于对DAO，Service，和Controller进行注解。  
1：@Repository 用于对DAO实现类进行注解。  
2：@Service 用于对业务层注解，但是目前该功能与 @Component 相同。  
3：@Constroller用于对控制层注解，但是目前该功能与 @Component 相同。

## 4.2、修改BookService

![复制代码](0.604340650457406-20220831215811-dfajlgi.png)[&quot;复制代码&quot;]("复制代码")

```
package com.zhangguo.Spring051.ioc02;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import org.springframework.stereotype.Component;
import org.springframework.stereotype.Service;

/**
 * 图书业务类
 */
@Component
public class BookService {
    IBookDAO bookDAO;
  
    public void storeBook(String bookname){
        //容器
        ApplicationContext ctx=new ClassPathXmlApplicationContext("IOCBeans02.xml");
        //从容器中获得id为bookdao的bean
        bookDAO=(IBookDAO)ctx.getBean("bookdaoObj");
        System.out.println("图书上货");
        String result=bookDAO.addBook(bookname);
        System.out.println(result);
    }
}
```

![复制代码](0.011987895523421388-20220831215811-sqyfhp7.png)[&quot;复制代码&quot;]("复制代码")

将构造方法中的代码直接写在了storeBook方法中，避免循环加载的问题。

## 4.3、修改IOC配置文件IOCBeans02.xml

![复制代码](0.561165286118211-20220831215811-7ngspk7.png)[&quot;复制代码&quot;]("复制代码")

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
    xmlns:p="http://www.springframework.org/schema/p"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context-4.3.xsd">
        <context:component-scan base-package="com.zhangguo.Spring051.ioc02"></context:component-scan>
</beans>
```

![复制代码](0.2766054149872501-20220831215811-dcg3uoh.png)[&quot;复制代码&quot;]("复制代码")

粗体字是新增的xml命名空间与模式约束文件位置。增加了注解扫描的范围，指定了一个包，可以通过属性设置更加精确的范围如：

<context>标记常用属性配置:  
resource-pattern：对指定的基包下面的子包进行选取  
<context>子标记：  
include-filter：指定需要包含的包  
exclude-filter：指定需要排除的包

<!-- 自动扫描com.zhangguo.anno.bo中的类进行扫描 -->

<context:component-scan base-package="com.zhangguo.anno" resource-pattern="bo/*.class" />

<context:component-scan base-package="com.zhangguo.anno" >

  <context:include-filter type="aspectj“ expression="com.zhangguo.anno.dao.*.*"/>  
  <context:exclude-filter type=“aspectj” expression=“com.zhangguo.anno.entity.*.*”/>

[/context:component-scan](/context:component-scan)

include-filter表示需要包含的目标类型，exclude-filter表示需要排除的目标类型，type表示采的过滤类型，共有如下5种类型：

|**Filter Type**|**Examples Expression**|**Description**|
| ------------| ----------------------------| ----------------------------------------------------------------|
|annotation|org.example.SomeAnnotation|注解了SomeAnnotation的类|
|assignable|org.example.SomeClass|所有扩展或者实现SomeClass的类|
|aspectj|org.example..*Service+|AspectJ语法表示org.example包下所有包含Service的类及其子类|
|regex|org.example.Default.*|Regelar Expression，正则表达式|
|custom|org.example.MyTypeFilter|通过代码过滤，实现org.springframework.core.type.TypeFilter接口|

expression表示过滤的表达式。

```
    <!-- 1、如果仅希望扫描特定的类而非基包下的所有类，可使用resource-pattern属性过滤特定的类 -->
    <context:component-scan base-package="com.zhangguo.Spring051"
        resource-pattern="ioc04/A*.class">
    </context:component-scan>
```

只扫描com.zhangguo.Spring051.ioc04下所有名称以A开始的类。

![复制代码](0.8568170810242111-20220831215811-sb0zj1o.png)[&quot;复制代码&quot;]("复制代码")

```
    <!--2、扫描注解了org.springframework.stereotype.Repository的类
     exclude-filter表示排除，include-filter表示包含，可以有多个-->
    <context:component-scan base-package="com.zhangguo.Spring051.ioc04"> 
        <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Repository" />
        <context:include-filter type="annotation" expression="org.springframework.stereotype.Service"/>
    </context:component-scan>
```

![复制代码](0.8572272940846883-20220831215811-hzdek6i.png)[&quot;复制代码&quot;]("复制代码")

![复制代码](0.13474616013243623-20220831215811-5m9kn8o.png)[&quot;复制代码&quot;]("复制代码")

```
   <!--3、aspectj类型，扫描dao下所有的类，排除entity下所有的类-->
  <context:component-scan base-package="com.zhangguo.anno" >
  <context:include-filter type="aspectj" expression="com.zhangguo.anno.dao.*.*"/>
  <context:exclude-filter type="aspectj" expression="com.zhangguo.anno.entity.*.*"/>
</context:component-scan>
```

![复制代码](0.04237751760932884-20220831215811-6ewk9za.png)[&quot;复制代码&quot;]("复制代码")

测试类

![复制代码](0.1213941815576911-20220831215811-2qpgtpn.png)[&quot;复制代码&quot;]("复制代码")

```
package com.zhangguo.Spring051.ioc02;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class Test {
    @org.junit.Test
    public void testStoreBook()
    {
        //容器
        ApplicationContext ctx=new ClassPathXmlApplicationContext("IOCBeans02.xml");
        BookService bookservice=ctx.getBean(BookService.class);
        bookservice.storeBook("《Spring MVC权威指南 第二版》");
    }
}
```

![复制代码](0.459152119848943-20220831215811-lr8sas8.png)[&quot;复制代码&quot;]("复制代码")

运行结果：

![](0.19017554150203297-20220831215811-8ztbip7.png)​

## 4.4、简单示例

IBookDao接口

![复制代码](0.06417486656693794-20220831215811-bhtfl65.png)[&quot;复制代码&quot;]("复制代码")

```
package spring11;

public interface IBookDao {
    void add();
}
```

![复制代码](0.4388989809828243-20220831215811-jz13p71.png)[&quot;复制代码&quot;]("复制代码")

BookDao实现类

![复制代码](0.4325716451874393-20220831215811-yga8wok.png)[&quot;复制代码&quot;]("复制代码")

```
package spring11;

import org.springframework.stereotype.Component;

@Component("bookdao")
public class BookDao implements IBookDao {
    public void add() {
        System.out.println("新增图书成功！");
    }
}
```

![复制代码](0.4434164625545056-20220831215811-de1aw5c.png)[&quot;复制代码&quot;]("复制代码")

MSBookDao实现类

![复制代码](0.10236358380002519-20220831215811-ny1htss.png)[&quot;复制代码&quot;]("复制代码")

```
package spring11;

import org.springframework.stereotype.Component;
@Component("bookdao")
public class MSBookDao implements IBookDao {
    public void add() {
        System.out.println("新增图书到SQLServer成功！");
    }
}
```

![复制代码](0.5097331262044869-20220831215811-fq0rrk0.png)[&quot;复制代码&quot;]("复制代码")

BookService测试类：

![复制代码](0.37479157416620934-20220831215811-vqwek5b.png)[&quot;复制代码&quot;]("复制代码")

```
package spring11;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class BookService {
    public static void main(String[] args) {
        //容器
        ApplicationContext ctx=
                new ClassPathXmlApplicationContext(new String[]{"bookbean11.xml"});
        //从容器中获得对象
        IBookDao dao=ctx.getBean("bookdao",IBookDao.class);

        dao.add();

    }
}
```

![复制代码](0.23535710929506015-20220831215811-arf5ytr.png)[&quot;复制代码&quot;]("复制代码")

XML配置：

![复制代码](0.8310032407420478-20220831215811-cg65cyv.png)[&quot;复制代码&quot;]("复制代码")

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context-4.3.xsd">
    <bean name="tom" class="spring02.Student" p:name="tom" p:height="224"/>

    <!--指定要扫描的包，如果有多个可以用逗号隔开-->
    <context:component-scan base-package="spring11">
        <!--使用正则排除以B开头的类-->
        <context:exclude-filter type="regex" expression="spring11\.B.*"></context:exclude-filter>
    </context:component-scan>

</beans>
```

![复制代码](0.08660573460431831-20220831215811-c2jdkrc.png)[&quot;复制代码&quot;]("复制代码")

运行结果：

![](0.9547617230875975-20220831215811-6vmxfpb.png)​

指定两个bookdao是不正确的，重名了，但是因为在组件扫描中我们排除了一个所有也可以正确运行。

## 4.5、作用域用scope来指定

默认容器中的bean是单例的：

```
        //从容器中获得对象
        IBookDao dao1=ctx.getBean("bookdao",IBookDao.class);
        IBookDao dao2=ctx.getBean("bookdao",IBookDao.class);
        System.out.println(dao1==dao2);
```

结果：

![](0.4527540812273585-20220831215811-2km62k5.png)​

修改成原型

![复制代码](0.8034201032635788-20220831215811-y4f3hkj.png)[&quot;复制代码&quot;]("复制代码")

```
package spring11;

import org.springframework.context.annotation.Scope;
import org.springframework.stereotype.Component;

@Component("bookdao")
@Scope("prototype")
public class MSBookDao implements IBookDao {
    public void add() {
        System.out.println("新增图书到SQLServer成功！");
    }
}
```

![复制代码](0.8938078963604081-20220831215811-npysfmt.png)[&quot;复制代码&quot;]("复制代码")

结果：

![](0.5259448781427036-20220831215811-l1krelx.png)​

用来指定bean的作用域

singleton---单例  只创建一个对象。

prototype---原型  想创建多少个就创建多少了。

request---针对Web项目，不同的请求创建单独的Bean对象，同一个请求共享一个Bean。

session---针对Web项目，不同的会话创建单独的Bean对象，同一个会话共享一个Bean。

## 4.5 、Lazy延迟初始化Bean

默认情况下Spring IoC容器在初始化时将Bean创建好存放到容器中：

测试：

![复制代码](0.5926871639441005-20220831215811-mbgf24o.png)[&quot;复制代码&quot;]("复制代码")

```
    @Test
    public void testMethod6() throws Exception {
        //通过spring配置文件初始化一个容器
        ApplicationContext ctx=new ClassPathXmlApplicationContext("springCfg2.xml");
        Thread.sleep(6000);
        IBookDao dao=ctx.getBean("mssql",IBookDao.class);
        dao.add("<<Java EE>>");
    }
```

![复制代码](0.7386014476501399-20220831215811-2x5gut1.png)[&quot;复制代码&quot;]("复制代码")

结果：

![](0.9032229285624533-20220831215811-8byu86g.png)​

此处等待6秒...

![](0.19087717809111626-20220831215811-1y0462t.png)​

增加注解修改成延迟初始化bean

![复制代码](0.37955524731914525-20220831215811-xbrczmq.png)[&quot;复制代码&quot;]("复制代码")

```
package dao;

import org.springframework.context.annotation.Lazy;
import org.springframework.stereotype.Repository;

@Repository("mssql")
@Lazy
public class SQLServerBookDao implements IBookDao {

    public SQLServerBookDao() {
        System.out.println("SQLServerBookDao构造方法被调用");
    }

    /**
     * 添加图书
     *
     * @param name
     */
    public void add(String name) {
        System.out.println("添加图书到SQLServer数据库成功："+name);
    }
}
```

![复制代码](0.3528088025377851-20220831215811-iq5vpbp.png)[&quot;复制代码&quot;]("复制代码")

再次测试结果：

![](0.5190910045656196-20220831215811-ob48qef.png)​

然后

![](0.899128232759868-20220831215811-tyhz0td.png)​

## 4.6、初始化回调注解与销毁回调注解

### 4.6.1、@PostConstruct

@PostConstruct  初始化方法的注解方式 等同与在XML中声明init-method=init

![复制代码](0.3192496849227122-20220831215811-fhofdfr.png)[&quot;复制代码&quot;]("复制代码")

```
package dao;

import org.springframework.context.annotation.Lazy;
import org.springframework.stereotype.Repository;

import javax.annotation.PostConstruct;

@Repository("mssql")
@Lazy
public class SQLServerBookDao implements IBookDao {

    public SQLServerBookDao() {
        System.out.println("SQLServerBookDao构造方法被调用");
    }

    /**
     * 添加图书
     *
     * @param name
     */
    public void add(String name) {
        System.out.println("添加图书到SQLServer数据库成功："+name);
    }

    //init-method callback
    @PostConstruct
    public void init(){
        System.out.println("SQLServerBookDao初创建完成了");
    }
}
```

![复制代码](0.6350809427804036-20220831215811-3gf3f4f.png)[&quot;复制代码&quot;]("复制代码")

结果

![](0.08363889481299713-20220831215811-tv9uqj1.png)​

### 4.6.2、@PreDestroy

@PreDestroy 销毁方法的注解方式 等同于在XML中声明destory-method=destory

![复制代码](0.3942884702181073-20220831215811-hdvlozb.png)[&quot;复制代码&quot;]("复制代码")

```
package dao;

import org.springframework.context.annotation.Lazy;
import org.springframework.stereotype.Repository;

import javax.annotation.PostConstruct;
import javax.annotation.PreDestroy;

@Repository("mssql")
@Lazy
public class SQLServerBookDao implements IBookDao {

    public SQLServerBookDao() {
        System.out.println("SQLServerBookDao构造方法被调用");
    }

    /**
     * 添加图书
     *
     * @param name
     */
    public void add(String name) {
        System.out.println("添加图书到SQLServer数据库成功："+name);
    }

    //init-method callback
    @PostConstruct
    public void init(){
        System.out.println("SQLServerBookDao初创建完成了");
    }

    //destory-method callback
    @PreDestroy
    public void destory(){
        System.out.println("SQLServerBookDao准备销毁了");
    }
}
```

![复制代码](0.4436995985287042-20220831215811-5ajeh29.png)[&quot;复制代码&quot;]("复制代码")

## 4.7、默认名称

如果使用Compont注解时不指定名称，基于@Componet及其扩展（如@Servic和自定义等）标注和classpath-scan定义的Bean，注解有一个value属性，如果提供了，那么就此Bean的名字。如果不提供。就会使用Spring默认的命名机制，即简单类名且第一个字母小写

![复制代码](0.6237818899477223-20220831215811-036iufb.png)[&quot;复制代码&quot;]("复制代码")

```
package spring11;

import org.springframework.context.annotation.Scope;
import org.springframework.stereotype.Component;

@Component()  //未指定名称
@Scope("prototype")
public class MSBookDao implements IBookDao {
    public void add() {
        System.out.println("新增图书到SQLServer成功！");
    }
}
```

![复制代码](0.17536162588032744-20220831215811-1r1488x.png)[&quot;复制代码&quot;]("复制代码")

测试：

![复制代码](0.8118384932019627-20220831215811-4rf1he6.png)[&quot;复制代码&quot;]("复制代码")

```
package spring11;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class BookService {
    public static void main(String[] args) {
        //容器
        ApplicationContext ctx=
                new ClassPathXmlApplicationContext(new String[]{"bookbean11.xml"});
        //从容器中获得对象
        IBookDao dao1=ctx.getBean("MSBookDao",IBookDao.class);
        IBookDao dao2=ctx.getBean("MSBookDao",IBookDao.class);
        System.out.println(dao1==dao2);
        dao1.add();

    }
}
```

![复制代码](0.8074474490904116-20220831215811-2fjry2w.png)[&quot;复制代码&quot;]("复制代码")

结果：

![](0.1032106643224473-20220831215811-969w5lz.png)​

在基于XML的配置中bean标签还有很多属性，如scope、Lazy、init-method、depends-on、Qualifier等。

从容器中获取实例时也可以直接根据类型获取

![复制代码](0.9190422705599597-20220831215811-hrttzc9.png)[&quot;复制代码&quot;]("复制代码")

```
package spring12;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import org.springframework.stereotype.Component;

@Component
public class BookStore {
    @Autowired
    BookService service;

    public static void main(String[] args) {
        ApplicationContext ctx = new ClassPathXmlApplicationContext("bookbean12.xml");
        BookStore store =ctx.getBean(BookStore.class);
        store.service.addNewBook();
        A a=ctx.getBean("a",A.class);
        System.out.println(a);
    }
}

@Component
class A{

}
@Component
class B extends A{

}
```

![复制代码](0.10834424804270282-20220831215811-cpkpmgy.png)[&quot;复制代码&quot;]("复制代码")

结果：

![](0.32817918252220757-20220831215811-0chsxfh.png)​

默认名称时需要将首字母小写，Camel命名规范：

![复制代码](0.6252239169618015-20220831215811-v43s95z.png)[&quot;复制代码&quot;]("复制代码")

```
package dao;

import org.springframework.context.annotation.Scope;
import org.springframework.stereotype.Component;
import org.springframework.stereotype.Controller;
import org.springframework.stereotype.Repository;
import org.springframework.stereotype.Service;

@Component
public class MySqlBookDao implements IBookDao {
    /**
     * 添加图书
     *
     * @param name
     */
    public void add(String name) {
        System.out.println("添加图书到MySQL数据库成功："+name);
    }
}
```

![复制代码](0.768277290172583-20220831215811-f56f7dg.png)[&quot;复制代码&quot;]("复制代码")

测试：

![复制代码](0.249994955009875-20220831215811-saykvfq.png)[&quot;复制代码&quot;]("复制代码")

```
    @Test
    public void testMethod5() throws Exception {
        //通过spring配置文件初始化一个容器
        ApplicationContext ctx=new ClassPathXmlApplicationContext("springCfg2.xml");
        IBookDao bookDao=ctx.getBean("mySqlBookDao",IBookDao.class);
        bookDao.add("《Spring从入门到精通》");
    }
```

![复制代码](0.4679413076319374-20220831215811-bbxl955.png)[&quot;复制代码&quot;]("复制代码")

结果：

![](0.4009421879215731-20220831215811-tuig2ms.png)​

## 4.6、小结

从配置文件中我们可以看出我们并没有声明bookdaoObj与BookService类型的对象，但还是从容器中获得了实例并成功运行了，原因是：在类的开头使用了@Component注解，它可以被Spring容器识别，启动Spring后，会自动把它转成容器管理的Bean。

# **五、自动装配**

 从上一个示例中可以看出有两个位置都使用了ApplicationContext初始化容器后获得需要的Bean，可以通过自动装配简化。

## 5.1、修改BookDAO

![复制代码](0.7476764136403962-20220831215811-fx7doa0.png)[&quot;复制代码&quot;]("复制代码")

```
package com.zhangguo.Spring051.ioc03;

import org.springframework.stereotype.Component;
import org.springframework.stereotype.Repository;

/**
 * 图书数据访问实现类
 */
@Repository
public class BookDAO implements IBookDAO {

    public String addBook(String bookname) {
        return "添加图书"+bookname+"成功！";
    }
}
```

![复制代码](0.015440445476913478-20220831215811-mols6jt.png)[&quot;复制代码&quot;]("复制代码")

把注解修改成了Repository，比Component更贴切一些，非必要。

## 5.2、修改BookService

![复制代码](0.857038501942516-20220831215811-48s9fgl.png)[&quot;复制代码&quot;]("复制代码")

```
package com.zhangguo.Spring051.ioc03;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import org.springframework.stereotype.Service;

/**
 * 图书业务类
 */
@Service
public class BookService {
    @Autowired
    IBookDAO bookDAO;
  
    public void storeBook(String bookname){
        System.out.println("图书上货");
        String result=bookDAO.addBook(bookname);
        System.out.println(result);
    }
}
```

![复制代码](0.22649470507734382-20220831215811-2xb0kzh.png)[&quot;复制代码&quot;]("复制代码")

将类BookService上的注解替换成了Service；在bookDao成员变量上增加了一个注解@Autowired，该注解的作用是：可以对成员变量、方法和构造函数进行注解，来完成自动装配的工作，通俗来说就是会根据类型从容器中自动查到到一个Bean给bookDAO字段。@Autowired是根据类型进行自动装配的，如果需要按名称进行装配，则需要配合@Qualifier。另外可以使用其它注解，@ Resource ：等同于@Qualifier，@Inject：等同于@ Autowired。

@Service用于注解业务层组件（我们通常定义的service层就用这个）

@Controller用于注解控制层组件（如struts中的action）

@Repository用于注解数据访问组件，即DAO组件

@Component泛指组件，当组件不好归类的时候，我们可以使用这个注解进行注解。

装配注解主要有：@Autowired、@Qualifier、@Resource，它们的特点是：

1、@Resource默认是按照名称来装配注入的，只有当找不到与名称匹配的bean才会按照类型来装配注入；

2、@Autowired默认是按照类型装配注入的，如果想按照名称来转配注入，则需要结合@Qualifier一起使用；

3、@Resource注解是又J2EE提供，而@Autowired是由spring提供，故减少系统对spring的依赖建议使用@Resource的方式；如果Maven项目是1.5的JRE则需换成更高版本的。

4、@Resource和@Autowired都可以书写注解在字段或者该字段的setter方法之上

5、@Autowired 可以对成员变量、方法以及构造函数进行注释，而 @Qualifier 的注解对象是成员变量、方法入参、构造函数入参。

6、@Qualifier("XXX") 中的 XX是 Bean 的名称，所以 @Autowired 和 @Qualifier 结合使用时，自动注入的策略就从 byType 转变成 byName 了。

7、@Autowired 注释进行自动注入时，Spring 容器中匹配的候选 Bean 数目必须有且仅有一个，通过属性required可以设置非必要。

8、@Resource装配顺序  
　　8.1. 如果同时指定了name和type，则从Spring上下文中找到唯一匹配的bean进行装配，找不到则抛出异常  
　　8.2. 如果指定了name，则从上下文中查找名称（id）匹配的bean进行装配，找不到则抛出异常  
　　8.3. 如果指定了type，则从上下文中找到类型匹配的唯一bean进行装配，找不到或者找到多个，都会抛出异常  
　　8.4. 如果既没有指定name，又没有指定type，则自动按照byName方式进行装配；如果没有匹配，则回退为一个原始类型进行匹配，如果匹配则自动装配；

![复制代码](0.3425873211411712-20220831215811-pi81swk.png)[&quot;复制代码&quot;]("复制代码")

```
package com.zhangguo.Spring051.ioc05;

import javax.annotation.Resource;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import org.springframework.stereotype.Service;

/**
 * 图书业务类
 */
@Service
public class BookService {
  
    public IBookDAO getDaoofbook() {
        return daoofbook;
    }

    /*
    @Autowired
    @Qualifier("bookdao02")
    public void setDaoofbook(IBookDAO daoofbook) {
        this.daoofbook = daoofbook;
    }*/
  
    @Resource(name="bookdao02")
    public void setDaoofbook(IBookDAO daoofbook) {
        this.daoofbook = daoofbook;
    }

    /*
    @Autowired
    @Qualifier("bookdao02")
    */
    IBookDAO daoofbook;
  
    /*
    public BookService(@Qualifier("bookdao02") IBookDAO daoofbook) {
        this.daoofbook=daoofbook;
    }*/
  
    public void storeBook(String bookname){
        System.out.println("图书上货");
        String result=daoofbook.addBook(bookname);
        System.out.println(result);
    }
}
```

![复制代码](0.39023557375964146-20220831215811-2u1e4q2.png)[&quot;复制代码&quot;]("复制代码")

测试运行

![复制代码](0.7886521342540305-20220831215811-iagj1la.png)[&quot;复制代码&quot;]("复制代码")

```
package com.zhangguo.Spring051.ioc03;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class Test {
    @org.junit.Test
    public void testStoreBook()
    {
        //容器
        ApplicationContext ctx=new ClassPathXmlApplicationContext("IOCBeans03.xml");
        BookService bookservice=ctx.getBean(BookService.class);
        bookservice.storeBook("《Spring MVC权威指南 第三版》");
    }
}
```

![复制代码](0.6628689134625785-20220831215811-18d1hfx.png)[&quot;复制代码&quot;]("复制代码")

运行结果：

![](0.469678130517857-20220831215811-cfq1vx3.png)​

## 5.3、简单示例

IBookDao

![复制代码](0.2882735830260217-20220831215811-a33bmzw.png)[&quot;复制代码&quot;]("复制代码")

```
package spring12;

/**图书数据访问接口*/
public interface IBookDao {
    /**添加新书*/
    void save(String name);
}
```

![复制代码](0.5392479219619268-20220831215811-dpszn88.png)[&quot;复制代码&quot;]("复制代码")

BookDao

![复制代码](0.3326553775930543-20220831215811-57wwjc8.png)[&quot;复制代码&quot;]("复制代码")

```
package spring12;

import org.springframework.stereotype.Repository;

/**
 * 完成图书数据访问
 */
@Repository
public class BookDao implements IBookDao {
    public void save(String name) {
        System.out.println("添加图书" + name + "到数据库成功！");
    }
}
```

![复制代码](0.10951891738876962-20220831215811-17f8oar.png)[&quot;复制代码&quot;]("复制代码")

BookService

![复制代码](0.21074438033385445-20220831215811-yljx6cu.png)[&quot;复制代码&quot;]("复制代码")

```
package spring12;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class BookService {

    @Autowired
    IBookDao bookDao;

    /**新增一本书*/
    public void addNewBook(){
        String bookname="《Spring MVC学习指南》";
        bookDao.save(bookname);
    }
}
```

![复制代码](0.36425288382056165-20220831215811-grluy08.png)[&quot;复制代码&quot;]("复制代码")

配置bookbean12.xml

![复制代码](0.13938191957972168-20220831215811-p9w9wek.png)[&quot;复制代码&quot;]("复制代码")

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context-4.3.xsd">
    <!--指定要扫描的包，如果有多个可以用逗号隔开-->
    <context:component-scan base-package="spring12">
    </context:component-scan>
</beans>
```

![复制代码](0.4291090980909782-20220831215811-v3fow59.png)[&quot;复制代码&quot;]("复制代码")

BookStore

![复制代码](0.007212655700699511-20220831215811-2yr0xqo.png)[&quot;复制代码&quot;]("复制代码")

```
package spring12;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import org.springframework.stereotype.Component;

@Component
public class BookStore {

    @Autowired
    BookService service;

    public static void main(String[] args) {
        ApplicationContext ctx = new ClassPathXmlApplicationContext("bookbean12.xml");
        BookStore store =ctx.getBean(BookStore.class);
        store.service.addNewBook();
    }
}
```

![复制代码](0.5770396275625584-20220831215811-xwoj5tj.png)[&quot;复制代码&quot;]("复制代码")

测试结果：

![](0.2795350500382947-20220831215811-jqtibat.png)​

## 5.4、@Qualifier指定名称

@Qualifier("XXX") 中的 XX是 Bean 的名称，所以 @Autowired 和 @Qualifier 结合使用时，自动注入的策略就从 byType 转变成 byName 了。qualifier的意思是合格者，通过这个标示，表明了哪个实现类才是我们所需要的  
先看下面这个示例：

![复制代码](0.9637698351439181-20220831215811-hacif0r.png)[&quot;复制代码&quot;]("复制代码")

```
package spring13;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import org.springframework.stereotype.Component;
import org.springframework.stereotype.Repository;
import org.springframework.stereotype.Service;

@Service
public class QualifierTest {
    @Autowired
    IBookDao dao;

    public static void main(String[] args) {
        ApplicationContext ctx =
                new ClassPathXmlApplicationContext("bookbean13.xml");
        QualifierTest obj = ctx.getBean(QualifierTest.class);
        System.out.println(obj.dao);
    }
}

interface IBookDao {
}

@Repository
class BookDaoA implements IBookDao {
}

@Repository
class BookDaoB implements IBookDao {
}
```

![复制代码](0.6538585224986719-20220831215811-o1f9z4n.png)[&quot;复制代码&quot;]("复制代码")

运行结果：

 ![](0.05232079258668776-20220831215811-vxu3lj9.png)​

这样报错的原因是找到了多个Bean，Spring不知道选择那一个。使用Qualifier指定名称

![复制代码](0.3024591280882447-20220831215811-nppbvfl.png)[&quot;复制代码&quot;]("复制代码")

```
package spring13;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import org.springframework.stereotype.Component;
import org.springframework.stereotype.Repository;
import org.springframework.stereotype.Service;

@Service
public class QualifierTest {
    @Autowired
    @Qualifier("daoA")
    IBookDao dao;

    public static void main(String[] args) {
        ApplicationContext ctx =
                new ClassPathXmlApplicationContext("bookbean13.xml");
        QualifierTest obj = ctx.getBean(QualifierTest.class);
        System.out.println(obj.dao);
    }
}

interface IBookDao {
}

@Repository("daoA")
class BookDaoA implements IBookDao {
}

@Repository("daoB")
class BookDaoB implements IBookDao {
}
```

![复制代码](0.3810254518478753-20220831215811-885yx6s.png)[&quot;复制代码&quot;]("复制代码")

结果：

![](0.9952442116787761-20220831215811-x8ptz5z.png)​

## 5.5、@Resource

@Resource装配顺序  
如果同时指定了name和type，则从Spring上下文中找到唯一匹配的bean进行装配，找不到则抛出异常  
如果指定了name，则从上下文中查找名称（id）匹配的bean进行装配，找不到则抛出异常  
如果指定了type，则从上下文中找到类型匹配的唯一bean进行装配，找不到或者找到多个，都会抛出异常  
如果既没有指定name，又没有指定type，则自动按照byName方式进行装配；如果没有匹配，则回退为一个原始类型进行匹配，如果匹配则自动装配；

![复制代码](0.4640054223301129-20220831215811-qpsp7dr.png)[&quot;复制代码&quot;]("复制代码")

```
package spring13;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import org.springframework.stereotype.Repository;
import org.springframework.stereotype.Service;

import javax.annotation.Resource;

@Service
public class ResourceTest {
    @Resource(name = "carB")
    ICarDao dao;

    public static void main(String[] args) {
        ApplicationContext ctx =
                new ClassPathXmlApplicationContext("bookbean13.xml");
        ResourceTest obj = ctx.getBean(ResourceTest.class);
        System.out.println(obj.dao);
    }
}

interface ICarDao {
}

@Repository("carA")
class CarDaoA implements ICarDao {
}

@Repository("carB")
class CarDaoB implements ICarDao {
}

/*
@Resource装配顺序
如果同时指定了name和type，则从Spring上下文中找到唯一匹配的bean进行装配，找不到则抛出异常
如果指定了name，则从上下文中查找名称（id）匹配的bean进行装配，找不到则抛出异常
如果指定了type，则从上下文中找到类型匹配的唯一bean进行装配，找不到或者找到多个，都会抛出异常
如果既没有指定name，又没有指定type，则自动按照byName方式进行装配；如果没有匹配，则回退为一个原始类型进行匹配，如果匹配则自动装配；
*/
```

![复制代码](0.049592443286100796-20220831215811-gpnzgoa.png)[&quot;复制代码&quot;]("复制代码")

```
运行结果：
```

![](0.04274114254617656-20220831215811-0wkqtx5.png)​

## 5.6、多种注入方式

示例：

![复制代码](0.42731348808065417-20220831215811-tehrpyf.png)[&quot;复制代码&quot;]("复制代码")

```
package spring13;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.Scope;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import org.springframework.stereotype.Repository;
import org.springframework.stereotype.Service;

import javax.annotation.Resource;

@Service
public class InjectTest {
    //注入给构造方法
    @Autowired
    public InjectTest(IUserDao dao2) {
        this.dao2=dao2;
    }

    //注入给成员变量，字段
    @Resource
    IUserDao dao1;
    IUserDao dao2;
    IUserDao dao3;
    IUserDao dao4;

    //注入给属性
    @Autowired
    public void setDao3(IUserDao dao3) {
        this.dao3 = dao3;
    }

    //注入给方法参数
    @Autowired
    public void injectDao4(IUserDao dao4, IUserDao dao5) {
        this.dao4 = dao4;
        System.out.println(dao5);
    }

    public static void main(String[] args) {
        ApplicationContext ctx =
                new ClassPathXmlApplicationContext("bookbean13.xml");
        InjectTest obj = ctx.getBean(InjectTest.class);
        System.out.println(obj.dao1);
        System.out.println(obj.dao2);
        System.out.println(obj.dao3);
        System.out.println(obj.dao4);
    }
}

interface IUserDao {
}

@Scope("prototype")
@Repository
class UserDao implements IUserDao {
}
```

![复制代码](0.8282144364668991-20220831215811-mytzsov.png)[&quot;复制代码&quot;]("复制代码")

结果：

![](0.6150017591313208-20220831215811-q0f7dtx.png)​

# **六、零配置实现IOC**

**6.1、综合示例**

所谓的零配置就是不再使用xml文件来初始化容器，使用一个类型来替代，

 IBookDAO代码如下：

![复制代码](0.5254099230859237-20220831215811-3d1avve.png)[&quot;复制代码&quot;]("复制代码")

```
package com.zhangguo.Spring051.ioc06;

/**
 * 图书数据访问接口
 */
public interface IBookDAO {
    /**
     * 添加图书
     */
    public String addBook(String bookname);
}
```

![复制代码](0.2249096555970349-20220831215811-1ilbons.png)[&quot;复制代码&quot;]("复制代码")

IBookDAO的实现类BookDAO代码如下：

![复制代码](0.5855306724933897-20220831215811-uce4y94.png)[&quot;复制代码&quot;]("复制代码")

```
package com.zhangguo.Spring051.ioc06;

import org.springframework.stereotype.Component;
import org.springframework.stereotype.Repository;

/**
 * 图书数据访问实现类
 */
@Repository
public class BookDAO implements IBookDAO {

    public String addBook(String bookname) {
        return "添加图书"+bookname+"成功！";
    }
}
```

![复制代码](0.5342751452038823-20220831215811-wuimgbc.png)[&quot;复制代码&quot;]("复制代码")

在BookDAO类上注解了@Repository当初始化时该类将被容器管理会生成一个Bean，可以通过构造方法测试。

业务层BookService代码如下：

![复制代码](0.7952454651831551-20220831215811-xvm08ck.png)[&quot;复制代码&quot;]("复制代码")

```
package com.zhangguo.Spring051.ioc06;

import javax.annotation.Resource;

import org.springframework.stereotype.Service;

/**
 * 图书业务类
 */
@Service
public class BookService {
    @Resource
    IBookDAO bookDAO;
  
    public void storeBook(String bookname){
        System.out.println("图书上货");
        String result=bookDAO.addBook(bookname);
        System.out.println(result);
    }
}
```

![复制代码](0.08433172658794774-20220831215811-h9sb797.png)[&quot;复制代码&quot;]("复制代码")

类BookService将对容器管理因为注解了@Service，初始化时会生成一个单例的Bean，类型为BookService。在字段bookDAO上注解了@Resource，用于自动装配，Resource默认是按照名称来装配注入的，只有当找不到与名称匹配的bean才会按照类型来装配注入。

新增一个用于替代原xml配置文件的ApplicationCfg类，代码如下：

![复制代码](0.7578989626306225-20220831215811-gniaak9.png)[&quot;复制代码&quot;]("复制代码")

```
package com.zhangguo.Spring051.ioc06;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;

/**
 * 容器的配置类
 */
@Configuration
@ComponentScan(basePackages="com.zhangguo.Spring051.ioc06")
public class ApplicationCfg {
    @Bean
    public User getUser(){
        return new User("成功");
    }
}
```

![复制代码](0.8925925231507019-20220831215811-lnw5jh6.png)[&quot;复制代码&quot;]("复制代码")

@Configuration相当于配置文件中的<beans/>，ComponentScan相当于配置文件中的context:component-scan，属性也一样设置

,@Bean相当于<bean/>，只能注解在方法和注解上，一般在方法上使用，源码中描述：@Target({ElementType.METHOD, ElementType.ANNOTATION_TYPE})，方法名相当于id。中间使用到了User，User类的代码如下：

![复制代码](0.57964868018615-20220831215811-58c5fu6.png)[&quot;复制代码&quot;]("复制代码")

```
package com.zhangguo.Spring051.ioc06;

import org.springframework.stereotype.Component;

@Component("user1")
public class User {
    public User() {
        System.out.println("创建User对象");
    }
    public User(String msg) {
        System.out.println("创建User对象"+msg);
    }
    public void show(){
        System.out.println("一个学生对象！");
    }
}
```

![复制代码](0.9269458546250062-20220831215811-jz689sp.png)[&quot;复制代码&quot;]("复制代码")

初始化容器的代码与以前有一些不一样，具体如下：

![复制代码](0.8702523787079537-20220831215811-pobq0ry.png)[&quot;复制代码&quot;]("复制代码")

```
package com.zhangguo.Spring051.ioc06;

import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class Test {
    @org.junit.Test
    public void testStoreBook()
    {
        //容器，注解配置应用程序容器，Spring通过反射ApplicationCfg.class初始化容器
        ApplicationContext ctx=new AnnotationConfigApplicationContext(ApplicationCfg.class);
        BookService bookservice=ctx.getBean(BookService.class);
        bookservice.storeBook("《Spring MVC权威指南 第四版》");
        User user1=ctx.getBean("user1",User.class);
        user1.show();
        User getUser=ctx.getBean("getUser",User.class);
        getUser.show();
    }
}
```

![复制代码](0.29703048597898896-20220831215811-4prxl7h.png)[&quot;复制代码&quot;]("复制代码")

容器的初始化通过一个类型完成，Spring通过反射ApplicationCfg.class初始化容器，中间user1与getUser是否为相同的Bean呢？

答案是否定的，因为在ApplicationCfg中声明的方法getUser当相于在xml文件中定义了一个<bean id="getUser" class="..."/>，在User类上注解@Component("user1")相当于另一个<bean id="user1" class="..."/>。

运行结果：

![](0.49321624859094526-20220831215811-hd1cjos.png)​

小结：使用零配置和注解虽然方便，不需要编写麻烦的xml文件，但并非为了取代xml，应该根据实例需要选择，或二者结合使用，毕竟使用一个类作为容器的配置信息是硬编码的，不好在发布后修改。

## 6.2、零配置，由注解指定实例

![复制代码](0.8767445410205241-20220831215811-2b5zenh.png)[&quot;复制代码&quot;]("复制代码")

```
package spring14;

import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.stereotype.Repository;

public class NoXMLIoC {
    public static void main(String[] args) {
        //基于类型的配置
        ApplicationContext ctx=new AnnotationConfigApplicationContext(AppCfg.class);
        ICarDao dao1=ctx.getBean(ICarDao.class);
        dao1.add("Spring Pro");
    }
}

interface ICarDao{
    void add(String name);
}
@Repository
class CarDao implements ICarDao{
    public void add(String name) {
        System.out.println("添加"+name+"成功！");
    }
}
@Configuration
@ComponentScan(basePackages = "spring14")
class AppCfg{

}
```

![复制代码](0.11355879651288547-20220831215811-0t5dspn.png)[&quot;复制代码&quot;]("复制代码")

运行结果：

![](0.006144946546594587-20220831215811-k85ynlp.png)​

## 6.3、零配置，由方法指定实例

![复制代码](0.64246750441212-20220831215811-qhp6cih.png)[&quot;复制代码&quot;]("复制代码")

```
package spring14;

import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.stereotype.Repository;

public class NoXMLIoC {
    public static void main(String[] args) {
        //基于类型的配置
        ApplicationContext ctx=new AnnotationConfigApplicationContext(AppCfg.class);
        ICarDao dao2=ctx.getBean("mysqlDao",ICarDao.class);
        dao2.add("Spring Pro");
    }
}

interface ICarDao{
    void add(String name);
}
class CarDao implements ICarDao{
    public void add(String name) {
        System.out.println("添加"+name+"成功！");
    }
}
@Configuration
@ComponentScan(basePackages = "spring14")
class AppCfg{
    @Bean
    ICarDao mysqlDao(){  //方法名就是bean的name
        return new CarDao();
    }
}
```

![复制代码](0.340604368138268-20220831215811-kpfu6lx.png)[&quot;复制代码&quot;]("复制代码")

运行结果：

![](0.511961782187669-20220831215811-xvfeiho.png)​

![复制代码](0.6269043362984141-20220831215811-rpcifwn.png)[&quot;复制代码&quot;]("复制代码")

```
package spring14;

import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.stereotype.Repository;

public class NoXMLIoC {
    public static void main(String[] args) {
        //基于类型的配置
        ApplicationContext ctx=new AnnotationConfigApplicationContext(AppCfg.class);
        ICarDao dao1=ctx.getBean("oracleDao",ICarDao.class);
        dao1.add("Spring Pro Oracle");
        ICarDao dao2=ctx.getBean("mysqlDao",ICarDao.class);
        dao2.add("Spring Pro MySQL");
        System.out.println(dao1==dao2);
    }
}

interface ICarDao{
    void add(String name);
}
@Repository("oracleDao")
class CarDao implements ICarDao{
    public void add(String name) {
        System.out.println("添加"+name+"成功！");
    }
}
@Configuration
@ComponentScan(basePackages = "spring14")
class AppCfg{
    @Bean
    ICarDao mysqlDao(){  //方法名就是bean的name
        return new CarDao();
    }
}
```

![复制代码](0.9339547207887053-20220831215811-lq9toss.png)[&quot;复制代码&quot;]("复制代码")

运行结果：

![](0.47447205372743717-20220831215811-l39dv3v.png)​

# 七、通过注解@Value获取properties配置

@value注解可以实现为对象赋值，可以直接指定值也可以从properties文件中获取，这里主要讲解两种方式：

## 7.1、使用XML实现

1、在resource目录下创建一个properties文件，如db.properties：

![](0.5534589846364442-20220831215811-lleehxs.png)​

2、在Spring配置文件中导入资源文件

![复制代码](0.14585483010812839-20220831215811-knqmz1b.png)[&quot;复制代码&quot;]("复制代码")

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="
       http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop-4.3.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context-4.3.xsd
       ">
    <context:component-scan base-package="com.zhangguo.spring03.demo.v32"></context:component-scan>
  
    <!--属性点位，location用于指定资源位置，ignore-unresolvable忽视不能解析的内容-->
    <context:property-placeholder location="db.properties" ignore-unresolvable="true"></context:property-placeholder>
</beans>
```

![复制代码](0.06495895476276403-20220831215811-umwvzzz.png)[&quot;复制代码&quot;]("复制代码")

3、通过@Value引用资源文件中的内容

![复制代码](0.8985668302996703-20220831215811-98wahyd.png)[&quot;复制代码&quot;]("复制代码")

```
package com.zhangguo.spring03.demo.v32;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;
import org.springframework.stereotype.Repository;

@Component
public class DbUtils {

    @Value("com.jdbc.driver.Oracle")
    private String driver;
    @Value("${URL}")
    private String location;
    @Value("${userName}")
    private String uid;
    @Value("${password}")
    private String pwd;

    @Override
    public String toString() {
        return "DbUtils{" +
                "driver='" + driver + '\'' +
                ", location='" + location + '\'' +
                ", uid='" + uid + '\'' +
                ", pwd='" + pwd + '\'' +
                '}';
    }

    public String getDriver() {
        return driver;
    }

    public void setDriver(String driver) {
        this.driver = driver;
    }

    public String getLocation() {
        return location;
    }

    public void setLocation(String location) {
        this.location = location;
    }

    public String getUid() {
        return uid;
    }

    public void setUid(String uid) {
        this.uid = uid;
    }

    public String getPwd() {
        return pwd;
    }

    public void setPwd(String pwd) {
        this.pwd = pwd;
    }
}
```

![复制代码](0.06293692312757582-20220831215811-5k19l4p.png)[&quot;复制代码&quot;]("复制代码")

4、测试

![复制代码](0.26179899085670466-20220831215811-mtxbebj.png)[&quot;复制代码&quot;]("复制代码")

```
package com.zhangguo.spring03.demo.v32;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class Client32 {
    public static void main(String[] args) {
        ApplicationContext ctx=new ClassPathXmlApplicationContext("springCfg32.xml");
        UserDao dao=ctx.getBean(UserDao.class);
        System.out.println(dao.dbUtils);
    }
}
```

![复制代码](0.4943615791936049-20220831215811-yjklmg5.png)[&quot;复制代码&quot;]("复制代码")

5、结果

![](0.11841488494249819-20220831215811-hj2dfrw.png)​

## 7.2、使用零配置注解方式实现

1、创建配置类

![复制代码](0.11783811163752644-20220831215811-ycpu3dm.png)[&quot;复制代码&quot;]("复制代码")

```
package com.zhangguo.spring03.demo.v33;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.ImportResource;
import org.springframework.context.annotation.PropertySource;

@Configuration
@ComponentScan("com.zhangguo.spring03.demo.v33")
@PropertySource("db.properties")
public class SpringCfg {

}
```

![复制代码](0.21400367556570443-20220831215811-x83u3dh.png)[&quot;复制代码&quot;]("复制代码")

使用@ImportResource可能会产生的异常（前言中不允许有内容）：

![](0.3035047622434217-20220831215811-oi1ljt0.png) **View Code**

2、测试

![复制代码](0.1115837687223471-20220831215811-nrhinln.png)[&quot;复制代码&quot;]("复制代码")

```
package com.zhangguo.spring03.demo.v33;

import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class Client33 {
    public static void main(String[] args) {
        ApplicationContext ctx=new AnnotationConfigApplicationContext(SpringCfg.class);
        UserDao dao=ctx.getBean(UserDao.class);
        System.out.println(dao.dbUtils);
    }
}
```

![复制代码](0.3087132565634054-20220831215811-sb21w2b.png)[&quot;复制代码&quot;]("复制代码")

3、结果

![](0.6767615099084412-20220831215811-bagvzgt.png)​

## 7.3、其它方式

假如有以下属性文件dev.properties, 需要注入下面的tag

```
tag=123
```

通过PropertyPlaceholderConfigurer

```
<bean class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer">
<property name="location" value="dev.properties" />
</bean>
```

代码

```
@Value("${tag}")
private String tag;
```

通过PreferencesPlaceholderConfigurer

```
<bean id="appConfig" class="org.springframework.beans.factory.config.PreferencesPlaceholderConfigurer">
<property name="location" value="dev.properties" />
</bean>
```

代码：

```
@Value("${tag}")
private String tag;
```

通过PropertiesFactoryBean

```
<bean id="config" class="org.springframework.beans.factory.config.PropertiesFactoryBean">
<property name="location" value="dev.properties" />
</bean>
```

代码：

```
@Value("#{config['tag']}")
private String tag;
```

通过util:properties

效果同PropertiesFactoryBean一样

代码：

```
@Value("#{config['tag']}")
private String tag;
```

# 八、示例下载

[https://git.coding.net/zhangguo5/Spring.git](https://git.coding.net/zhangguo5/Spring.git)

[点击下载](http://files.cnblogs.com/files/best/Spring051.rar)

[https://git.coding.net/zhangguo5/c6_2_SpringIoC.githttps://git.dev.tencent.com/zhangguo5/Spring01.git](https://git.coding.net/zhangguo5/c6_2_SpringIoC.git)

# 九、视频

[https://www.bilibili.com/video/av16071354/](https://www.bilibili.com/video/av16071354/)

# 十、作业

**第一次作业**

1.1、请完成所有基于XML配置Ioc上课示例。

1.2、请学会使用帮助文档

1.3、请定义一个Animal动物类（名称name，重量weight)，定义两个Animal子类猫Cat（品种type)与狗Dog(颜色color)

1.4、以Animal、Cat、Dog为基础练习3.1-3.6的所有知识点

1.5、请写一个微型的Ioc框架（MiniIoc)，实现最基础的控制反转功能（选做）

**第二次作业**

2.1、请完成所有基于注解、自动装配的Ioc上课示例。

2.2、请定义一个Animal动物类（名称name，重量weight)，定义两个Animal子类猫Cat（品种type)与狗Dog(颜色color)

2.3、以Animal、Cat、Dog为基础练习基于注解与自动装配所有知识点，这里使用XML作为配置文件

**第三次作业**

3.1、请完成所有基于零配置实现Ioc上课示例。

3.3、以Animal、Cat、Dog为基础练习基于注解、自动装配与零配置的综合示例，尽量贯穿所有知识点，这里不使用XML作为配置文件

# 十一、总结

@Compent 要求Spring容器管理，被Spring扫描，应用于不确定的功能（宽泛）  
@Repository 应用于数据访问层 DAO  
@Service 应用于业务层  
@Controller 应用于控制层  
注解要被Spring容器管理的类 -> 配置文件指定要扫描的包 ->初始化容器，获得bean

@Autowired 自动装配，字段（成员变量）、方法、属性、构造， 不支持指定名称，配合@Qualifier  
@Resource 自动装配，指定名称，指定类型，不属于Spring javax  
@Qualifier 在自动装配时指定对象的名称，避免多个不唯一的实例

@Autowired()  
@Qualifier("z")  
X x;

@Resource(name="z")  
X x;

X  
@Compent("y")  
Y:X  
@Compent("z")  
Z:X

[« ](https://www.cnblogs.com/best/p/5681511.html)上一篇： [Spring MVC 学习总结（七）——FreeMarker模板引擎与动态页面静态化](https://www.cnblogs.com/best/p/5681511.html "发布于 2016-08-01 08:43")  
[» ](https://www.cnblogs.com/best/p/5736422.html)下一篇： [Spring学习总结（三）——Spring实现AOP的多种方式](https://www.cnblogs.com/best/p/5736422.html "发布于 2016-08-04 13:46")

posted @ 2016-08-02 08:30 [张果](https://www.cnblogs.com/best/) 阅读(69332) 评论(19) [编辑](https://i.cnblogs.com/EditPosts.aspx?postid=5727935) [收藏]()

来源： [https://www.cnblogs.com/best/p/5727935.html](https://www.cnblogs.com/best/p/5727935.html)
