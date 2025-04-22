# Java性能分析神器-JProfiler详解

前段时间在给公司项目做性能分析，从简单的分析Log（GC log, postgrep log, [hibernate](http://lib.csdn.net/base/javaee "Java EE知识库") statitistic），到通过AOP搜集软件运行数据，再到PET[测试](http://lib.csdn.net/base/softwaretest "软件测试知识库")，感觉时间花了不少，性能也有一定的提升，但总感觉像是工作在原始时代，无法简单顺畅，又无比清晰的获得想要的结果。遂花费了一定的时间，从新梳理学习了一下之前用过的关于jvm调优和内存分析的各种工具，包括JDK自带的jps, jstack, jmap, jconsole，以及IBM的HeapAnalyzer等，这些工具虽然提供了不少功能，但其可用度，便捷度，远没达到IntelliJ之于[Java](http://lib.csdn.net/base/java "Java 知识库")开发那种地步。在偶然情况下，在云栖社区上发现有人推荐Jprofiler，装上使用版一用，发现果然是神器，特此推荐给大家。先声明，这个软件是商用的，网上有很多关于lisence的帖子，**我这里转发，但是绝不推荐大家用破解版！**

L-Larry_Lau@163.com#36573-fdkscp15axjj6#25257  
L-Larry_Lau@163.com#5481-ucjn4a16rvd98#6038  
L-Larry_Lau@163.com#99016-hli5ay1ylizjj#27215  
L-Larry_Lau@163.com#40775-3wle0g1uin5c1#0674  
L-Larry_Lau@163.com#7009-14frku31ynzpfr#20176  
L-Larry_Lau@163.com#49604-1jfe58we9gyb6#5814  
L-Larry_Lau@163.com#25531-1qcev4yintqkj#23927  
L-Larry_Lau@163.com#96496-1qsu1lb1jz7g8w#23479  
L-Larry_Lau@163.com#20948-11amlvg181cw0p#171159

然后，先转一篇云栖上的文章，然后再慢慢开始我们的Jprofiler之旅。

## 一.JProfiler是什么

JProfiler是由ej-technologies GmbH公司开发的一款性能瓶颈分析工具(该公司还开发部署工具)。  
其特点:

* 使用方便
* 界面操作友好
* 对被分析的应用影响小
* CPU,Thread,Memory分析功能尤其强大
* 支持对jdbc,noSql, jsp, servlet, socket等进行分析
* 支持多种模式(离线，在线)的分析
* 跨平台 ![_2014_10_06_7_54_34_PM](0.42888417838278703-20220831221721-918zs7s.png "_2014_10_06_7_54_34_PM")[http://img1.tbcdn.cn/L1/461/1/f71a75090d48e46eb809001918d37d7cc8d5ec90?spm=5176.100239.blogcont276.6.pMeguT](http://img1.tbcdn.cn/L1/461/1/f71a75090d48e46eb809001918d37d7cc8d5ec90?spm=5176.100239.blogcont276.6.pMeguT) (图1)

## 二.数据采集

Q1. JProfiler既然是一款性能瓶颈分析工具，这些分析的相关数据来自于哪里？  
Q2. JProfiler是怎么将这些数据收集并展现的?

![_2014_10_06_3_09_15_PM](0.5616119720998445-20220831221721-ektz1o5.png "_2014_10_06_3_09_15_PM")[http://img4.tbcdn.cn/L1/461/1/774e1de366c3dced5bf97ab0cd34471ec9a99537?spm=5176.100239.blogcont276.7.pMeguT](http://img4.tbcdn.cn/L1/461/1/774e1de366c3dced5bf97ab0cd34471ec9a99537?spm=5176.100239.blogcont276.7.pMeguT)  
(图2)

A1. 分析的数据主要来自于下面俩部分

1. 一部分来自于jvm的分析接口**JVMTI**(JVM Tool Interface) , JDK必须>=1.6

> JVMTI is an event-based system. The profiling agent library can register handler functions for different events. It can then enable or disable selected events

例如: 对象的生命周期，thread的生命周期等信息  
2. 一部分来自于instruments classes(可理解为class的重写,增加JProfiler相关统计功能)  
例如：方法执行时间，次数，方法栈等信息

A2. 数据收集的原理如图2

1. 用户在JProfiler GUI中下达监控的指令(一般就是点击某个按钮)
2. JProfiler GUI JVM 通过socket(默认端口8849)，发送指令给被分析的jvm中的JProfile Agent。
3. JProfiler Agent(如果不清楚Agent请看文章第三部分"启动模式") 收到指令后，将该指令转换成相关需要监听的事件或者指令,来注册到JVMTI上或者直接让JVMTI去执行某功能(例如dump jvm内存)
4. JVMTI 根据注册的事件，来收集当前jvm的相关信息。 例如: 线程的生命周期; jvm的生命周期;classes的生命周期;对象实例的生命周期;堆内存的实时信息等等
5. JProfiler Agent将采集好的信息保存到**内存**中，按照一定规则统计好(如果发送所有数据JProfiler GUI，会对被分析的应用网络产生比较大的影响)
6. 返回给JProfiler GUI Socket.
7. JProfiler GUI Socket 将收到的信息返回 JProfiler GUI Render
8. JProfiler GUI Render 渲染成最终的展示效果

## 三. 数据采集方式和启动模式

A1. JProfier采集方式分为两种：Sampling(样本采集)和Instrumentation

* Sampling: 类似于样本统计, 每隔一定时间(5ms)将每个线程栈中方法栈中的信息统计出来。优点是对应用影响小(即使你不配置任何Filter, Filter可参考文章第四部分)，缺点是一些数据/特性不能提供(例如:方法的调用次数)
* Instrumentation: 在class加载之前，JProfier把相关功能代码写入到需要分析的class中，对正在运行的jvm有一定影响。优点: 功能强大，但如果需要分析的class多，那么对应用影响较大，一般配合Filter一起使用。所以一般JRE class和framework的class是在Filter中通常会过滤掉。

注: JProfiler本身没有指出数据的采集类型，这里的采集类型是针对方法调用的采集类型 。因为JProfiler的绝大多数核心功能都依赖方法调用采集的数据, 所以可以直接认为是JProfiler的数据采集类型。

A2: 启动模式:

* Attach mode  
  可直接将本机正在运行的jvm加载JProfiler Agent. 优点是很方便，缺点是一些特性不能支持。如果选择Instrumentation数据采集方式，那么需要花一些额外时间来重写需要分析的class。
* Profile at startup  
  在被分析的jvm启动时，将指定的JProfiler Agent手动加载到该jvm。JProfiler GUI 将收集信息类型和策略等配置信息通过socket发送给JProfiler Agent，收到这些信息后该jvm才会启动。  
  在被分析的jvm 的启动参数增加下面内容：  
  语法: -agentpath:[path to jprofilerti library]  
  【注】: 语法不清楚没关系，JProfiler提供了帮助向导.  
  ![_2014_10_06_4_15_42_PM](0.8821852363565823-20220831221721-fio9fbp.png "_2014_10_06_4_15_42_PM")[http://img1.tbcdn.cn/L1/461/1/af3a9d42a43abf41a676e194dad2524c651b213c?spm=5176.100239.blogcont276.8.pMeguT](http://img1.tbcdn.cn/L1/461/1/af3a9d42a43abf41a676e194dad2524c651b213c?spm=5176.100239.blogcont276.8.pMeguT)  
  (图3)
* Prepare for profiling:  
  和Profile at startup的主要区别：被分析的jvm不需要收到JProfiler GUI 的相关配置信息就可以启动。
* Offline profiling  
  一般用于适用于不能直接调试线上的场景。Offline profiling需要将信息采集内容和策略(一些Trigger, Trigger请参考文章第五部分)打包成一个配置文件(config.xml)，在线上启动该jvm 加载 JProfiler Agent时，加载该xml。那么JProfiler Agent会根据Trigger的类型会生成不同的信息。例如: heap dump; thread dump; method call record等  
  语法:  
  -agentpath:/home/2080/jprofiler8/bin/[Linux](http://lib.csdn.net/base/linux "Linux知识库")-x64/libjprofilerti.so=offline,id=151,config=/home/2080/config.xml  
  【注】: config.xml中的每一个被分析的jvm的采集信息都有一个id来标识。  
  下面是使用了离线模式，并使用了每隔一秒dump heap 的Trigger：  
  ![_2014_10_06_9_07_56_PM](0.6288076915810106-20220831221721-wapil2m.png "_2014_10_06_9_07_56_PM")[http://img2.tbcdn.cn/L1/461/1/93ca30653b599d9a8564dd05e3971d8078e9ec16?spm=5176.100239.blogcont276.9.pMeguT](http://img2.tbcdn.cn/L1/461/1/93ca30653b599d9a8564dd05e3971d8078e9ec16?spm=5176.100239.blogcont276.9.pMeguT)

## 四. JProfiler核心概念

1. Filter: 什么class需要被分析。分为包含和不包含两种类型的Filter。  
    ![_2014_10_06_4_32_15_PM](0.8823276341914117-20220831221721-xxz5ibk.png "_2014_10_06_4_32_15_PM")[http://img2.tbcdn.cn/L1/461/1/c6490044d51af9e36d86c7c59774a26bf68934d8?spm=5176.100239.blogcont276.10.pMeguT](http://img2.tbcdn.cn/L1/461/1/c6490044d51af9e36d86c7c59774a26bf68934d8?spm=5176.100239.blogcont276.10.pMeguT)  
    (图4)
2. Profiling Settings: 收据收集的策略:Sampling和 Instrumentation，一些数据采集细节可以自定义.  
    ![_2014_10_06_4_34_28_PM](0.7896654810793962-20220831221721-ezlh8cb.png "_2014_10_06_4_34_28_PM")[http://img3.tbcdn.cn/L1/461/1/267a5a432ded52a220742608122e125f73810db1?spm=5176.100239.blogcont276.11.pMeguT](http://img3.tbcdn.cn/L1/461/1/267a5a432ded52a220742608122e125f73810db1?spm=5176.100239.blogcont276.11.pMeguT)  
    (图5)
3. Triggers: 一般用于**offline**模式，告知JProfiler Agent 什么时候触发什么行为来收集指定信息.  
    ![_2014_10_06_4_37_47_PM](0.07267586394852299-20220831221721-5hikwyj.png "_2014_10_06_4_37_47_PM")[http://img1.tbcdn.cn/L1/461/1/e69504d0b635fae209f5672e0b2a271b5354e87a?spm=5176.100239.blogcont276.12.pMeguT](http://img1.tbcdn.cn/L1/461/1/e69504d0b635fae209f5672e0b2a271b5354e87a?spm=5176.100239.blogcont276.12.pMeguT)  
    (图6)
4. Live memory: class/class instance的相关信息。 例如对象的个数，大小，对象创建的方法执行栈，对象创建的热点。  
    ![_2014_10_06_4_56_09_PM](0.49363439623887473-20220831221721-tf3gph0.png "_2014_10_06_4_56_09_PM")[http://img2.tbcdn.cn/L1/461/1/e8a52b590d0f4631058ca328fe0ff691c0d3aa89?spm=5176.100239.blogcont276.13.pMeguT](http://img2.tbcdn.cn/L1/461/1/e8a52b590d0f4631058ca328fe0ff691c0d3aa89?spm=5176.100239.blogcont276.13.pMeguT)  
    (图7)
5. Heap walker: 对一定时间内收集的内存对像信息进行静态分析，功能强大且使用。包含对象的outgoing reference, incoming reference, biggest object等  
    ![_2014_10_06_4_55_39_PM](0.01792049258433326-20220831221721-65eivdu.png "_2014_10_06_4_55_39_PM")[http://img2.tbcdn.cn/L1/461/1/c85b6b5e6880bab6b7ead4b0a673b8e1575b0158?spm=5176.100239.blogcont276.14.pMeguT](http://img2.tbcdn.cn/L1/461/1/c85b6b5e6880bab6b7ead4b0a673b8e1575b0158?spm=5176.100239.blogcont276.14.pMeguT)  
    (图8)
6. CPU views: CPU消耗的分布及时间(cpu时间或者运行时间); 方法的执行图; 方法的执行统计(最大，最小，平均运行时间等)  
    ![_2014_10_06_4_54_48_PM](0.10553047416700667-20220831221721-ub8rd9w.png "_2014_10_06_4_54_48_PM")[http://img3.tbcdn.cn/L1/461/1/31d669359bf4291f61c8ba7436374134936994ba?spm=5176.100239.blogcont276.15.pMeguT](http://img3.tbcdn.cn/L1/461/1/31d669359bf4291f61c8ba7436374134936994ba?spm=5176.100239.blogcont276.15.pMeguT)  
    (图9)
7. Thread: 当前jvm所有线程的运行状态，线程持有锁的状态，可dump线程。  
    ![_2014_10_06_4_54_07_PM](0.9354989331252852-20220831221721-gqsnyk6.png "_2014_10_06_4_54_07_PM")[http://img2.tbcdn.cn/L1/461/1/b8fef844181952665612a3ae9a23864d8eb0ec01?spm=5176.100239.blogcont276.16.pMeguT](http://img2.tbcdn.cn/L1/461/1/b8fef844181952665612a3ae9a23864d8eb0ec01?spm=5176.100239.blogcont276.16.pMeguT)  
    (图10)
8. Monitors & locks: 所有线程持有锁的情况以及锁的信息  
    ![_2014_10_06_4_59_15_PM](0.06758803128572466-20220831221721-um6kglj.png "_2014_10_06_4_59_15_PM")[http://img3.tbcdn.cn/L1/461/1/72e01bf3f2bdec2f6b05ce156a379ecb913f89e0?spm=5176.100239.blogcont276.17.pMeguT](http://img3.tbcdn.cn/L1/461/1/72e01bf3f2bdec2f6b05ce156a379ecb913f89e0?spm=5176.100239.blogcont276.17.pMeguT)  
    (图11)
9. Telemetries: 包含heap, thread, gc, class等的趋势图(遥测视图)

## 五. 实践

为了方便实践，直接以JProfiler8自带的一个例子来帮助理解上面的相关概念。  
JProfiler 自带的例子如下：模拟了内存泄露和线程阻塞的场景：  
具体源码参考: /jprofiler install path/demo/bezier

![_2014_10_06_5_06_49_PM](0.9224286651735305-20220831221721-78po9se.png "_2014_10_06_5_06_49_PM")[http://img3.tbcdn.cn/L1/461/1/e95ff007af328eb31b4f9fb4d9d888bffdfe1d29?spm=5176.100239.blogcont276.18.pMeguT](http://img3.tbcdn.cn/L1/461/1/e95ff007af328eb31b4f9fb4d9d888bffdfe1d29?spm=5176.100239.blogcont276.18.pMeguT)  
(图12 )

![_2014_10_06_5_09_43_PM](0.350965424116777-20220831221721-olucqi2.png "_2014_10_06_5_09_43_PM")[http://img4.tbcdn.cn/L1/461/1/375e6515445717a1bb110738a17e61ee3de1e2aa?spm=5176.100239.blogcont276.19.pMeguT](http://img4.tbcdn.cn/L1/461/1/375e6515445717a1bb110738a17e61ee3de1e2aa?spm=5176.100239.blogcont276.19.pMeguT)  
(图13 Leak Memory 模拟内存泄露, Simulate blocking 模拟线程间锁的阻塞)

A1. 首先来分析下内存泄露的场景:(勾选图13中 Leak Memory 模拟内存泄露)

1. 在**Telemetries-&gt; Memory**视图中你会看到大致如下图的场景(在看的过程中可以间隔一段时间去执行Run GC这个功能)：看到下图蓝色区域,老生代在gc后(**波谷**)内存的大小在慢慢的增加(理想情况下，这个值应该是稳定的)  
    ![_2014_10_06_5_18_56_PM](0.909460566273566-20220831221721-5mmtydd.png "_2014_10_06_5_18_56_PM")[http://img1.tbcdn.cn/L1/461/1/c4c3c21a29874988408786f3c62f2953f713594a?spm=5176.100239.blogcont276.20.pMeguT](http://img1.tbcdn.cn/L1/461/1/c4c3c21a29874988408786f3c62f2953f713594a?spm=5176.100239.blogcont276.20.pMeguT)  
    (图14)
2. 在 Live memory->Recorded Objects 中点击**record allocation data**按钮，开始统计一段时间内创建的对象信息。执行一次**Run GC**后看看当前对象信息的大小，并点击工具栏中**Mark Current**按钮(其实就是给当前对象数量打个标记。执行一次Run GC，然后再继续观察;执行一次Run GC，然后再继续观察...。最后看看哪些对象在不断GC后，数量还一直上涨的。最后你看到的信息可能和下图类似  
    ![_2014_10_06_5_26_41_PM](0.9208046796845-20220831221721-9cvabhi.png "_2014_10_06_5_26_41_PM")[http://img1.tbcdn.cn/L1/461/1/65ae84a68f6d81a46064d55fb88eb658742f3241?spm=5176.100239.blogcont276.21.pMeguT](http://img1.tbcdn.cn/L1/461/1/65ae84a68f6d81a46064d55fb88eb658742f3241?spm=5176.100239.blogcont276.21.pMeguT)  
    (图15 绿色是标记前的数量，红色是标记后的增量)
3. 在Heap walker中分析刚才记录的对象信息  
    ![_2014_10_06_5_28_00_PM](0.6709194976735227-20220831221721-halx8nn.png "_2014_10_06_5_28_00_PM")[http://img4.tbcdn.cn/L1/461/1/de3a4d18921259d1767bd7ac0c05fe02678b3c26?spm=5176.100239.blogcont276.22.pMeguT](http://img4.tbcdn.cn/L1/461/1/de3a4d18921259d1767bd7ac0c05fe02678b3c26?spm=5176.100239.blogcont276.22.pMeguT)  
    (图16)  
    ![_2014_10_06_5_29_11_PM](0.26001207625981193-20220831221721-82ypdhx.png "_2014_10_06_5_29_11_PM")[http://img3.tbcdn.cn/L1/461/1/a0693872e6fedb18d4ce3b94dea07bef2c35468e?spm=5176.100239.blogcont276.23.pMeguT](http://img3.tbcdn.cn/L1/461/1/a0693872e6fedb18d4ce3b94dea07bef2c35468e?spm=5176.100239.blogcont276.23.pMeguT)  
    (图17)
4. 点击上图中实例最多的class，右键**Use Selected Instances-&gt;Reference-&gt;Incoming Reference**.  
    发现该Long数据最终是存放在**bezier.BeaierAnim.leakMap**中。  
    ![_2014_10_06_5_31_54_PM](0.40827722284304735-20220831221721-m9t6p8r.png "_2014_10_06_5_31_54_PM")[http://img1.tbcdn.cn/L1/461/1/de60d0d5dcb88017ed0dbda668b921c43bca869a?spm=5176.100239.blogcont276.24.pMeguT](http://img1.tbcdn.cn/L1/461/1/de60d0d5dcb88017ed0dbda668b921c43bca869a?spm=5176.100239.blogcont276.24.pMeguT)  
    (图18)

在Allocations tab项中，右键点击其中的某个方法，可查看到具体的源码信息.  
![_2014_10_06_5_36_08_PM](0.672443295812527-20220831221721-0sfjum1.png "_2014_10_06_5_36_08_PM")[http://img2.tbcdn.cn/L1/461/1/ffb374dd45b1cf006f689fa56d2f4fbff3c85d1c?spm=5176.100239.blogcont276.25.pMeguT](http://img2.tbcdn.cn/L1/461/1/ffb374dd45b1cf006f689fa56d2f4fbff3c85d1c?spm=5176.100239.blogcont276.25.pMeguT)  
(图19)

【注】:到这里问题已经非常清楚了，明白了在图17中为什么哪些实例的数量是一样多，并且为什么内存在fullgc后还是回收不了(一个old 区的对象leakMap，put的信息也会进入old区, leakMap如回收不掉，那么该map中包含的对象也回收不掉)。

A2. 模拟线程阻塞的场景(勾选图13中Simulate blocking 模拟线程间锁的阻塞)  
为了方便区分线程，我将Demo中的BezierAnim.java的L236的线程命名为test

```
public void start() {
            thread = new Thread(this, "test");
            thread.setPriority(Thread.MIN_PRIORITY);
            thread.start();
        }
```

正常情况下，如下图  
![_2014_10_06_5_50_25_PM](0.9156086313273555-20220831221721-p0wr056.png "_2014_10_06_5_50_25_PM")[http://img2.tbcdn.cn/L1/461/1/603df4fd16b151739dbafedc0631fd3036b5fe36?spm=5176.100239.blogcont276.26.pMeguT](http://img2.tbcdn.cn/L1/461/1/603df4fd16b151739dbafedc0631fd3036b5fe36?spm=5176.100239.blogcont276.26.pMeguT)  
(图20)

勾选了Demo中"Simulate blocking"选项后，如下图(注意看下下图中的状态图标), test线程block状态明显增加了。  
![_2014_10_06_5_53_19_PM](0.5199104731794839-20220831221721-ooozrq4.png "_2014_10_06_5_53_19_PM")[http://img4.tbcdn.cn/L1/461/1/e7e80aa09ecca398679da298a4f6291e10030a03?spm=5176.100239.blogcont276.27.pMeguT](http://img4.tbcdn.cn/L1/461/1/e7e80aa09ecca398679da298a4f6291e10030a03?spm=5176.100239.blogcont276.27.pMeguT)  
(图21)

在**Monitors &amp; locks-&gt;Monitor History**观察了一段时间后，会发现有4种发生锁的情况。

第一种:  
AWT-EventQueue-0 线程持有一个Object的锁，并且处于Waiting状态。

图下方的代码提示出Demo.block方法调用了object.wait方法。这个还是比较容易理解的。  
![_2014_10_06_6_15_59_PM](0.6193102496242977-20220831221721-c1db2z5.png "_2014_10_06_6_15_59_PM")[http://img1.tbcdn.cn/L1/461/1/03ca5479b6b877cc9dc77029cbcb771ef0129e73?spm=5176.100239.blogcont276.28.pMeguT](http://img1.tbcdn.cn/L1/461/1/03ca5479b6b877cc9dc77029cbcb771ef0129e73?spm=5176.100239.blogcont276.28.pMeguT)  
(图22)

第二种:  
AWT-EventQueue-0占有了bezier.BezierAnim$Demo实例上的锁，而test线程等待该线程释放。

注意下图中下方的源代码, 这种锁的出现原因是Demo的blcok方法在AWT和test线程  
都会被执行，并且该方法是synchronized.  
![_2014_10_06_6_11_20_PM](0.44345224711501907-20220831221721-nt6myf5.png "_2014_10_06_6_11_20_PM")[http://img1.tbcdn.cn/L1/461/1/df7c092e85b1ae9db0bd98376b36c24cddff2b8b?spm=5176.100239.blogcont276.29.pMeguT](http://img1.tbcdn.cn/L1/461/1/df7c092e85b1ae9db0bd98376b36c24cddff2b8b?spm=5176.100239.blogcont276.29.pMeguT)  
(图23)

第三种和第四种:  
test线程中会不断向事件Event Dispatching Thread提交任务，导致竞争java.awt.EventQueue对象锁。  
提交任务的方式是下面的代码:`repaint()`和`EventQueue.invokeLater`

```
        public void run() {
            Thread me = Thread.currentThread();
            while (thread == me) {
                repaint();
                if (block) {
                    block(false);
                }
                try {
                    Thread.sleep(10);
                } catch (Exception e) {
                    break;
                }
                EventQueue.invokeLater(new Runnable() {
                    @Override
                    public void run() {
                        onEDTMethod();
                    }
                });
            }
            thread = null;
        }
```

![_2014_10_06_6_20_05_PM](0.8023068742782795-20220831221721-lbttmvr.png "_2014_10_06_6_20_05_PM")[http://img4.tbcdn.cn/L1/461/1/76f14916d07736bdf51db282045550c83d580fef?spm=5176.100239.blogcont276.30.pMeguT](http://img4.tbcdn.cn/L1/461/1/76f14916d07736bdf51db282045550c83d580fef?spm=5176.100239.blogcont276.30.pMeguT)  
(图24)

## 六. 最佳实践

1. JProfiler都会对一些特殊操作给予提示，这时候最好仔细阅读下说明.
2. "Mark Current"功能在某些场景很有效
3. Heap walker一般是静态分析在Live memory->Recorder objects的对象信息，这些信息可能会被GC回收掉，导致Heap walker中什么也没有显示出来。这种现象是正常的。
4. 可以才工具栏中Start Recordings配置一次性收集的信息
5. Filter中include和exclude是有顺序的，注意使用下图**左下方**的**“Show Filter Tree”**来验证一下顺序 ![_2014_10_17_3_41_18_PM](0.28455013788180183-20220831221721-eqdkjtl.png "_2014_10_17_3_41_18_PM")[http://img4.tbcdn.cn/L1/461/1/72bac3b3a3ce644f30f48b7b6eb6c371c7aa5a6f?spm=5176.100239.blogcont276.31.pMeguT](http://img4.tbcdn.cn/L1/461/1/72bac3b3a3ce644f30f48b7b6eb6c371c7aa5a6f?spm=5176.100239.blogcont276.31.pMeguT) (图25)

## 七. 参考文献

1. JProfiler helper: [http://resources.ej-technologies.com/jprofiler/help/doc/index.html](http://resources.ej-technologies.com/jprofiler/help/doc/index.html?spm=5176.100239.blogcont276.32.pMeguT)
2. JVMTI: [http://docs.oracle.com/javase/7/docs/platform/jvmti/jvmti.html](http://docs.oracle.com/javase/7/docs/platform/jvmti/jvmti.html?spm=5176.100239.blogcont276.33.pMeguT)
