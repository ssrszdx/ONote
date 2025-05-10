# Spring Boot 发展历史

### 

1996年 , 发布了java bean 1.00-A  当时的java bean有什么用呢

*javaBean最初是为Java GUI的可视化编程实现的.你拖动IDE构建工具创建一个GUI 组件（如多选框）,其实是工具给你创建java类,并提供将类的属性暴露出来给你修改调整,将事件监听器暴露出来*

*具体规范如下:*

1、所有属性为private , 使用public的get,set 方法访问

2、提供默认构造方法

3、提供getter和setter

4、实现serializable接口

**在实际企业开发中,需要实现事务,安全,分布式, javabean就不好用了.sun公司就开始往上面堆功能,这里java bean就变为复杂为EJB;**

**EJB (Enterprise Java Beans)企业javaBeans  是JavaEE的组成部分, 是核心规范, 是核心技术之一**

EJB设计目标: 部署分布式应用程序

EJB用途: 构筑企业级应用的服务端

它们提供了一个框架来开发和实施分布式商务逻辑，由此很显著地简化了具有可伸缩性和高度复杂的企业级应用的开发。EJB规范定义了EJB组件在何时如何与它们的容器进行交互作用。容器负责提供公用的服务，例如目录服务、事务管理、安全性、资源缓冲池以及容错性。但这里值得注意的是，EJB并不是实现JAVAEE的唯一途径。正是由于JAVAEE的开放性，使得有的厂商能够以一种和EJB平行的方式来达到同样的目的。

EJB容器可接受的三类EJB

会话Bean (session beans)

 实体Bean (entity beans)

 消息驱动Bean (Message Driven Beans)

早期EJB存在诸多弊端,

1 "以规范为驱动" 只是不断添加规范, 并没有针对性地解决问题, 反而在实际开发中引入很多复杂性

2 违反"帕累托法则" , 也称" 二八法则" , 是指花较少的(10%~20%) 的成本解决大部分问题(80%~90%)

3 引入重复代码, 编成模型复杂,开发周期长,移植困难,等问题

Rod johnson "spring之父" 洞察到JavaEE开发上的弊端, 从而推出现在热衷Spring 框架

 Spring 开发理念,从实际需求出发,着眼构建轻便, 灵巧并易于开发, 测试和部署的轻量级开发框架

主要表现如下几个方面

1 轻量级IoC容器

2 AOP编程方式 (Aspect Oriented Programming, 面向切面编程)

3 大量使用注解

4 避免重复"造轮子"

IoC+ AOP技术, 通过简单的java bean也能完成EJB的事情, 这里的javaBean简化为POJO

POJO (Plain Ordinary Java Object ) 简单的javaBeans , 是为了避免混淆和EJB所创造的javaBeans的简称

受到spring影响, EJB3.0也使用了所谓的" 传统简单Java对象", 支持依赖注入, 规范大幅采用java注解(annotation), 从而消减此前EJB编程的冗杂性
