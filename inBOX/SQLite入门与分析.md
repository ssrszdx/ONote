# SQLite入门与分析

# [SQLite入门与分析(一)---简介](http://www.cnblogs.com/hustcat/archive/2009/02/12/1389448.html)

 写在前面：出于项目的需要,最近打算对SQLite的内核进行一个完整的剖析,在此希望和对SQLite有兴趣的一起交流。我知道，这是一个漫长的过程，就像曾经去读Linux内核一样，这个过程也将是辛苦的，但我相信结果一定是美好的... ...接下来是第一章。

1、SQLite介绍

自几十年前出现的商业应用程序以来，数据库就成为软件应用程序的主要组成部分。正与数据库管理系统非常关键一样，它们也变得非常庞大，并占用了相当多的系统资源，增加了管理的复杂性。随着软件应用程序逐渐模块模块化，一种新型数据库会比大型复杂的传统数据库管理系统更适应。嵌入式数据库直接在应用程序进程中运行，提供了零配置（zero-configuration）运行模式，并且资源占用非常少。  
SQLite是一个开源的嵌入式关系数据库，它在2000年由D. Richard Hipp发布，它的减少应用程序管理数据的开销，SQLite可移植性好，很容易使用，很小，高效而且可靠。  
SQLite嵌入到使用它的应用程序中，它们共用相同的进程空间，而不是单独的一个进程。从外部看，它并不像一个RDBMS，但在进程内部，它却是完整的，自包含的数据库引擎。  
嵌入式数据库的一大好处就是在你的程序内部不需要网络配置，也不需要管理。因为客户端和服务器在同一进程空间运行。SQLite 的数据库权限只依赖于文件系统，没有用户帐户的概念。SQLite 有数据库级锁定，没有网络服务器。它需要的内存，其它开销很小，适合用于嵌入式设备。你需要做的仅仅是把它正确的编译到你的程序。

2、架构(architecture)

SQLite采用了模块的设计，它由三个子系统，包括8个独立的模块构成。  
 ![](8c5b0e744bcd350c443e2158326b6d25-20220901162154-mbe14af.jpg)  
2.1、接口(Interface)  
接口由SQLite C API组成，也就是说不管是程序、脚本语言还是库文件，最终都是通过它与SQLite交互的(我们通常用得较多的ODBC/JDBC最后也会转化为相应C API的调用)。

2.2、编译器(Compiler)  
在编译器中，分词器（Tokenizer）和分析器(Parser)对SQL进行语法检查，然后把它转化为底层能更方便处理的分层的数据结构---语法树，然后把语法树传给代码生成器(code generator)进行处理。而代码生成器根据它生成一种针对SQLite的汇编代码，最后由虚拟机(Virtual Machine)执行。

2.3、虚拟机(Virtual Machine)  
架构中最核心的部分是虚拟机，或者叫做虚拟数据库引擎(Virtual Database Engine,VDBE)。它和Java虚拟机相似，解释执行字节代码。VDBE的字节代码由128个操作码(opcodes)构成，它们主要集中在数据库操作。它的每一条指令都用来完成特定的数据库操作(比如打开一个表的游标)或者为这些操作栈空间的准备(比如压入参数)。总之，所有的这些指令都是为了满足SQL命令的要求(关于VM，后面会做详细介绍)。

2.4、后端(Back-End)  
后端由B-树(B-tree)，页缓存(page cache，pager)和操作系统接口(即系统调用)构成。B-tree和page cache共同对数据进行管理。B-tree的主要功能就是索引，它维护着各个页面之间的复杂的关系，便于快速找到所需数据。而pager的主要作用就是通过OS接口在B-tree和Disk之间传递页面。

3、SQLite的特点(SQLite’s Features and Philosophy)  
3.1、零配置(Zero Configuration)  
3.2、可移植(Portability)：  
它是运行在Windows,Linux,BSD,Mac OS X和一些商用Unix系统，比如Sun的Solaris,IBM的AIX，同样，它也可以工作在许多嵌入式操作系统下，比如QNX,VxWorks,Palm OS, Symbin和Windows CE。  
3.3、Compactness：  
SQLite是被设计成轻量级，自包含的。one header file, one library, and you’re relational, no external database server required  
3.4、简单(Simplicity)  
3.5、灵活(Flexibility)  
3.6、可靠(Reliability)：  
SQLite的核心大约有3万行标准C代码，这些代码都是模块化的，很容易阅读。

主要参考:The Definitive Guide to SQLite

# [SQLite入门与分析(二)---设计与概念](http://www.cnblogs.com/hustcat/archive/2009/02/13/1390340.html)

写在前面:谢谢各位的关注,没想到会有这么多人关注。高兴的同时，也感到压力，因为我接触SQLite也就几天，也没在实际开发中用过，只是最近项目的需求才来研究它，所以我很担心自己的文章是否会有错误，误导别人。但是我很想把自己的学习成果与大家分享，所以如果大家觉得我有不对的地方，望不吝赐教。  
我原打算直接从VDBE入手的，因为它起着承上启下的作用，是整个SQLite的核心，并分析源码，但考虑到这是一个系列的文章，我希望能把问题说全，所以还是从基本概念入手，对于初学者，如果没有这些概念，是很继续下去的。好了，下面开始第二章，由于这一章内容很多，我将分两部分讨论，下面开始第一部分。

1、 API  
由两部分组成: 核心API(core API) 和扩展API（extension API）  
核心API的函数实现基本的数据库操作：连接数据库，处理SQL，遍历结果集。它也包括一些实用函数，比如字符串转换，操作控制，调试和错误处理。  
扩展API通过创建你自定义的SQL函数去扩展SQLite。

1.1、SQLite Version 3的一些新特点：  
(1)SQLite的API全部重新设计，由第二版的15个函数增加到88个函数。这些函数包括支持UTF-8和UTF-16编码的功能函数。  
(2)改进并发性能。加锁子系统引进一种锁升级模型(lock escalation model)，解决了第二版的写进程饿死的问题(该问题是任何一个DBMS必须面对的问题)。这种模型保证写进程按照先来先服务的算法得到排斥锁(Exclusive Lock)。甚至，写进程通过把结果写入临时缓冲区(Temporary Buffer)，可以在得到排斥锁之前就能开始工作。这对于写要求较高的应用，性能可提高400%（引自参考文献）。  
(3)改进的B-树。对于表采用B+树，大大提高查询效率。  
(4)SQLite 3最重要的改变是它的存储模型。由第二版只支持文本模型，扩展到支持5种本地数据类型。  
总之，SQLite Version 3与SQLite Vertion 2有很大的不同，在灵活性，特点和性能方面有很大的改进。

1.2、主要的数据结构(The Principal Data Structures)  
SQLite由很多部分组成－parser,tokenize,virtual machine等等。但是从程序员的角度，最需要知道的是:connection, statements, B-tree和pager。它们之间的关系如下：

 ![](082a073dedc6487608485445a3edae34-20220901162209-almzd9d.jpg)​

上图告诉我们在编程需要知道的三个主要方面：API,事务(Transaction)和锁(Locks)。从技术上来说，B-tree和pager不是API的一部分。但是它们却在事务和锁上起着关键作用（稍后将讨论）。

1.3、Connections和Statements  
Connection和statement是执行SQL命令涉及的两个主要数据结构，几乎所有通过API进行的操作都要用到它们。一个连接(Connection)代表在一个独立的事务环境下的一个连接A (connection represents a single connection to a database as well as a single transaction context)。每一个statement都和一个connection关联，它通常表示一个编译过的SQL语句，在内部，它以VDBE字节码表示。Statement包括执行一个命令所需要一切，包括保存VDBE程序执行状态所需的资源，指向硬盘记录的B-树游标，以及参数等等。

1.4、B-tree和pager  
一个connection可以有多个database对象---一个主要的数据库以及附加的数据库，每一个数据库对象有一个B-tree对象，一个B-tree有一个pager对象(这里的对象不是面向对象的“对象”，只是为了说清楚问题)。  
Statement最终都是通过connection的B-tree和pager从数据库读或者写数据，通过B-tree的游标(cursor)遍历存储在页面(page)中的记录。游标在访问页面之前要把数所从disk加载到内存，而这就是pager的任务。任何时候，如果B-tree需要页面，它都会请求pager从disk读取数据，然后把页面(page)加载到页面缓冲区(page cache)，之后，B-tree和与之关联的游标就可以访问位于page中的记录了。  
如果cursor改变了page，为了防止事务回滚，pager必须采取特殊的方式保存原来的page。总的来说，pager负责读写数据库，管理内存缓存和页面（page），以及管理事务，锁和崩溃恢复(这些在事务一节会详细介绍)。  
总之，关于connection和transaction，你必须知道两件事：  
(1) 对数据库的任何操作，一个连接存在于一个事务下。  
(2) 一个连接决不会同时存在多个事务下。  
whenever a connection does anything with a database, it always operates under exactly one  
transaction, no more, no less.

1.5、核心API

核心API 主要与执行SQL命令有关，本质上有两种方法执行SQL语句：prepared query 和wrapped query。Prepared query由三个阶段构成：preparation，execution和finalization。其实wrapped query只是对prepared query的三个过程包装而已，最终也会转化为prepared query的执行。

1.5.1、连接的生命周期(The Connection Lifecycle)  
和大多数据库连接相同，由三个过程构成：  
（1） 连接数据库(Connect to the database)：  
每一个SQLite数据库都存储在单独的操作系统文件中，连接，打开数据库的C API为：sqlite3_open()，它的实现位于main.c文件中，如下：  
int sqlite3_open(const char *zFilename, sqlite3 **ppDb)  
{  
  return openDatabase(zFilename, ppDb, SQLITE_OPEN_READWRITE | SQLITE_OPEN_CREATE, 0);  
}  
当连接一个在磁盘上的数据库，如果数据库文件存在，SQLite打开一个文件；如果不存在，SQLite会假定你想创建一个新的数据库。在这种情况下，SQLite不会立即在磁盘上创建一个文件，只有当你向数据库写入数据时才会创建文件，比如：创建表、视图或者其它数据库对象。如果你打开一个数据，不做任何事，然后关闭它，SQLite会创建一个文件，只是一个空文件而已。  
另外一个不立即创建一个新文件的原因是，一些数据库的参数，比如：编码，页面大小等，只在在数据库创建前设置。默认情况下，页面大小为1024字节，但是你可以选择512-32768字节之间为 2幂数的数字。有些时候，较大的页面能更有效的处理大量的数据。  
（2） 执行事务(Perform transactions)：  
all commands are executed within transactions。默认情况下，事务自动提交，也就是每一个SQL语句都在一个独立的事务下运行。当然也可以通过使用BEGIN..COMMIT手动提交事务。  
（3） 断开连接(Disconnect from the database)：  
主要是关闭数据库的文件。

