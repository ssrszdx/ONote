# 使用 SQL SERVER PROFILER 监测死锁

# [使用 SQL SERVER PROFILER 监测死锁](http://www.cnblogs.com/firstdream/p/4663389.html)

作为DBA，可能经常会遇到有同事或者客户反映经常发生死锁，影响了系统的使用。此时，你需要尽快侦测和处理这类问题。

死锁是当两个或者以上的事务互相阻塞引起的。在这种情况下两个事务会无限期地等待对方释放资源以便操作。下面是死锁的示意图：

![](0.9861007523175691-20220831233837-4rw6615.png)​

本文将使用SQLServer Profiler来跟踪死锁。

# 准备工作：

为了侦测死锁，我们需要先模拟死锁。本例将使用两个不同的会话创建两个事务。

# 步骤：

1、 打开SQLServer Profiler

2、 选择【新建跟踪】，连到实例。

3、 然后选择【空白】模版：

![](0.0609953045776237-20220831233837-0vq4zan.png)​

4、 在【事件选择】页中，展开Locks事件，并选择以下事件：

1、 Deadlock graph

2、 Lock:Deadlock

3、 Lock:Deadlock Chain

![](0.7554279344649393-20220831233837-8ds0ans.png)​

5、 然后打开TSQL事件，并选择以下事件：

1、 SQL:StmtCompleted

2、 SQL:StmtStarting

![](0.45760732521148473-20220831233837-6oyvsrc.png)​

6、 点击【列筛选器】，在跟踪属性中，选择数据库名为需要侦测的数据库，这里使用AdventureWorks。

![](0.5259058777659968-20220831233837-9tegkb2.png)​

7、 在【组织列】中，调整顺序，如下：

![](0.6709085788734617-20220831233837-08kz367.png)​

8、 点击运行。

9、 然后打开SQLServer，并打开两个连接。

10、 在第一个窗口中输入并执行下面脚本：

