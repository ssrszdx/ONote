# 为什么CPU需要时钟这种概念？

本文同时发表在[https://github.com/zhangyachen/zhangyachen.github.io/issues/132](https://github.com/zhangyachen/zhangyachen.github.io/issues/132)

最近在研究计算机里的基本逻辑电路，想到一个问题：为什么CPU需要时钟这样的概念？

首先考虑如下逻辑电路：  
​![image](0.9291697897159934-20220901140647-m2nd5td.png)​

当A=B=1时，Q=0。当输入信号发生变化时，逻辑元件不会立即对输入变化做出反应，会有一个传播时延（propagation delay）。当B变化为0时，由于B也作为XOR的直接输入，所以XOR异或门会立即感知一个输入变为0的状态变化，XOR输出变为了1。但是由于传播时延的作用，AND与门的输出会过一小段时间才变为0，XOR的输出会在变为1后隔一小段时间重现变为0。表现为下图就是这样：

![image](0.0737949134595457-20220901140647-bnessmi.png)​

上面这种现象叫作空翻（race condition），即指输出中出现了一个不希望有的脉冲信号。

一个简单的办法就是在输出端放置一个边沿触发器：

![image](0.366187684266726-20220901140647-bmsnd63.png)​

边沿触发器的作用就是只有当CLK端输入从0变到1时，数据端D的输入才会影响边沿触发器的输出。这样，所有的传播时延都会被边沿触发器所隐藏掉，这时Q端的输出将变得稳定。比如：

![image](0.407595634255822-20220901140647-lyjus0m.png)​

其中灰色的部分代表没有边沿触发器时的Q端输出状态。我们可以看出，当有了边沿触发器后，Q端的输出变得稳定，基本消除了传播时延。

从上面的例子我们可以看出CPU为什么要时钟：目前绝大多数的微处理器都是被同步时序电路所驱动，而时序电路由各种逻辑门组成。正如上面说的那样，逻辑门需要一小段时间对输入的变化做出反应（propagation delay）。所以需要时钟周期来容纳传播时延，并且时钟周期应当大到需要容纳所有逻辑门的传播时延。

当然，目前也有Asynchronous sequential logic，即不需要时钟信号做同步。但是这种异步逻辑电路虽然速度比同步时序电路快，然而设计起来比同步时序电路复杂的多，并且会遇到上面说的空翻现象（race condition），所以，现在绝大多数的CPU还是需要时钟做信号同步的。

参考资料：

* [Why do Microcontrollers need a Clock](https://electronics.stackexchange.com/questions/176488/why-do-microcontrollers-need-a-clock)
* [Sequential_logic](https://en.m.wikipedia.org/?title=Sequential_logic)
* [编码:隐匿在计算机软硬件背后的语言](https://www.amazon.cn/dp/B009RSXIB4/ref=sr_1_1?ie=UTF8&qid=1514710058&sr=8-1&keywords=%E7%BC%96%E7%A0%81) 第14章反馈与触发器