1.5.2、执行Prepared Query  
前面提到，预处理查询(Prepared Query)是SQLite执行所有SQL命令的方式，包括以下三个过程：  
(1) Prepared Query：  
分析器（parser），分词器(tokenizer)和代码生成器(code generator)把SQL Statement编译成VDBE字节码，编译器会创建一个statement句柄(sqlite3_stmt)，它包括字节码以及其它执行命令和遍历结果集的所有资源。  
 相应的C API为sqlite3_prepare()，位于prepare.c文件中，如下：  
int sqlite3_prepare(  
  sqlite3 *db,              /* Database handle. */  
  const char *zSql,         /* UTF-8 encoded SQL statement. */
  int nBytes,               /* Length of zSql in bytes. */  
  sqlite3_stmt **ppStmt,    /* OUT: A pointer to the prepared statement */  
  const char **pzTail       /* OUT: End of parsed string */  
){  
  int rc;  
  rc = sqlite3LockAndPrepare(db,zSql,nBytes,0,ppStmt,pzTail);  
  assert( rc==SQLITE_OK || ppStmt==0 || *ppStmt==0 );  /* VERIFY: F13021 */  
  return rc;  
}  
(2) Execution：  
虚拟机执行字节码，执行过程是一个步进(stepwise)的过程，每一步(step)由sqlite3_step()启动，并由VDBE执行一段字节码。由sqlite3_prepare编译字节代码，并由sqlite3_step()启动虚拟机执行。在遍历结果集的过程中，它返回SQLITE_ROW，当到达结果末尾时，返回SQLITE_DONE。  
(3) Finalization：  
VDBE关闭statement，释放资源。相应的C API为sqlite3_finalize()。

通过下图可以更容易理解该过程：

 ![](1ce24407db134eea1c7bd5f3162235ea-20220901162209-z4szx49.jpg)​

最后以一个具体的例子结束本节，下节讨论事务。

#include <stdio.h>  
#include <stdlib.h>  
#include "sqlite3.h"

#include <string.h>

int main(int argc, char **argv)  
{  
    int rc, i, ncols;  
    sqlite3 *db;  
    sqlite3_stmt *stmt;  
    char *sql;  
    const char *tail;  
    //打开数据  
    rc = sqlite3_open("foods.db", &db);

    if(rc) {  
        fprintf(stderr, "Can't open database: %s\n", sqlite3_errmsg(db));  
        sqlite3_close(db);  
        exit(1);  
    }

    sql = "select * from episodes";  
    //预处理  
    rc = sqlite3_prepare(db, sql, (int)strlen(sql), &stmt, &tail);

    if(rc != SQLITE_OK) {  
        fprintf(stderr, "SQL error: %s\n", sqlite3_errmsg(db));  
    }

    rc = sqlite3_step(stmt);  
    ncols = sqlite3_column_count(stmt);

    while(rc == SQLITE_ROW) {

        for(i=0; i < ncols; i++) {  
            fprintf(stderr, "'%s' ", sqlite3_column_text(stmt, i));  
        }

        fprintf(stderr, "\n");

        rc = sqlite3_step(stmt);  
    }  
    //释放statement  
    sqlite3_finalize(stmt);  
    //关闭数据库  
    sqlite3_close(db);

    return 0;<br />}

 [/Files/hustcat/2008/Learn.rar](http://files.cnblogs.com/hustcat/2008/Learn.rar)

主要参考:The Definitive Guide to SQLite

写在前面:本节讨论事务,事务是DBMS最核心的技术之一.在计算机科学史上,有三位科学家因在数据库领域的成就而获ACM图灵奖,而其中之一Jim Gray(曾任职微软)就是因为在事务处理方面的成就而获得这一殊荣,正是因为他,才使得OLTP系统在随后直到今天大行其道.关于事务处理技术,涉及到很多,随便就能写一本书.在这里我只讨论SQLite事务实现的一些原理,SQLite的事务实现与大型通用的DBMS相比,其实现比较简单.这些内容可能比较偏于理论,但却不难,也是理解其它内容的基础.好了,下面开始第二节---事务.

2、    事务(Transaction)

2.1、事务的周期(Transaction Lifecycles)  
程序与事务之间有两件事值得注意：  
（1）    哪些对象在事务下运行——这直接与API有关。  
（2）    事务的生命周期，即什么时候开始，什么时候结束以及它在什么时候开始影响别的连接（这点对于并发性很重要）——这涉及到SQLite的具体实现。  
一个连接（connection）可以包含多个(statement)，而且每个连接有一个与数据库关联的B-tree和一个pager。Pager在连接中起着很重要的作用，因为它管理事务、锁、内存缓存以及负责崩溃恢复(crash recovery)。当你进行数据库写操作时，记住最重要的一件事：在任何时候，只在一个事务下执行一个连接。这些回答了第一个问题。  
一般来说，一个事务的生命和statement差不多，你也可以手动结束它。默认情况下，事务自动提交，当然你也可以通过BEGIN..COMMIT手动提交。接下来就是锁的问题。

2.2、锁的状态(Lock States)  
锁对于实现并发访问很重要，而对于大型通用的DBMS，锁的实现也十分复杂，而SQLite相对较简单。通常情况下，它的持续时间和事务一致。一个事务开始，它会先加锁，事务结束，释放锁。但是系统在事务没有结束的情况下崩溃，那么下一个访问数据库的连接会处理这种情况。  
在SQLite中有5种不同状态的锁，连接（connection）任何时候都处于其中的一个状态。下图显示了相应的状态以及锁的生命周期。  
​![](0d091ea97c6942ef445e3b5b6d657016-20220901162310-h3ln43e.jpg)  
 关于这个图有以下几点值得注意：  
（1）    一个事务可以在UNLOCKED，RESERVED或EXCLUSIVE三种状态下开始。默认情况下在UNLOCKED时开始。  
（2）    白色框中的UNLOCKED, PENDING, SHARED和 RESERVED可以在一个数据库的同一时存在。  
（3）    从灰色的PENDING开始，事情就变得严格起来，意味着事务想得到排斥锁(EXCLUSIVE)（注意与白色框中的区别）。  
虽然锁有这么多状态，但是从体质上来说，只有两种情况：读事务和写事务。

2.3、读事务(Read Transactions)  
我们先来看看SELECT语句执行时锁的状态变化过程，非常简单：一个连接执行select语句，触发一个事务，从UNLOCKED到SHARED，当事务COMMIT时，又回到UNLOCKED，就这么简单。  
考虑下面的例子(为了简单，这里用了伪码)：  
**db = open('foods.db')
db.exec('BEGIN')
db.exec('SELECT * FROM episodes')
db.exec('SELECT * FROM episodes')
db.exec('COMMIT')
db.close()**

由于显式的使用了BEGIN和COMMIT，两个SELECT命令在一个事务下执行。第一个exec()执行时，connection处于SHARED，然后第二个exec()执行，当事务提交时，connection又从SHARED回到UNLOCKED状态，如下：  
UNLOCKED→PENDING→SHARED→UNLOCKED  
如果没有BEGIN和COMMIT两行时如下：  
UNLOCKED→PENDING→SHARED→UNLOCKED→PENDING→ SHARED→UNLOCKED

2.4、写事务（Write Transactions）  
下面我们来考虑写数据库，比如UPDATE。和读事务一样，它也会经历UNLOCKED→PENDING→SHARED，但接下来却是灰色的PENDING，

2.4.1、The Reserved States  
当一个连接（connection）向数据库写数据时，从SHARED状态变为RESERVED状态，如果它得到RESERVED锁，也就意味着它已经准备好进行写操作了。即使它没有把修改写入数据库，也可以把修改保存到位于pager中缓存中（page cache）。  
当一个连接进入RESERVED状态，pager就开始初始化恢复日志（rollback journal）。在RESERVED状态下，pager管理着三种页面：  
(1)    Modified pages：包含被B-树修改的记录，位于page cache中。  
(2)    Unmodified pages：包含没有被B-tree修改的记录。  
(3)    Journal pages：这是修改页面以前的版本，这些并不存储在page cache中，而是在B-tree修改页面之前写入日志。  
Page cache非常重要，正是因为它的存在，一个处于RESERVED状态的连接可以真正的开始工作，而不会干扰其它的（读）连接。所以，SQLite可以高效的处理在同一时刻的多个读连接和一个写连接。

2.4.2 、The Pending States  
当一个连接完成修改，就真正开始提交事务，执行该过程的pager进入EXCLUSIVE状态。从RESERVED状态，pager试着获取PENDING锁，一旦得到，就独占它，不允许任何其它连接获得PENDING锁（PENDING is a gateway lock）。既然写操作持有PENDING锁，其它任何连接都不能从UNLOCKED状态进入SHARED状态，即没有任何连接可以进入数据（no new readers, no new writers）。只有那些已经处于SHARED状态的连接可以继续工作。而处于PENDING状态的Writer会一直等到所有这些连接释放它们的锁，然后对数据库加EXCUSIVE锁，进入EXCLUSIVE状态，独占数据库(讨论到这里，对SQLite的加锁机制应该比较清晰了)。

2.4.3、The Exclusive State  
在EXCLUSIVE状态下，主要的工作是把修改的页面从page cache写入数据库文件，这是真正进行写操作的地方。  
在pager写入modified pages之前，它还得先做一件事：写日志。它检查是否所有的日志都写入了磁盘，而这些通常位于操作的缓冲区中，所以pager得告诉OS把所有的文件写入磁盘，这是由程序synchronous(通过调用OS的相应的API实现)完成的。  
日志是数据库进行恢复的惟一方法，所以日志对于DBMS非常重要。如果日志页面没有完全写入磁盘而发生崩溃，数据库就不能恢复到它原来的状态，此时数据库就处于不一致状态。日志写入完成后，pager就把所有的modified pages写入数据库文件。接下来就取决于事务提交的模式，如果是自动提交，那么pager清理日志，page cache，然后由EXCLUSIVE进入UNLOCKED。如果是手动提交，那么pager继续持有EXCLUSIVE锁和保存日志，直到COMMIT或者ROLLBACK。

总之，从性能方面来说，进程占有排斥锁的时间应该尽可能的短，所以DBMS通常都是在真正写文件时才会占有排斥锁，这样能大大提高并发性能。

‍

# [SQLite入门与分析(三)---内核概述(1)](http://www.cnblogs.com/hustcat/archive/2009/02/15/1390989.html)

写在前面:从本章开始,我们开始进入SQLite的内核。为了能更好的理解SQLite，我先从总的结构上讨论一下内核，从全局把握SQLite很重要。SQLite的内核实现不是很难，但是也不是很简单。总的来说分为三个部分，本章主要讨论虚拟机(Virtual Machine)，但是这里只是从原理上概述，不会太多的涉及实际代码。但是概述完内核之后会仔细讨论源代码的。好了，下面我们来讨论虚拟机(VM)。

1、虚拟机（Virtual Machine）  
VDBE是SQLite的核心，它的上层模块和下层模块都是本质上都是为它服务的。它的实现位于vbde.c, vdbe.h, vdbeapi.c, vdbeInt.h, 和vdbemem.c几个文件中。它通过底层的基础设施B+Tree执行由编译器（Compiler）生成的字节代码，这种字节代码程序语言(bytecode programming lauguage)是为了进行查询，读取和修改数据库而专门设计的。  
字节代码在内存中被封装成sqlite3_stmt对象(内部叫做Vdbe，见vdbeInt.h)，Vdbe（或者说statement）包含执行程序所需要的一切：  
**a)    a bytecode program
b)    names and data types for all result columns
c)    values bound to input parameters
d)    a program counter
e)    an execution stack of operands
f)    an arbitrary amount of &quot;numbered&quot; memory cells
g)    other run-time state information (such as open BTree objects, sorters, lists, sets)**