**[sql]** [view plain](http://blog.csdn.net/dba_huangzj/article/details/8697600 "view plain")​[copy](http://blog.csdn.net/dba_huangzj/article/details/8697600 "copy")​[print](http://blog.csdn.net/dba_huangzj/article/details/8697600 "print")​[?](http://blog.csdn.net/dba_huangzj/article/details/8697600 "?")

1. USE AdventureWorks
2. GO
3. **SET** **TRANSACTION** **ISOLATION** **LEVEL** **REPEATABLE** **READ**
4. GO
5. **BEGIN** **TRANSACTION**
6. **SELECT** *
7. **FROM** Sales.SalesOrderDetail
8. **WHERE** SalesOrderDetailID = 121316

11、 然后在第二个窗口中输入并执行下面脚本：

**[sql]** [view plain](http://blog.csdn.net/dba_huangzj/article/details/8697600 "view plain")​[copy](http://blog.csdn.net/dba_huangzj/article/details/8697600 "copy")​[print](http://blog.csdn.net/dba_huangzj/article/details/8697600 "print")​[?](http://blog.csdn.net/dba_huangzj/article/details/8697600 "?")

1. USE AdventureWorks
2. GO
3. **SET** **TRANSACTION** **ISOLATION** **LEVEL** **REPEATABLE** **READ**
4. **BEGIN** **TRANSACTION**
5. **SELECT** *
6. **FROM** Sales.SalesOrderDetail
7. **WHERE** SalesOrderDetailID = 121317

12、现在回到第一个窗体，并运行下面的脚本：

**[sql]** [view plain](http://blog.csdn.net/dba_huangzj/article/details/8697600 "view plain")​[copy](http://blog.csdn.net/dba_huangzj/article/details/8697600 "copy")​[print](http://blog.csdn.net/dba_huangzj/article/details/8697600 "print")​[?](http://blog.csdn.net/dba_huangzj/article/details/8697600 "?")

1. **UPDATE** Sales.SalesOrderDetail
2. **SET** OrderQty=2
3. **WHERE** SalesOrderDetailID=121317

13、在第二个窗口输入下面语句：

**[sql]** [view plain](http://blog.csdn.net/dba_huangzj/article/details/8697600 "view plain")​[copy](http://blog.csdn.net/dba_huangzj/article/details/8697600 "copy")​[print](http://blog.csdn.net/dba_huangzj/article/details/8697600 "print")​[?](http://blog.csdn.net/dba_huangzj/article/details/8697600 "?")

1. **UPDATE** Sales.SalesOrderDetail
2. **SET** OrderQty=2
3. **WHERE** SalesOrderDetailID=121316

14、 然后在第二个窗口就会看到下面的消息：

![](0.3924119361812646-20220831233837-2ex29vg.png)​

15、切换到SQLServer Profiler，可以看到下面的截图：

![](0.6668613966649273-20220831233837-sdii84r.png)​

16、 点击【Deadlock graph】时间，会显示死锁的图像：

![](0.9365880825768311-20220831233837-6xzcwny.png)​

17、可以保存死锁图像，右键然后选择导出事件数据，并另存为xdl文件：

![](0.3085688444825734-20220831233837-y65uwx8.png)​

下面是其XML格式：

![](0.07880712108740395-20220831233837-aqx81o4.png)​

# 分析：

在本文中，首先创建一个Profiler空白模版，然后选择下面的事件进行监控：

1、 Deadlock graph

2、 Lock:Deadlock

3、 Lock:Deadlock Chain

4、 SQL:StmtCompleted

5、 SQL:StmtStarting

然后通过限定数据库，来限制监控过得对象范围。

在配置好之后，运行跟踪，并在ssms中运行脚本。SQLServer会自动处理和侦测这种类型的死锁。然后会在第二个窗体中收到1205的错误。

在SQLServer Profiler中，演示了如何收集死锁事件，在跟踪结果中可以看到两个事务尝试在一个拥有共享锁的键上添加排它锁。通过死锁图像，可以看到死锁发生的细节。

为了避免或者最小化死锁的发生，有一些建议可以参考：

1、 确保你的事务尽可能地小，这里指范围。

2、 使用较低隔离级别的事务。

3、 对于可能的查询，使用NOLOCK查询提示。

4、 规范化数据库设计。

5、 在需要的列上创建索引，以便是表不需要经常扫描，减少锁问题的发生。

6、 控制数据库对象访问的顺序是相同的顺序。

ps：我用了这个例子，结果没出现死锁，用下面这个例子可以：

在一个窗口输入以下语句：

BEGIN TRAN

USE TEST;

UPDATE T6 SET time3='2013-12-31 00:00:00.000'

WHERE time3='9999-09-09 00:00:00.000'

WAITFOR DELAY '0:0:15'

UPDATE T3 SET VALUE=5

WHERE ID=10

COMMIT TRAN

在另一个窗口输入以下语句：

BEGIN TRAN

USE TEST;

UPDATE T3 SET VALUE=5

WHERE ID=10

WAITFOR DELAY '0:0:15'

UPDATE T6 SET time3='2013-12-31 00:00:00.000'

WHERE time3='9999-09-09 00:00:00.000'

COMMIT TRAN

在第一个窗口执行后，在15秒之内执行第二个窗口的查询。

会发生死锁。

事务的执行过程：

①：窗口1要更新T6的数据，需要向数据库引擎申请T6的排他锁。

②：窗口2要更新T3的数据，需要向数据库引擎申请T3的排他锁。

因为请求锁的对象不同，所以它们可以同时得到想要的锁。

③：在窗口2还没有完成对T3更新前，窗口1完成了对T6的更新，然后想要更新T3的数据，此时需要请求T3的排他锁。由于排他锁与窗口2上面的T3上面的排他锁不兼容，所以窗口1必须等待窗口2执行完事务，然后放在T3上面的排他锁后，才能获得对T3的排他锁。

④：窗口2想要申请T6上的排他锁同上面道理一样。

双方都等待对方释放资源，才能继续事务操作，从而形成死锁。

例子中使用WAITFOR语句在执行两条语句间休息5秒，可以增加死锁的概率。

数据库引擎会定期检索死锁，一旦发现问题，会选取其中一个事务作为牺牲品，终止事务的运行，从而释放资源。

被牺牲的事务会显示如下信息：

(1 行受影响)

消息 1205，级别 13，状态 45，第 7 行

事务(进程 ID 55)与另一个进程被死锁在 锁 资源上，并且已被选作死锁牺牲品。请重新运行该事务。

‘1行受影响’表示第一个UPDATE语句已经执行成功。但是由于事务没有成功提交，所以回滚掉了。