字节代码和汇编程序十分类似，每一条指令由操作码和三个操作数构成：<opcode, P1, P2, P3>。Opcode为一定功能的操作码，为了理解，可以看成一个函数。P1是32位的有符号整数，p2是31位的无符号整数，它通常是导致跳转(jump)的指令的目标地址（destination），当然这了有其它用途；p3为一个以null结尾的字符串或者其它结构体的指针。和C API不同的是，VDBE操作码经常变化，所以不应该用字节码写程序。  
下面的几个C API直接和VDBE交互：  
• sqlite3_bind_xxx() functions  
• sqlite3_step()  
• sqlite3_reset()  
• sqlite3_column_xxx() functions  
• sqlite3_finalize()

为了有个感性，下面看一个具体的字节码程序：  
sqlite> .m col  
sqlite> .h on  
sqlite> .w 4 15 3 3 15  
sqlite> explain select * from episodes;  
addr  opcode           p1   p2   p3

---

0     Goto                0    12  
1     Integer             0    0  
2     OpenRead         0    2    # episodes  
3     SetNumColumns  0    3  
4     Rewind             0    10  
5     Recno               0    0  
6     Column            0    1  
7     Column            0    2  
8     Callback           3    0  
9     Next                0    5  
10    Close               0    0  
11    Halt                 0    0  
12    Transaction       0    0  
13    VerifyCookie      0    10  
14    Goto               0    1  
15    Noop               0    0

1.1、    栈(Stack)  
一个VDBE程序通常由不同完成特定任务的段（section）构成，每一个段中，都有一些操作栈的指令。这是由于不同的指令有不同个数的参数，一些指令只有一个参数；一些指令没有参数；一些指令有好几个参数，这种情况下，三个操作数就不能满足。  
考虑到这些情况，指令采用栈来传递参数。(注：从汇编的角度来看，传递参数的方式有好几种，比如：寄存器，全局变量，而堆栈是现代语言常用的方式，它具有很大的灵活性)。而这些指令不会自己做这些事情，所以在它们之前，需要其它一些指令的帮助。VDBE把计算的中间结果保存到内存单元(memory cells)中，其实，堆栈和内存单元都是基于Mem（见vdbeInt.h）数据结构(注：这里的栈，内存单元都是虚拟的，记得一位计算机科学家说过：计算机科学中90%以上的科学都是虚拟化问题。一点不假，OS本质上也是虚拟机，而在这里SQLite，我们也处处可见虚拟化的身影，到后面的OS Interface模块中再仔细讨论这个问题)。

1.2、程序体(Program Body)  
这是一个打开episodes表的过程。  
第一条指令：Integer是为第二条指令作准备的，也就是把第二条指令执行需要的参数压入堆栈，OpenRead从堆栈中取出参数值然后执行。SQLite可以通过ATTACH命令在一个连接中打开多个数据库文件，每当SQLite打开一个数据，它就为之赋一个索引号(index)，main database的索引为0，第一个数据库为1，依次如此。Integer指令数据库索引的值压入栈，而OpenRead从中取出值，并决定打开哪个数据，来看看SQLite文档中的解释：  
     Open a read-only cursor for the database table whose root page is P2 in a database file.  
The database file is determined by an integer from the top of the stack. 0 means the main database and 1 means the database used for temporary tables. Give the new cursor an identifier of P1. The P1 values need not be contiguous but all P1 values should be small integers. It is an error for P1 to be negative.  
     If P2==0 then take the root page number from off of the stack.  
     There will be a read lock on the database whenever there is an open cursor. If the data-  
base was unlocked prior to this instruction then a read lock is acquired as part of this instruction. A read lock allows other processes to read the database but prohibits any other process from modifying the database. The read lock is released when all cursors are closed. If this instruction attempts to get a read lock but fails, the script terminates with an SQLITE_BUSY error code.  
     The P3 value is a pointer to a KeyInfo structure that defines the content and collating

sequence of indices. P3 is NULL for cursors that are not pointing to indices.

再来看看SetNumColumns指令设置游标将指向的列。P1为游标的索引（这里为0，刚刚打开），P2为列的数目，episodes表有三列。  
继续Rewind指令，它将游标重新设置到表的开始，它会检查表是否为空（即没有记录），如果没有记录，它会导致指令指针跳到P2指定的指令处。在这里，P2为10，即Close指令。一旦Rewind设置游标，接下就执行5-9这几条指令，它们的主要功能是遍历结果集，Recno把由游标P1指定的记录的关键字压入堆栈。Column指令从由P1指定的游标，P2指定的列取值。5,6,7三条指令分别把id(primary key),season和name字段的值压入栈。接下来，Callback指令从栈中取出三个值（P1），然后形成一个记录数组，存储在内存单元中(memory cell)。Callback会停止VDBE的操作，把控制权交给sqlite3_stemp()，该函数返回SQLITE_ROW。  
​![](54d55ead18c02bbdd0cdb23ddc4fb015-20220901162323-3rkw619.jpg)  
一旦VDBE创建了记录结构，我们就可以通过sqlite3_column_xxx() functions从记录结构的域内取出值。当下次调用sqlite3_step()时，指令指针会指向Next指令，而Next指令会把游标向移向下一行，如果有其它的记录，它会跳到由P2指定的指令，在这里为指令5，创建一个新的记录结构，一直循环，直到结果集的最后。Close指令会关闭游标，然后执行Halt指令，结束VDBE程序。

1.3、程序开始与停止  
现在来看看其余的指令，Goto指令是一条跳转指令，跳到P2处，即第12条指令。指令12是Transaction，它开始一个新的事务；然后执行VerifyCookie，它的主要功能VDBE程序编译后，数据库模式是否改变（即是否进行过更新操作）。这在SQLite中是一个很重要的概念，在SQL被sqlite3_prepare()编译成VDBE代码至程序调用sqlite3_step()执行字节码的这段时间，另一个SQL命令可能会改变数据库模式（such as ALTER TABLE, DROP TABLE, or CREATE TABLE）。一旦发生这种情况，之前编译的statement就会变得无效，数据库模式信息记录在数据库文件的根页面中。类似，每一个statement都有一份用来比较的在编译时刻该模式的备份，VerifyCookie的功能就是检查它们是否匹配，如果不匹配，将采取相关操作。  
​![](db800c791bb3a8eb3b2d84ab051eeec8-20220901162323-fzxb58g.jpg)​

如果两者匹配，会执行下一条指令Goto；它会跳到程序的主要部分，即第一条指令，打开表读取记录。这里有两点值得注意：  
(1)Transaction指令自己不会获取锁（ The Transaction instruction doesn’t acquire any locks in itself）。它的功能相当于BEGIN，而实际是由OpenRead指令获取share lock的。当事务关闭时释放锁，这取决于Halt指令，它会进行扫尾工作。  
(2)statement对象（VDBE程序）所需的存储空间在程序执行前就已经确定。这有原于两个重要事实：首先，栈的深度不会比指令的数目还多（通常少得多）。其次，在执行VDBE程序之前，SQLite可以计算出为分配资源所需要的内存。

1.4指令的类型(Instruction Types)  
每条指令都完成特定的任务，而且通常和别的指令有关。大体上来说，指令可分为三类：  
（1)Value manipulation：这些指令通常完成算术运算，比如：add, subtract, divide；逻辑运算，比如：AND和OR；还有字符串操作。  
（2)Data management：这些指令操作在内存和磁盘上的数据。内存指令进行栈操作或者在内存单元之间传递数据。磁盘操作指令控制B-tree和pager打开或操作游标，开始或结束事务，等等。  
（3)Control flow：控制指令主要是移动指令指针。

1.5、程序的执行(Program execution)  
最后我们来看VM解释器是如何实现以及字节代码大致是如何执行的。在vdbe.c文件中有一个很关键的函数：  
//执行VDBE程序  
int sqlite3VdbeExec(  
  Vdbe *p                    /* The VDBE */  
)  
该函数是执行VDBE程序的入口。来看看它的内部实现：

/*从这里开始执行指令  
**pc为程序计数器(int)  
*/
for(pc=p-&gt;pc; rc==SQLITE_OK; pc++){
  //取得操作码
  pOp = &amp;p-&gt;aOp[pc];
  switch( pOp-&gt;opcode ){
  case OP_Goto: {             /* jump */  
      CHECK_FOR_INTERRUPT;  
      pc = pOp->p2 - 1;  
      break;  
     }  
    … …  
   }  
}  
从这段代码，我们大致可以推出VM执行的原理：VM解释器实际上是一个包含大量switch语句的for循环，每一个switch语句实现一个特定的操作指令。

‍

# [SQLite入门与分析(三)---内核概述(2)](http://www.cnblogs.com/hustcat/archive/2009/02/17/1392746.html)

写在前面:本节是前一节内容的后续部分，这两节都是从全局的角度SQLite内核各个模块的设计和功能。只有从全局上把握SQLite，才会更容易的理解SQLite的实现。SQLite采用了层次化，模块化的设计，而这些使得它的可扩展性和可移植性非常强。而且SQLite的架构与通用DBMS的结构差别不是很大，所以它对于理解通用DBMS具有重要意义。好了，下面我们开始讨论SQLite剩余的两部分：Back-end(后端)和compiler(编译器)。

2、B-tree和Pager  
B-Tree使得VDBE可以在O(logN)下查询，插入和删除数据，以及O(1)下双向遍历结果集。B-Tree不会直接读写磁盘，它仅仅维护着页面(pages)之间的关系。当B-TREE需要页面或者修改页面时，它就会调用Pager。当修改页面时，pager保证原始页面首先写入日志文件，当它完成写操作时，pager根据事务状态决定如何做。B-tree不直接读写文件，而是通过page cache这个缓冲模块读写文件对于性能是有重要意义的（注：这和操作系统读写文件类似，在Linux中,操作系统的上层模块并不直接调用设备驱动读写设备，而是通过一个高速缓冲模块调用设备驱动读写文件,并将结果存到高速缓冲区）。

2.1、数据库文件格式（Database File Format）  
数据库中所有的页面都按从1开始顺序标记。一个数据库由许多B-tree构成——每一个表和索引都有一个B-tree（注：索引采用B-tree，而表采用B+tree，这主要是表和索引的需求不同以及B-tree和B+tree的结构不同决定的：B+tree的所有叶子节点包含了全部关键字信息，而且可以有两种顺序查找——具体参见《数据结构》，严蔚敏。而B-tree更适合用来作索引）。所有表和索引的根页面都存储在sqlite_master表中。  
数据库中第一个页面（page 1）有点特殊，page 1的前100个字节包含一个描述数据库文件的特殊的文件头。它包括库的版本，模式的版本，页面大小，编码等所有创建数据库时设置的参数。这个特殊的文件头的内容在btree.c中定义，page 1也是sqlite_master表的根页面。

2.1、页面重用及回收(Page Reuse and Vacuum )  
SQLite利用一个空闲列表(free list)进行页面回收。当一个页面的所有记录都被删除时，就被插入到该列表。当运行VACUUM命令时，会清除free list，所以数据库会缩小，本质上它是在新的文件重新建立数据库，而所有使用的页在都被拷贝过去，而free list却不会，结果就是一个新的，变小的数据库。当数据库的autovacuum开启时，SQLite不会使用free list，而且在每一次commit时自动压缩数据库。

2.2、B-Tree记录  
B-tree中页面由B-tree记录组成，也叫做payloads。每一个B-tree记录，或者payload有两个域：关键字域(key field)和数据域(data field)。Key field就是ROWID的值，或者数据库中表的关键字的值。从B-tree的角度，data field可以是任何无结构的数据。数据库的记录就保存在这些data fields中。B-tree的任务就是排序和遍历，它最需要就是关键字。Payloads的大小是不定的，这与内部的关键字和数据域有关，当一个payload太大不能存在一个页面内进便保存到多个页面。

B+Tree按关键字排序，所有的关键字必须唯一。表采用B+tree，内部页面不包含数据，如下：  
​![](6d195be3cdeb336507f6579c1d733f54-20220901162356-73ex3ds.jpg)​

 B+tree中根页面(root page)和内部页面(internal pages)都是用来导航的，这些页面的数据域都是指向下级页面的指针，仅仅包含关键字。所有的数据库记录都存储在叶子页面(leaf pages)内。在叶节点一级，记录和页面都是按照关键字的顺序的，所以B-tree可以水平方向遍历，时间复杂度为O(1)。

2.3、记录和域（Records and Fields）  
位于叶节点页面的数据域的记录由VDBE管理，数据库记录以二进制的形式存储，但有一定的数据格式。记录格式包括一个逻辑头（logical header）和一个数据区(data segment)，header segment包括header的大小和一个数据类型数组，数据类型用来在data segment的数据的类型，如下：

 ![](eb08741d03924114b054536ad6b24b21-20220901162356-zk891bp.jpg)​

2.4、层次数据组织(Hierarchical Data Organization)

![](44192b7894f5113b0fbbaae3790c43d3-20220901162356-ke65wag.jpg)​

从上往下，数据越来越无序，从下向上，数据越来越结构化.

2.5、B-Tree API  
B-Tree模块有它自己的API，它可以独立于C API使用。另一个特点就是它支持事务。由pager处理的事务，锁和日志都是为B-tree服务的。根据功能可以分为以下几类：  
2.5.1、访问和事务函数  
 **sqlite3BtreeOpen** : Opens a new database file. Returns a B-tree object.  
 **sqlite3BtreeClose** : Closes a database.  
 **sqlite3BtreeBeginTrans** : Starts a new transaction.  
 **sqlite3BtreeCommit** : Commits the current transaction.  
 **sqlite3BtreeRollback** : Rolls back the current transaction.  
 **sqlite3BtreeBeginStmt** : Starts a statement transaction.  
 **sqlite3BtreeCommitStmt** : Commits a statement transaction.  
 **sqlite3BtreeRollbackStmt** : Rolls back a statement transaction.

2.5.2、表函数  
 **sqlite3BtreeCreateTable** : Creates a new, empty B-tree in a database file.  
 **sqlite3BtreeDropTable** : Destroys a B-tree in a database file.  
 **sqlite3BtreeClearTable** : Removes all data from a B-tree, but keeps the B-tree intact.  
2.5.3、游标函数(Cursor Functions)  
 **sqlite3BtreeCursor** : Creates a new cursor pointing to a particular B-tree.  
 **sqlite3BtreeCloseCursor** : Closes the B-tree cursor.  
 **sqlite3BtreeFirst** : Moves the cursor to the first element in a B-tree.  
 **sqlite3BtreeLast** : Moves the cursor to the last element in a B-tree.  
 **sqlite3BtreeNext** : Moves the cursor to the next element after the one it is currently  
       pointing to.  
 **sqlite3BtreePrevious** : Moves the cursor to the previous element before the one it is  
      currently pointing to.

sqlite3BtreeMoveto: Moves the cursor to an element that matches the key value passed  in as a parameter.

2.5.4、记录函数(Record Functions)  
 **sqlite3BtreeDelete** : Deletes the record that the cursor is pointing to.  
 **sqlite3BtreeInsert** : Inserts a new element in the appropriate place of the B-tree.  
 **sqlite3BtreeKeySize** : Returns the number of bytes in the key of the record that the  
              cursor is pointing to.  
 **sqlite3BtreeKey** : Returns the key of the record the cursor is currently pointing to.  
 **sqlite3BtreeDataSize** : Returns the number of bytes in the data record that the cursor is  
              currently pointing to.  
 **sqlite3BtreeData** : Returns the data in the record the cursor is currently pointing to.

2.5.5、配置函数(Configuration Functions)  
sqlite3BtreeSetCacheSize: Controls the page cache size as well as the synchronous  
            writes (as defined in the synchronous pragma).  
sqlite3BtreeSetSafetyLevel: Changes the way data is synced to disk in order to increase  
           or decrease how well the database resists damage due to OS crashes and power     failures.  
           Level 1 is the same as asynchronous (no syncs() occur and there is a high probability of  
           damage). This is the equivalent to pragma synchronous=OFF. Level 2 is the default. There  
           is a very low but non-zero probability of damage. This is the equivalent to pragma  
           synchronous=NORMAL. Level 3 reduces the probability of damage to near zero but with a  
           write performance reduction. This is the equivalent to pragma synchronous=FULL.  
sqlite3BtreeSetPageSize: Sets the database page size.  
sqlite3BtreeGetPageSize: Returns the database page size.  
sqlite3BtreeSetAutoVacuum: Sets the autovacuum property of the database.  
sqlite3BtreeGetAutoVacuum: Returns whether the database uses autovacuum.  
sqlite3BtreeSetBusyHandler: Sets the busy handler  
2.6、实例分析  
最后以sqlite3_open的具体实现结束本节的讨论(参见Version 3.6.10的源码)：

![](0cd3799381cbf2f3f9766cb931a2a001-20220901162356-4lqu9re.jpg)​

由上图可以知道，SQLite的所有IO操作，最终都转化为操作系统的系统调用(一名话：DBMS建立在痛苦的OS之上)。同时也可以看到SQLite的实现非常的层次化，模块化，使得SQLite更易扩展，可移植性非常强。

3、编译器（Compiler）  
3.1、分词器(Tokenizer)  
接口把要执行的SQL语句传递给Tokenizer,Tokenizer按照SQL的词法定义把它切分一个一个的词，并传递给分析器(Parser)进行语法分析。分词器是手工写的，主要在Tokenizer.c中实现。  
3.2、分析器(Parser)  
SQLite的语法分析器是用Lemon——一个开源的LALR(1)语法分析器的生成器，生成的文件为parser.c。  
一个简单的语法树：  
 SELECT rowid, name, season FROM episodes WHERE rowid=1 LIMIT 1  
​![](b667d8cc611ede5a74728acb377000cf-20220901162356-aqdrcuz.jpg)​

 3.3、代码生成器（Code Generator）  
代码生成器是SQLite中取庞大，最复杂的部分。它与Parser关系紧密，根据语法分析树生成VDBE程序执行SQL语句的功能。由诸多文件构成：select.c,update.c,insert.c,delete.c,trigger.c,where.c等文件。这些文件生成相应的VDBE程序指令，比如SELECT语句就由select.c生成。下面是一个读操作中打开表的代码的生成实现：  
/* Generate code that will open a table for reading.  
*/  
void sqlite3OpenTableForReading(  
  Vdbe *v,        /* Generate code into this VDBE */
  int iCur,       /* The cursor number of the table */  
  Table *pTab     /* The table to be opened */  
){  
  sqlite3VdbeAddOp(v, OP_Integer, pTab->iDb, 0);  
  sqlite3VdbeAddOp(v, OP_OpenRead, iCur, pTab->tnum);  
  VdbeComment((v, "# %s", pTab->zName));  
  sqlite3VdbeAddOp(v, OP_SetNumColumns, iCur, pTab->nCol);  
}  
Sqlite3vdbeAddOp函数有三个参数：（1）VDBE实例（它将添加指令），（2）操作码（一条指令），（3）两个操作数。

3.4、查询优化  
代码生成器不仅负责生成代码，也负责进行查询优化。主要的实现位于where.c中，生成的WHERE语句块通常被其它模块共享，比如select.c，update.c以及delete.c。这些模块调用sqlite3WhereBegin()开始WHERE语句块的指令生成，然后加入它们自己的VDBE代码返回，最后调用sqlite3WhereEnd()结束指令生成，如下：

![](583819f608b8358448d519f8653cb7bb-20220901162356-5mxs64t.jpg)​

‍

# [SQLite入门与分析(四)---Page Cache之事务处理(1)](http://www.cnblogs.com/hustcat/archive/2009/02/26/1398558.html)

写在前面：从本章开始，将对SQLite的每个模块进行讨论。讨论的顺序按照我阅读SQLite的顺序来进行，由于项目的需要，以及时间关系，不能给出一个完整的计划，但是我会先讨论我认为比较重要的内容。本节讨论SQLite的事务处理技术，事务处理是DBMS中最关键的技术，对SQLite也一样，它涉及到并发控制，以及故障恢复，由于内容较多，分为两节。好了，下面进入正题。

 本节通过一个具体的例子来分析SQLite原子提交的实现（基于Version 3.3.6的代码）。CREATE TABLE episodes( id integer primary key,name text, cid int) ；插入一条记录：insert into episodes(name,cid) values("cat",1) ;它经过编译器处理后生成的虚拟机代码如下：sqlite> explain insert into episodes(name,cid) values("cat",1);0|Trace|0|0|0|explain insert into episodes(name,cid) values("cat",1);|00|1|Goto|0|12|0||00|2|SetNumColumns|0|3|0||00|3|OpenWrite|0|2|0||00|4|NewRowid|0|2|0||00|5|Null|0|3|0||00|6|String8|0|4|0|cat|00|7|Integer|1|5|0||00|8|MakeRecord|3|3|6|dad|00|9|Insert|0|6|2|episodes|0b|10|Close|0|0|0||00|11|Halt|0|0|0||00|12|Transaction|0|1|0||00|13|VerifyCookie|0|1|0||00|14|Transaction|1|1|0||00|15|VerifyCookie|1|0|0||00|

16|TableLock|0|2|1|episodes|00|

17|Goto|0|2|0||00|

 

1、初始状态（Initial State）当一个数据库连接第一次打开时，状态如图所示。图中最右边（“Disk”标注）表示保存在存储设备中的内容。每个方框代表一个扇区。蓝色的块表示这个扇区保存了原始数据。图中中间区域是操作系统的磁盘缓冲区。开始的时候，这些缓存是还没有被使用，因此这些方框是空白的。图中左边区域显示SQLite用户进程的内存。因为这个数据库连接刚刚打开，所以还没有任何数据记录被读入，所以这些内存也是空的。

 

2、获取读锁(Acquiring A Read Lock)在SQLite写数据库之前，它必须先从数据库中读取相关信息。比如，在插入新的数据时，SQLite会先从sqlite_master表中读取数据库模式(相当于数据字典)，以便编译器对INSERT语句进行分析，确定数据插入的位置。在进行读操作之前，必须先获取数据库的共享锁(shared lock)，共享锁允许两个或更多的连接在同一时刻读取数据库。但是共享锁不允许其它连接对数据库进行写操作。shared lock存在于操作系统磁盘缓存，而不是磁盘本身。文件锁的本质只是操作系统的内核数据结构，当操作系统崩溃或掉电时，这些内核数据也会随之消失。

 

 3、读取数据一旦得到shared lock，就可以进行读操作。如图所示，数据先由OS从磁盘读取到OS缓存，然后再由OS移到用户进程空间。一般来说，数据库文件分为很多页，而一次读操作只读取一小部分页面。如图，从8个页面读取3个页面。

4、获取Reserved Lock在对数据进行修改操作之前，先要获取数据库文件的Reserved Lock，Reserved Lock和shared lock的相似之处在于，它们都允许其它进程对数据库文件进行读操作。Reserved Lock和Shared Lock可以共存，但是只能是一个Reserved Lock和多个Shared Lock——多个Reserved Lock不能共存。所以，在同一时刻，只能进行一个写操作。Reserved Lock意味着当前进程(连接)想修改数据库文件，但是还没开始修改操作，所以其它的进程可以读数据库，但不能写数据库。

5、创建恢复日志(Creating A Rollback Journal File)在对数据库进行写操作之前，SQLite先要创建一个单独的日志文件，然后把要修改的页面的原始数据写入日志。回滚日志包含一个日志头(图中的绿色)——记录数据库文件的原始大小。所以即使数据库文件大小改变了，我们仍知道数据库的原始大小。从OS的角度来看，当一个文件创建时，大多数OS(Windows,Linux,Mac OS X)不会向磁盘写入数据，新创建的文件此时位于磁盘缓存中，之后才会真正写入磁盘。如图，日志文件位于OS磁盘缓存中，而不是位于磁盘。

![](dab7a363607336ac76c17367518e1d16-20220901162416-od6gn8n.gif)​

上面 5步的代码的实现：

Code

 其实现过程如下图所示：

![](bfddd9ae8aa57ca441b81c517a371e43-20220901162416-kz1vzfn.gif)​

主要参考：http://www.sqlite.org/atomiccommit.html

# [SQLite入门与分析(四)---Page Cache之事务处理(2)](http://www.cnblogs.com/hustcat/archive/2009/02/26/1398774.html)

写在前面：个人认为pager层是SQLite实现最为核心的模块，它具有四大功能：I/O，页面缓存，并发控制和日志恢复。而这些功能不仅是上层Btree的基础，而且对系统的性能和健壮性有关至关重要的影响。其中并发控制和日志恢复是事务处理实现的基础。SQLite并发控制的机制非常简单——封锁机制；别外，它的查询优化机制也非常简单——基于索引。这一切使得整个SQLite的实现变得简单，SQLite变得很小，运行速度也非常快，所以，特别适合嵌入式设备。好了，接下来讨论事务的剩余部分。6、修改位于用户进程空间的页面(Changing Database Pages In User Space)页面的原始数据写入日志之后，就可以修改页面了——位于用户进程空间。每个数据库连接都有自己私有的空间，所以页面的变化只对该连接可见，而对其它连接的数据仍然是磁盘缓存中的数据。从这里可以明白一件事：一个进程在修改页面数据的同时，其它进程可以继续进行读操作。图中的红色表示修改的页面。

7、日志文件刷入磁盘(Flushing The Rollback Journal File To Mass Storage)接下来把日志文件的内容刷入磁盘，这对于数据库从意外中恢复来说是至关重要的一步。而且这通常也是一个耗时的操作，因为磁盘I/O速度很慢。这个步骤不只把日志文件刷入磁盘那么简单，它的实现实际上分成两步：首先把日志文件的内容刷入磁盘（即页面数据）；然后把日志文件中页面的数目写入日志文件头，再把header刷入磁盘（这一过程在代码中清晰可见）。

代码如下：

 

Code

8、获取排斥锁（Obtaining An Exclusive Lock）在对数据库文件进行修改之前(注：这里不是内存中的页面),我们必须得到数据库文件的排斥锁(Exclusive Lock)。得到排斥锁的过程可分为两步：首先得到Pending lock；然后Pending lock升级到exclusive lock。Pending lock允许其它已经存在的Shared lock继续读数据库文件，但是不允许产生新的shared lock，这样做目的是为了防止写操作发生饿死情况。一旦所有的shared lock完成操作，则pending lock升级到exclusive lock。

9、修改的页面写入文件(Writing Changes To The Database File)一旦得到exclusive lock，其它的进程就不能进行读操作，此时就可以把修改的页面写回数据库文件，但是通常OS都把结果暂时保存到磁盘缓存中，直到某个时刻才会真正把结果写入磁盘。

以上两步的实现代码：

 

Code

10、修改结果刷入存储设备(Flushing Changes To Mass Storage)为了保证修改结果真正写入磁盘，这一步必不要少。对于数据库存的完整性，这一步也是关键的一步。由于要进行实际的I/O操作，所以和第7步一样，将花费较多的时间。

![](73bbedcc34e7caf702075c5640063a11-20220901162431-b2up3if.gif)​

最后来看看这几步是如何实现的：

其实以上以上几步是在函数sqlite3BtreeSync()---btree.c中调用的(而关于该函数的调用后面再讲)。

代码如下：

Code

下图可以进一步解释该过程：

![](41c3123ca401abd2838469b4b9af8582-20220901162431-0hklehm.gif)​

‍

# [SQLite入门与分析(四)---Page Cache之事务处理(3)](http://www.cnblogs.com/hustcat/archive/2009/02/26/1398826.html)

写在前面：由于内容较多，所以断续没有写完的内容。

11、删除日志文件(Deleting The Rollback Journal)一旦更改写入设备，日志文件将会被删除，这是事务真正提交的时刻。如果在这之前系统发生崩溃，就会进行恢复处理，使得数据库和没发生改变一样；如果在这之后系统发生崩溃，表明所有的更改都已经写入磁盘。SQLite就是根据日志存在情况决定是否对数据库进行恢复处理。

删除文件本质上不是一个原子操作，但是从用户进程的角度来看是一个原子操作，所以一个事务看起来是一个原子操作。在许多系统中，删除文件也是一个高代价的操作。作为优化，SQLite可以配置成把日志文件的长度截为0或者把日志文件头清零。

12、释放锁(Releasing The Lock)作为原子提交的最后一步，释放排斥锁使得其它进程可以开始访问数据库了。下图中，我们指明了当锁被释放的时候用户空间所拥有的信息已经被清空了.对于老版本的SQLite你可这么认为。但最新的SQLite会保存些用户空间的缓存不会被清空—万一下一个事务开始的时候，这些数据刚好可以用上呢。重新利用这些内存要比再次从操作系统磁盘缓存或者硬盘中读取要来得轻松与快捷得多，何乐而不为呢？在再次使用这些数据之前，我们必须先取得一个共享锁，同时我们还不得不去检查一下，保证还没有其他进程在我们拥有共享锁之前对数据库文件进行了修改。数据库文件的第一页中有一个计数器，数据库文件每做一次修改，这个计数器就会增长一下。我们可以通过检查这个计数器就可得知是否有其他进程修改过数据库文件。如果数据库文件已经被修改过了，那么用户内存空间的缓存就不得不清空，并重新读入。大多数情况下，这种情况不大会发生，因此用户空间的内存缓存将是有效的，这对于性能提高来说作用是显著的。

![](b855a99999b59c9b7b34b73e2d61e383-20220901162450-xbdm1oo.gif)​

以上两步是在sqlite3BtreeCommit()---btree.c函数中实现的。

代码如下：

Code

下图可进一步描述该过程：

![](a28373c9d90963af44d4e11965c69411-20220901162450-e8e9kbm.gif)​

最后来看看sqlite3BtreeSync()和sqlite3BtreeCommit()是如何被调用的。

一般来说，事务提交方式为自动提交的话，在虚拟机中的OP_Halt指令实现提交事务，相关代码如下：

‍

# [SQLite入门与分析(五)---Page Cache之并发控制](http://www.cnblogs.com/hustcat/archive/2009/03/01/1400757.html)

写在前面:本节主要谈谈SQLite的锁机制，SQLite是基于锁来实现并发控制的，所以本节的内容实际上是属于事务处理的，但是SQLite的锁机制实现非常的简单而巧妙，所以在这里单独讨论一下。如果真正理解了它，对整个事务的实现也就理解了。而要真正理解SQLite的锁机制，最好方法就是阅读SQLite的源码，所以在阅读本文时，最好能结合源码。SQLite的锁机制很巧妙，尽管在本节中的源码中，我写了很多注释，也是我个人在研究时的一点心得，但是我发现仅仅用言语，似乎不能把问题说清楚，只有通过体会，才能真正理解SQLite的锁机制。好了，下面进入正题。

SQLite的并发控制机制是采用加锁的方式，实现非常简单，但也非常的巧妙，本节将对其进行一个详细的解剖。请仔细阅读下图，它可以帮助更好的理解下面的内容。

 ![](0d091ea97c6942ef445e3b5b6d657016-20220901162532-g8v33zy.jpg)​

1、RESERVED LOCKRESERVED锁意味着进程将要对数据库进行写操作。某一时刻只能有一个RESERVED Lock，但是RESERVED锁和SHARED锁可以共存，而且可以对数据库加新的SHARED锁。为什么要用RESERVED锁？主要是出于并发性的考虑。由于SQLite只有库级排斥锁（EXCLUSIVE LOCK），如果写事务一开始就上EXCLUSIVE锁，然后再进行实际的数据更新，写磁盘操作，这会使得并发性大大降低。而SQLite一旦得到数据库的RESERVED锁，就可以对缓存中的数据进行修改，而与此同时，其它进程可以继续进行读操作。直到真正需要写磁盘时才对数据库加EXCLUSIVE锁。2、PENDING LOCKPENDING LOCK意味着进程已经完成缓存中的数据修改，并想立即将更新写入磁盘。它将等待此时已经存在的读锁事务完成，但是不允许对数据库加新的SHARED LOCK(这与RESERVED LOCK相区别)。为什么要有PENDING LOCK?主要是为了防止出现写饿死的情况。由于写事务先要获取RESERVED LOCK，所以可能一直产生新的SHARED LOCK，使得写事务发生饿死的情况。3、加锁机制的具体实现

SQLite在pager层获取锁的函数如下：

Code

Windows下具体的实现如下：

Code

在几个关键的部位标记数字。

(I)对于一个读事务会的完整经过：语句序列：（1）——>（2）——>（6）相应的状态真正的变化过程为：UNLOCKED→PENDING(1)→PENDING、SHARED(2)→SHARED（6）→UNLOCKED(II)对于一个写事务完整经过：第一阶段：语句序列：（1）——>（2）——>（6）状态变化：UNLOCKED→PENDING(1)→PENDING、SHARED(2)→SHARED(6)。此时事务获得SHARED LOCK。第二个阶段：语句序列：（3）此时事务获得RESERVED LOCK。第三个阶段：事务执行修改操作。第四个阶段：语句序列：（1）——>（4）——>（5）状态变化为：RESERVED→ RESERVED 、PENDING(1)→PENDING（4）→EXCLUSIVE（5）。此时事务获得排斥锁，就可以进行写磁盘操作了。

注：在上面的过程中，由于（1）的执行，使得某些时刻SQLite处于两种状态，但它持续的时间很短，从某种程度上来说可以忽略，但是为了把问题说清楚，在这里描述了这一微妙而巧妙的过程。

4、SQLite的死锁问题SQLite的加锁机制会不会出现死锁?这是一个很有意思的问题，对于任何采取加锁作为并发控制机制的DBMS都得考虑这个问题。有两种方式处理死锁问题：（1）死锁预防(deadlock prevention)（2）死锁检测(deadlock detection)与死锁恢复(deadlock recovery)。SQLite采取了第一种方式，如果一个事务不能获取锁，它会重试有限次（这个重试次数可以由应用程序运行预先设置，默认为1次）——这实际上是基本锁超时的机制。如果还是不能获取锁，SQLite返回SQLITE_BUSY错误给应用程序，应用程序此时应该中断，之后再重试；或者中止当前事务。虽然基于锁超时的机制简单，容易实现，但是它的缺点也是明显的——资源浪费。

5、事务类型(Transaction Types)既然SQLite采取了这种机制，所以应用程序得处理SQLITE_BUSY 错误，先来看一个会产生SQLITE_BUSY错误的例子：

 ![](3886e9b640fb0b5ee948ebe9f4619e4d-20220901162532-5c72gks.jpg)​

所以应用程序应该尽量避免产生死锁，那么应用程序如何做可以避免死锁的产生呢？答案就是为你的程序选择正确合适的事务类型。SQLite有三种不同的事务类型，这不同于锁的状态。事务可以从DEFERRED，IMMEDIATE或者EXCLUSIVE，一个事务的类型在BEGIN命令中指定：BEGIN [ DEFERRED | IMMEDIATE | EXCLUSIVE ] TRANSACTION；一个deferred事务不获取任何锁，直到它需要锁的时候，而且BEGIN语句本身也不会做什么事情——它开始于UNLOCK状态；默认情况下是这样的。如果仅仅用BEGIN开始一个事务，那么事务就是DEFERRED的，同时它不会获取任何锁，当对数据库进行第一次读操作时，它会获取SHARED LOCK；同样，当进行第一次写操作时，它会获取RESERVED LOCK。由BEGIN开始的Immediate事务会试着获取RESERVED LOCK。如果成功，BEGIN IMMEDIATE保证没有别的连接可以写数据库。但是，别的连接可以对数据库进行读操作，但是RESERVED LOCK会阻止其它的连接BEGIN IMMEDIATE或者BEGIN EXCLUSIVE命令，SQLite会返回SQLITE_BUSY错误。这时你就可以对数据库进行修改操作，但是你不能提交，当你COMMIT时，会返回SQLITE_BUSY错误，这意味着还有其它的读事务没有完成，得等它们执行完后才能提交事务。Exclusive事务会试着获取对数据库的EXCLUSIVE锁。这与IMMEDIATE类似，但是一旦成功，EXCLUSIVE事务保证没有其它的连接，所以就可对数据库进行读写操作了。上面那个例子的问题在于两个连接最终都想写数据库，但是他们都没有放弃各自原来的锁，最终，shared 锁导致了问题的出现。如果两个连接都以BEGIN IMMEDIATE开始事务，那么死锁就不会发生。在这种情况下，在同一时刻只能有一个连接进入BEGIN IMMEDIATE，其它的连接就得等待。BEGIN IMMEDIATE和BEGIN EXCLUSIVE通常被写事务使用。就像同步机制一样，它防止了死锁的产生。基本的准则是：如果你在使用的数据库没有其它的连接，用BEGIN就足够了。但是，如果你使用的数据库在其它的连接也要对数据库进行写操作，就得使用BEGIN IMMEDIATE或BEGIN EXCLUSIVE开始你的事务。

‍

# [SQLite入门与分析(六)---再谈SQLite的锁](http://www.cnblogs.com/hustcat/archive/2009/03/10/1408208.html)

写在前面：SQLite封锁机制的实现需要底层文件系统的支持，不管是Linux，还是Windows，都提供了文件锁的机制，而这为SQLite提供了强大的支持。本节就来谈谈SQLite使用到的文件锁——主要基于Linux和Windows平台。

* Linux的文件锁

Linux 支持的文件锁技术主要包括建议锁（advisory lock）和强制锁（mandatory lock）这两种。此外，Linux 中还引入了两种强制锁的变种形式：共享模式强制锁（share-mode mandatory lock）和租借锁（lease）。在这里，主要讨论建议锁（advisory lock）。  
建议锁并不由内核强制实行，也就是说如果有进程不遵守“游戏规则”，不检查目标文件是否已经由别的进程加了锁就往其中写入数据，那么内核是不会加以阻拦的。因此，建议锁并不能阻止进程对文件的访问，而只能依靠各个进程在访问文件之前检查该文件是否已经被其他进程加锁来实现并发控制。进程需要事先对锁的状态做一个约定，并根据锁的当前状态和相互关系来确定其他进程是否能对文件执行指定的操作。而强制锁是由内核强制采用的文件锁——由于内核对每个read()和write()操作都会检查相应的锁，所以会降低系统性能。  
对于建议锁，Linux提供两种实现方式：锁文件（lock files）和记录锁（ record locking）。

(1)锁文件(lock files)  
锁文件是最简单的对文件加锁的方法，每个需要加锁的数据文件都有一个锁文件（lock file）。当锁文件存在时，就认为该数据文件已经被加锁，别的进程不应该访问（但是你非要访问，Linux也不会阻止）。当锁不存在，进程就可以创建一个锁文件，然后访问相应的数据文件。只要创建锁的过程是原子的，就能保证某一时刻只有一个进程拥有该锁，这种方法保证某一时刻只有一个进程访问文件。  
这种想法很简单，当一个进程想访问文件时，可以按如下方式对文件加锁：

[](# "复制代码")

fd = open("somefile.lck", O_RDONLY, 0644);  
if (fd >= 0) {  
    close(fd);  
    printf("the file is already locked");  
    return 1;  
} else {  
    /* the lock file does not exist, we can lock it and access it */
    fd = open(&quot;somefile.lck&quot;, O_CREAT | O_WRONLY, 0644&quot;);
    if (fd &lt; 0) {
        perror(&quot;error creating lock file&quot;);
        return 1;
    }
    /* we could write our pid to the file */  
    close(fd);  
}[](# "复制代码")

当一个进程处理完文件后，就可以调用unlink("somefile.lck")释放锁了——本质上是删除somefile.lck文件。  
上面这段代码实际上存在竞争情况，原因在于if语句块不是原子性的，进入if语句块，内核可能调度别的进程运行。更好的方式如下：

[](# "复制代码")

fd = open("somefile.lck", O_WRONLY | O_CREAT | O_EXCL, 0644);  
if (fd < 0 && errno == EEXIST) {  
    printf("the file is already locked");  
    return 1;  
} else if (fd < 0) {  
    perror("unexpected error checking lock");  
    return 1;  
}

/* we could write our pid to the file */  
close(fd);[](# "复制代码")

O_EXCL标志保证open()创建锁文件的过程是原子性的。  
注意以下几点：  
1、任何时刻只有一个进程可以拥有锁。  
2、O_EXCL标志只对本志文件系统是可靠的，对于网络文件系统并不能很好的支持。  
3、锁仅仅只是建议性的。  
4、如果一个持有锁的进程不正常结束，锁文件仍然存在。如果加锁进程的pid存储在锁文件中，其它进程可以检查锁进程是否存在，当它结束时就可以删除锁。但是，在检查的时候，如果pid被其它进程使用了，此时就无能为力了。

(2)记录锁（Record Locking）  
为了克服锁文件的缺点，System V和BSD4.3引入了记录锁，相应的系统调用为lockf()和flock()。而POSIX对于记录锁提供了另外一种机制，其系统调用为fcntl()。Linux提供三种接口，在这里仅讨论POSIX的接口。  
记录锁和锁文件有两个很重要的区别：首先，记录锁可以对文件的任何一部分加锁——这对于DBMS这样的应用程序，有极大的帮助，SQLite当然没有放过这样的好处。其次，记录锁的另一个优点就是它由内核持有，而不是文件系统持有。当进程结束时，所有的锁也随之释放。  
和锁文件一样，POSIX锁也是建议性的。记录锁有两种锁：读锁（read locks）和写锁（write locks）。读锁也就是共享锁（shared lock），写锁也就是排它锁（exclusive lock）。对于一个记录，只能有一个进程持有写锁，读锁不能存在。  
对于一个进程本身而言，多个锁绝不会冲突。如果一个进程对文件的200-250字节持有读锁，然后对200-225字节数据加写锁，是会成功的。此时，200-225为写锁，而226-250字节数据为读锁，该规则主要是防止进程本身发生死锁（尽管多进程之间仍然可能发生死锁）。

POSIX锁通过fcntl()系统来实现，如下：

#include <fcntl.h>

int fcntl(int fd, int command, long arg);

arg为指向flock结构体的指针：

[](# "复制代码")

#include <fcntl.h>

struct flock {  
    short l_type;  
    short l_whence;  
    off_t l_start;  
    off_t l_len;  
    pid_t l_pid;  
};

[](# "复制代码")

 在 flock 结构中，l_type 用来指明创建的是共享锁还是排他锁，其取值有三种：F_RDLCK（共享锁）、F_WRLCK（排他锁）和F_UNLCK（删除之前建立的锁）；l_pid 指明了该锁的拥有者；l_whence、l_start 和l_end 这些字段指明了进程需要对文件的哪个区域进行加锁，这个区域是一个连续的字节集合。因此，进程可以对同一个文件的不同部分加不同的锁。l_whence 必须是 SEEK_SET、SEEK_CUR 或 SEEK_END 这几个值中的一个，它们分别对应着文件头、当前位置和文件尾。l_whence 定义了相对于 l_start 的偏移量，l_start 是从文件开始计算的。

可以执行的操作包括：

    * F_GETLK：进程可以通过它来获取通过 fd 打开的那个文件的加锁信息。执行该操作时，lock 指向的结构中就保存了希望对文件加的锁（或者说要查询的锁）。如果确实存在这样一把锁，它阻止 lock 指向的 flock 结构所给出的锁描述符，则把现存的锁的信息写到 lock 指向的 flock 结构中，并将该锁拥有者的 PID 写入 l_pid 字段中，然后返回；否则，就将 lock 指向的 flock 结构中的 l_type 设置为 F_UNLCK，并保持 flock 结构中其他信息不变返回，而不会对该文件真正加锁。  
    * F_SETLK：进程用它来对文件的某个区域进行加锁（l_type的值为 F_RDLCK 或 F_WRLCK）或者删除锁（l_type 的值为F_UNLCK），如果有其他锁阻止该锁被建立，那么 fcntl() 就出错返回  
    * F_SETLKW：与 F_SETLK 类似，唯一不同的是，如果有其他锁阻止该锁被建立，则调用进程进入睡眠状态，等待该锁释放。一旦这个调用开始了等待，就只有在能够进行加锁或者收到信号时才会返回

需要注意的是，F_GETLK 用于测试是否可以加锁，在 F_GETLK 测试可以加锁之后，F_SETLK 和 F_SETLKW 就会企图建立一把锁，但是这两者之间并不是一个原子操作，也就是说，在 F_SETLK 或者 F_SETLKW 还没有成功加锁之前，另外一个进程就有可能已经插进来加上了一把锁。而且，F_SETLKW 有可能导致程序长时间睡眠。还有，程序对某个文件拥有的各种锁会在相应的文件描述符被关闭时自动清除，程序运行结束后，其所加的各种锁也会自动清除。

* Windows中的文件锁

Windows中的锁都是强制锁(mandatory locks)，Windows中的共享文件通过以下几个机制来管理：  
(1)    通过共享访问控制方式，应用程序可以指定整个文件进行共享读，写或者删除。  
(2)    通过字节范围锁（byte range locks）可以对文件的某一部分进行读写访问。  
(3)    Windows文件系统不允许正在执行的文件被打开用来进行写或删除操作。  
文件的共享方式由WIN32 API中的打开文件函数CreateFile()中的sharing mode参数确定：

[](# "复制代码")

HANDLE CreateFile(  
  LPCTSTR lpFileName,  
  DWORD dwDesiredAccess,  
  DWORD dwShareMode,  
  LPSECURITY_ATTRIBUTES lpSecurityAttributes,  
  DWORD dwCreationDisposition,  
  DWORD dwFlagsAndAttributes,  
  HANDLE hTemplateFile  
);[](# "复制代码")

 dwShareMode的取值通常为：  
FILE_SHARE_DELETE：  
 　　Enables subsequent open operations on an object to request delete access.  
Otherwise, other processes cannot open the object if they request delete access.  
If this flag is not specified, but the object has been opened for delete access, the function fails.  
FILE_SHARE_READ：  
   　 Enables subsequent open operations on an object to request read access.  
Otherwise, other processes cannot open the object if they request read access.  
If this flag is not specified, but the object has been opened for read access, the function fails.  
FILE_SHARE_WRITE：  
Enables subsequent open operations on an object to request write access.  
Otherwise, other processes cannot open the object if they request write access.  
If this flag is not specified, but the object has been opened for write access, the function fails.

具体的实现函数：

[](# "复制代码")

BOOL LockFile(  
  HANDLE hFile,  
  DWORD dwFileOffsetLow,  
  DWORD dwFileOffsetHigh,  
  DWORD nNumberOfBytesToLockLow,  
  DWORD nNumberOfBytesToLockHigh  
);[](# "复制代码")

* SQLite封锁机制的几个注意点

SQLite的lock byte的定义如下：

#define PENDING_BYTE      0x40000000  /* First byte past the 1GB boundary */  
#define RESERVED_BYTE     (PENDING_BYTE+1)  
#define SHARED_FIRST      (PENDING_BYTE+2)  
#define SHARED_SIZE       510

(1)PENDING_BYTE为何设置为0X4000 0000（1GB）？  
在Windows文件中，被加锁的区域不要求有数据，并且它会阻止所有的进程写文件的该区域,包括第一个持有该锁的进程.为了防止出现由于对含有mandatory lock的页面进行读写操作而出现错误(这在Windows中是不允许的),SQLite完全忽略包含pending byte的页面,所以pending byte在数据库文件上产生一个”文件洞”。PENDING_BYTE设置得那么高,则大部分数据库文件不会遇到由于PENDING_BYTE产生”文件洞”引起的空间损失(除非文件特别大,超过1GB)。

(2)对于Windows来说，文件中加锁的区域不能重叠，为了使两个读进程可以同时访问文件，对于SHARED LOCK选择一个SHARED_FIRST——SHARED_FIRST+ SHARED_SIZE范围内的随机数，所以有可能两个进程取得一样的lock byte，所以对于Windows，SQLite的并发性就受到限制。

‍

# [SQLite入门与分析(七)---浅谈SQLite的虚拟机](http://www.cnblogs.com/hustcat/archive/2009/03/18/1415752.html)

 写在前面：虚拟机技术在现在是一个非常热的技术，它的历史也很悠久。最早的虚拟机可追溯到IBM的VM/370，到上个世纪90年代，在计算机程序设计语言领域又出现一件革命性的事情——Java语言的出现，它与c++最大的不同在于它必须在Java虚拟机上运行。Java虚拟机掀起了虚拟机技术的热潮，随后，Microsoft也不甘落后，雄心勃勃的推出了.Net平台。由于在这里主要讨论SQLite的虚拟机，不打算对这些做过多评论，但是作为对比，我会先对Java虚拟机作一个概述。好了，下面进入正题。

 1、概述  
所谓虚拟机是指对真实计算机资源环境的一个抽象，它为解释性语言程序提供了一套完整的计算机接口。虚拟机的思想对现在的编译有很大影响，其思路是先编译成虚拟机指令，然后针对不同计算机实现该虚拟机。  
虚拟机定义了一组抽象的逻辑组件，这些组件包括寄存器组、数据栈和指令集等等。虚拟机指令的解释执行包括3步：  
1．获取指令参数；  
2. 执行该指令对应的功能；  
3. 分派下一条指令。  
其中第一步和第三步构成了虚拟机的执行开销。  
很多语言都采用了虚拟机作为运行环境。作为下一代计算平台的竞争者，Sun的Java和微软的.NET平台都采用了虚拟机技术。Java的支撑环境是Java虚拟机（Java Virtual Machine，JVM），.NET的支撑环境是通用语言运行库（Common Language Runtime，CLR）。JVM是典型的虚拟机架构。  
Java平台结构如图所示。从图中可以看出，JVM处于核心位置，它的下方是移植接口。移植接口由依赖平台的和不依赖平台的两部分组成，其中依赖于平台的部分称为适配器。JVM通过移植接口在具体的操作系统上实现。如果在Java操作系统（Java Operation System, JOS）上实现，则不需要依赖于平台的适配器，因为这部分工作已由JOS完成。因此对于JVM来说，操作系统和更低的硬件层是透明的。在JVM的上方，是Java类和Java应用程序接口（Java API）。在Java API上可以编写Java应用程序和Java小程序（applet）。所以对于Java应用程序和applet这一层次来说，操作系统和硬件就更是透明的了。我们编写的Java程序，可以在任何Java平台上运行而无需修改。

 ![](52a28081f75f58305757a20c8fe934e5-20220901162619-yxua4o9.jpg)​

JVM定义了独立于平台的类文件格式和字节码形式的指令集。在任何Java程序的字节码表示形式中，变量和方法的引用都是使用符号，而不是使用具体的数字。由于内存的布局要在运行时才确定，所以类的变量和方法的改变不会影响现存的字节码。例如，一个Java程序引用了其他系统中的某个类，该系统中那个类的更新不会使这个Java程序崩溃。这也提高了Java的平台独立性。  
虚拟机一般都采用了基于栈的架构，这种架构易于实现。虚拟机方法显著提高了程序语言的可移植性和安全性，但同时也导致了执行效率的下降。

 2、Java虚拟机

2.1、概述  
Java虚拟机的主要任务是装载Class文件并执行其中的字节码。Java虚拟机包含一个类装载器(class  loader)，它从程序和API中装载class文件，Java API中只有程序执行时需要的那些类才会被装载，字节码由执行引擎来执行。  
不同的Java虚拟机，执行引擎的实现可能不同。在软件实现的虚拟机中，一般有几下几中实现方式：  
（1）    解释执行：实现简单，但速度较慢，这是Java最初阶段的实现方式。  
（2）    即时编译(just-in-time)：执行较快，但消耗内存。在这种情况下，第一次执行的字节码会编译成本地机器代码，然后被缓存，以后可以重用。  
（3）    自适应优化器：虚拟机开始的时候解释字节码，但是会监视程序的运行，并记录下使用最频繁的代码，然后把这些代码编译成本地代码，而其它的代码仍保持为字节码。该方法既提高的运行速度，又减少了内存开销。  
同样，虚拟机也可由硬件来实现，它用本地方法执行Java字节码。  
​![](203e347787efbf256c00f70577251c04-20220901162619-16lz23m.jpg)​

2.2、Java虚拟机

Java虚拟机的结构分为：类装载子系统，运行时数据区，执行引擎，本地方法接口。其中运行时数据区又分为：方法区，堆，Java栈，PC寄存器，本地方法栈。

 ![](ce0c2defcf0c92c44a9e85e76c7e6dd7-20220901162619-w8lwnxy.jpg)​

关于Java虚拟机就介绍到此,由于Java虚拟机内容庞大，在这里不可能一一介绍，如果想更多了解Java虚拟机，参见《深入Java虚拟机》。

3、SQLite虚拟机

 在SQLite的后端（backend）的上一层，通常叫做虚拟数据库引擎(virtual database engine)，或者叫做虚拟机(virtual machine)。从作用上来说，它是SQLite的核心。用户程序发出的SQL语句请求，由前端(frontend)编译器（以后会继续介绍）处理，生成字节代码程序（bytecode programs），然后由VM解释执行。VM执行时，又会调用B-tree模块的相关的接口，并输出执行的结果（本节将以一个具体的查询过程来描述这一过程）。

3.1、虚拟机的内部结构

先来看一个简单的例子：

[](# "复制代码")

int main(int argc, char **argv)  
{  
    int rc, i,  id, cid;  
    char *name;  
    char *sql;  
    char *zErr;  
    sqlite3 *db; sqlite3_stmt *stmt;  
    sql="select id,name,cid from episodes";  
    //打开数据库  
    sqlite3_open("test.db", &db);  
    //编译sql语句  
    sqlite3_prepare(db, sql, strlen(sql), &stmt, NULL);  
    //调用VM，执行VDBE程序  
    rc = sqlite3_step(stmt);

    while(rc == SQLITE_ROW) {  
        id = sqlite3_column_int(stmt, 0);  
        name = (char *)sqlite3_column_text(stmt, 1);
        cid = sqlite3_column_int(stmt, 2);
        if(name != NULL){
            fprintf(stderr, &quot;Row:  id=%i, cid=%i, name='%s'\n&quot;, id,cid,name);
        } else {
            /* Field is NULL */  
            fprintf(stderr, "Row:  id=%i, cid=%i, name=NULL\n", id,cid);  
        }  
        rc = sqlite3_step(stmt);  
    }  
    //释放资源  
    sqlite3_finalize(stmt);  
    //关闭数据库  
    sqlite3_close(db);  
    return 0;  
}  
[](# "复制代码")

 这段程序很简单，它的功能就是遍历整个表，并把查询结果输出。  
在SQLite 中，用户发出的SQL语句，都会由编译器生成一个虚拟机实例。在上面的例子中，变量sql代表的SQL语句经过sqlite3_prepare()处理后，便生成一个虚拟机实例——stmt。虚拟机实例从外部看到的结构是sqlite3_stmt所代表的数据结构，而在内部，是一个vdbe数据结构代表的实例。  
关于这点可以看看它们的定义：  
//sqlite3.h  
typedef struct sqlite3_stmt sqlite3_stmt;

vdbe的定义：

Code

 由vdbe的定义，可以总结出SQLite虚拟机的内部结构：

![](c69fd3de0341eeff74effa136783be9d-20220901162619-x0i3308.jpg)​

 3.2、指令

int nOp;            /* Number of instructions in the program(指令的条数) */
Op ​*​**aOp;            /**​*​ Space to hold the virtual machine's program(指令)*/

 aOp数组保存有SQL经过编译后生成的所有指令，对于上面的例子为：

[](# "复制代码")

0、Goto(0x5b-91)    |0|0c  
1、Integer(0x2d-45) |0|0  
2、OpenRead(0x0c-12)|0|2  
3、SetNumColumns(0x64-100)|0|03  
4、Rewind(0x77-119) |0|0a  
5、Rowid(0x23-35)   |0|0  
6、Column(0x02-2)   |0|1  
7、Column(0x02-2)   |0|2  
8、Callback(0x36-54)|3|0  
9、Next(0x68)       |0|5  
10、Close  
11、Halt  
12、Transaction(0x66-102)|0|0  
13、VerifyCookie(0x61-97)|0|1  
14、Goto(0x5b-91)    |0|1|[](# "复制代码")

 sqlite3_step()引起VDBE解释引擎执行这段代码，下面来分析该段指令的执行过程：

Goto：这是一条跳转指令，它的作用仅仅是跳到第12条指令；  
Transaction：开始一个事务（读事务）；  
Goto：跳到第1条指令；  
Integer：把操作数P1入栈，这里的0表示OpenRead指令打开的数据库的编号；  
OpenRead：打开表的游标,数据库的编号从栈顶中取得，P1为游标的编号，P2为root page。  
               如果P2<=0,则从栈中取得root page no；

SetNumColumns：对P1确定的游标的列数设置为P2（在这里为3），在OP_Column指令执行前,该指令应该被调用来

               设置表的列数；

Rewind：移动当前游标（P1）移到表或索引的第一条记录；  
Rowid：把当前游标（P1）指向的记录的关键字压入栈；  
Column：解析当前游标指定的记录的数据，p1为当前游标索引号，p2为列号，并将结果压入栈中；

Callback：该指令执行后，PC将指向下一条指令。该指令的执行会结束sqlite3_step()的运行，并向其返回

              SQLITE_ROW ——如果存在记录的话；并将VDBE的PC指针指向下一条指令——即Next指令，所以当

             重新 调用sqlite3_step()执行VDBE程序时，会执行Next指令（具体的分析见后面的指令实例分析）；

Next：将游标移到下一条记录，并将PC指向第5条指令；  
Close：关闭数据库。

 3.3、栈

Mem *aStack;        /* The operand stack, except string values(栈空间) */  
  Mem *pTos;          /* Top entry in the operand stack(栈顶指针) */

 aStack是VDBE执行时使用的栈，它主要用来保指令执行进需要的参数，以及指令执行时产生的中间结果(参见后面的指令实例分析)。  
在计算机硬件领域，基于寄存器的架构已经压倒基于栈的架构成为当今的主流，但是在解释性的虚拟机领域，基于栈架构的实现占了上风。

1. 从编译的角度来看，许多编程语言可以很容易地被编译成栈架构机器语言。如果采用寄存器架构，编译器为了获得好的性能必须进行优化，如全局寄存器分配（这需要对数据流进行分析）。这种复杂的优化工作使虚拟机的便捷性大打折扣。
2. 如果采用寄存器架构，虚拟机必须经常保存和恢复寄存器中的内容。与硬件计算机相比，这些操作在虚拟机中的开销要大得多。因为每一条虚拟机指令都需要进行很费时的指令分派操作。虽然其它的指令也要分派，但是它们的语义内容更丰富。
3. 采用寄存器架构时，指令对应的操作数位于不同寄存器中，对操作数的寻址也是一个问题。而在基于栈的虚拟机中，操作数位于栈顶或紧跟在虚拟机指令之后。由于基于栈的架构的简便性，一些查询语言的实现也采用了此种架构。  
    SQLite的虚拟机就是基于栈架构的实现。每一个vdbe都有一个栈顶指针，它保存着vdbe的初始栈顶值。而在解释引擎中也有一个pTos，它们是有区别的：  
    （1）vdbe的pTos：在一趟vdbe执行的过程中不会变化，直到相应的指令修改它为止，在上面的例子中，Callback指令会修改其值（见指令分析）。  
    （2）而解释引擎中的pTos是随着指令的执行而动态变化的,在上面的例子中,Integer,Column指令的执行都会引起解释引擎pTos的改变。

 3.4、指令计数器(PC)  
每一个vdbe都有一个程序计数器，用来保存初始的计数器值。和pTos一样，解释引擎也有一个pc，它用来指向VM下一条要执行的指令。

3.5、解释引擎  
经过编译器生成的vdbe最终都是由解释引擎解释执行的，SQLite的解释引擎实现的原理非常简单，本质上就是一个包含大量case语句的for循环，但是由于SQLite的指令较多（在version 3.3.6中是139条），所以代码比较庞大。  
SQLite的解释引擎是在一个方法中实现的：  
int sqlite3VdbeExec(  
  Vdbe *p                    /* The VDBE */  
)  
具体代码如下（为了阅读，去掉了一些不影响阅读的代码，具体见SQLite的源码）：

[](# "复制代码")

/*执行VDBE程序.当从数据库中取出一行数据时,该函数会调用回调函数(如果有的话),  
**或者返回SQLITE_ROW.  
*/  
int sqlite3VdbeExec(  
  Vdbe *p                    /* The VDBE */  
){

  //指令计数器  
  int pc;                    /* The program counter */  
  //当前指令  
  Op *pOp;                   /* Current operation */
  int rc = SQLITE_OK;        /* Value to return */  
  //数据库  
  sqlite3 *db = p-&gt;db;       /* The database */

  u8 encoding = ENC(db);     /* The database encoding */  
  //栈顶  
  Mem *pTos;                 /* Top entry in the operand stack */

  if( p->magic!=VDBE_MAGIC_RUN ) return SQLITE_MISUSE;

  //当前栈顶指针  
  pTos = p->pTos;

  if( p->rc==SQLITE_NOMEM ){  
    /* This happens if a malloc() inside a call to sqlite3_column_text() or  
    ** sqlite3_column_text16() failed.  */  
    goto no_mem;  
  }<br />  p->rc = SQLITE_OK;<br />  //如果需要进行出栈操作，则进行出栈操作  
  if( p->popStack ){  
    popStack(&pTos, p->popStack);  
    p->popStack = 0;  
  }  
  //表明栈中没有结果  
  p->resOnStack = 0;  
  db->busyHandler.nBusy = 0;

  //执行指令  
  for(pc=p->pc; rc==SQLITE_OK; pc++){  
    //取出操作码  
    pOp = &p->aOp[pc];

    switch( pOp->opcode ){  
        //跳到操作数P2指向的指令  
        case OP_Goto: {             /* no-push */  
          CHECK_FOR_INTERRUPT;  
          //设置pc  
          pc = pOp->p2 - 1;  
          break;  
            }

        //P1入栈  
        case OP_Integer: {  
          //当前栈顶指针上移  
          pTos++;  
          //设为整型  
          pTos->flags = MEM_Int;  
          //取操作数P1,并赋值  
          pTos->i = pOp->p1;  
          break;  
            }

        //其它指令的实现  
    }//end switch  
  }//end for  
}[](# "复制代码")

3.6、指令实例分析

 由于篇幅限制，仅给出几条的指令的实现，其它具体实现见源码。

 1、Callback指令

Code

 2、Rewind指令

Code

 3、Column指令

Code

 4、Next指令

Code

‍

绿色通道： [好文要顶](#) [关注我](#) [收藏该文](#)​[与我联系](http://space.cnblogs.com/msg/send/MrDB) ![](c5fd93bfefed3def29aa5f58f5173174-20220901162619-l493ghs.png)[#](# "分享至新浪微博")
