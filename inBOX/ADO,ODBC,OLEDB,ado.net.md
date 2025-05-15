# ADO,ODBC,OLEDB,ADO.net

一、ODBC

ODBC的介绍

ODBC(Open Database Connectivity)，开放数据库互连。ODBC是上个世纪八十年代末九十年代初出现的技术，它为编写关系数据库的客户软件提供了一种统一的接口。ODBC提供一个单一的API，可用于处理不同数据库的客户应用程序。使用ODBC API的应用程序可以与任何具有ODBC驱动程序的关系数据库进行通信。

ODBC(Open Database Connectivity,开放数据库互连)是微软公司开放服务结构(WOSA，Windows Open Services Architecture)中有关数据库的一个组成部分，它建立了一组规范，并提供了一组对数据库访问的标准API（应用程序编程接口）。这些API利用SQL来完成其大部分任务。ODBC本身也提供了对SQL语言的支持，用户可以直接将SQL语句送给ODBC。

应用程序要访问一个数据库，首先必须用ODBC管理器注册一个数据源，管理器根据数据源提供的数据库位置、数据库类型及ODBC驱动程序等信息，建立起ODBC与具体数据库的联系。这样，只要应用程序将数据源名提供给ODBC，ODBC就能建立起与相应数据库的连接。

二、 OLE DB

OLE-DB的由来

随着数据源日益复杂化，现今的应用程序很可能需要从不同的数据源取得数据，加以处理，再把处理过的数据输出到另外一个数据源中。更麻烦的是这些数据源可能不是传统的关系数据库，而可能是Excel文件，Email，Internet/Intranet上的电子签名信息。Microsoft为了让应用程序能够以统一的方式存取各种不同的数据源，在1997年提出了UniversalDataAccess(UDA)架构。UDA以COM技术为核心，协助程序员存取企业中各类不同的数据源。UDA以OLE-DB(属于操作系统层次的软件)做为技术的骨架。OLE-DB定义了统一的COM接口做为存取各类异质数据源的标准，并且封装在一组COM对象之中。藉由OLE-DB，程序员就可以使用一致的方式来存取各种数据。但仍然OLEDB是一个低层次的，利用效率不高。

OLE-DB的介绍

OLE DB（Object Link and embed 即对象连接与嵌入。）是微软的战略性的通向不同的数据源的低级应用程序接口。OLE DB不仅包括微软资助的标准数据接口开放数据库连通性（ODBC）的结构化问题语言（SQL）能力，还具有面向其他非SQL数据类型的通路。 作为微软的组件对象模型（COM）的一种设计，OLE DB是一组读写数据的方法（在过去可能被称为渠道）。OLD DB中的对象主要包括数据源对象、阶段对象、命令对象和行组对象。使用OLE DB的应用程序会用到如下的请求序列：初始化OLE 连接到数据源 发出命令 处理结果 释放数据源对象并停止初始化OLE

OLE DB标准中定义的新概念----OLE DB将传统的数据库系统划分为多个逻辑组件，这些组件之间相对独立又相互通信。这种组件模型中的各个部分被冠以不同的名称：数据提供者（Data Provider）。 提供数据存储的软件组件，小到普通的文本文件、大到主机上的复杂数据库，或者电子邮件存储，都是数据提供者的例子。有的文档把这些软件组件的开发商也称为数据提供者。

我们要开启如Access 数据库中的数据，必须用[http://](https://link.zhihu.com/?target=http%3A//ADO.NET)​[**ADO.NET**](https://link.zhihu.com/?target=http%3A//ADO.NET)透过OLE DB 来开启。[Web Page is Unavailable](https://link.zhihu.com/?target=http%3A//ADO.NET) 利用OLE DB 来取得数据，这是因为OLE DB 了解如何和许多种数据源作沟通，所以对OLE DB有相当程度的了解是很重要的。

OLE DB 和ODBC的区别

OLE DB，对象链接与嵌入数据库。 OLE DB在两个方面对ODBC进行了扩展。首先， OLE DB提供了一个数据库编程的COM接口；第二， OLE DB提供了一个可用于关系型和非关系型数据源的接口(可以访问更多类型数据库)。

由于OLEDB和ODBC 标准都是为了提供统一的访问数据接口，所以曾经有人疑惑：OLE DB 是不是替代ODBC 的新标准？答案是否定的。实际上，ODBC 标准的对象是基于SQL 的数据源（SQL-Based Data Source），而OLE DB 的对象则是范围更为广泛的任何数据存储。从这个意义上说，符合ODBC 标准的数据源是符合OLE DB 标准的数据存储的子集。

三、 ADO

ADO的由来

虽然OLE-DB允许程序员存取各类数据，是一个非常良好的架构，但是由于OLE-DB太底层化，而且在使用上非常复杂，需要程序员拥有高超的技巧，因此只有少数的程序员才有办法使用OLE-DB。这让OLE-DB无法广为流行。为了解决这个问题，并且让VB和脚本语言也能够藉由OLE-DB存取各种数据源，Microsoft同样以COM技术封装OLE-DB为ADO对象，(这一步是很重要的，实现了多种程序可以互相调，并且可以开发的语言也丰富了)简化了程序员数据存取的工作。由于 ADO成功地封装了OLE-DB大部分的功能，并且大量简化了数据存取工作，因此 ADO也逐渐被愈来愈多的程序员所接受。

ADO的介绍

微软公司的ADO (ActiveX Data Objects)是一个用于存取数据源的COM组件（类似一种通用的接口）。它提供了编程语言和统一数据访问方式OLE DB的一个中间层。允许开发人员编写访问数据的代码而不用关心数据库是如何实现的，而只用关心到数据库的连接。访问数据库的时候，关于SQL的知识不是必要的，但是特定数据库支持的SQL命令仍可以通过ADO中的命令对象来执行。

ADO被设计来继承微软早期的数据访问对象层，包括RDO (Remote DataObjects)和DAO(Data Access Objects)。ADO在1996年冬被发布。

ADO包括了6个类：Connection，Command，Recordset，Errors，Parameters，Fields

Connection对象表示了到数据库的连接，它管理应用程序和数据库之间的通信。 Recordset和Command对象都有一个ActiveConnection属性，该属性用来引用Connection对象

Command对象被用来处理重复执行的查询，或处理需要检查在存储过程调用中的输出或返回参数的值的查询、

Recordset对象被用来获取数据。 Recordset对象存放查询的结果，这些结果由数据的行(称为记录)和列(称为字段)组成。每一列都存放在Recordset的Fields集合中的一个Field对象中。

说说自己的理解

说通俗点 OLE DB和ODBC都是最底层的东西，而ADO对象给我们提供了一个“可视化”，和应用层直接交互的组件，我们不用过多的关注OLEDB的内部机制，只需要了解ADO通过OLE DB创建数据源的几种方法即可，就可以通过ADO轻松地获取数据源。可以说ADO是应用程序和数据底层的一个中间层，ADO对象通过OLE DB间接取得数据库中的数据。OLE DB只是提供了通向各种数据库的一个通用接口，可以通过[https://](https://link.zhihu.com/?target=https%3A//blog.csdn.net/baidu_31437863/article/details/85798462)​[**blog.csdn.net/baidu\_314**](https://link.zhihu.com/?target=https%3A//blog.csdn.net/baidu_31437863/article/details/85798462)​[37863/article/details/85798462](https://link.zhihu.com/?target=https%3A//blog.csdn.net/baidu_31437863/article/details/85798462)。进行理解。

在再原本的操作数据的基础上加了一层ADO,通过ADO可以便捷操作。

多数内容参考

[ODBC、OLE DB、 ADO的区别](https://link.zhihu.com/?target=https%3A//blog.csdn.net/yinjingjing198808/article/details/7665577)

[MFC利用ADO技术访问数据库(c++) - A_Elite - 博客园](https://link.zhihu.com/?target=https%3A//www.cnblogs.com/aelite/articles/6581918.html)

‍

## ADO

ADO 代表 ActiveX 数据对象。在这种情况下，无论底层数据源如何，程序员都使用一组通用的对象。它需要锁定数据库资源和应用程序的长时间连接。在 ADO 中，连接是使用单个 Connection 对象创建的。

## ADO.NET

ADO.NET 是 ADO 的高级版本。它与数据库交互，并对各种数据源具有一致的访问权限。它的应用程序不需要锁定和冗长的连接。ADO.NET 可以有一个单独的对象来表示与不同数据源的连接，从而使访问更快、更高效。它使用多层架构，其中包括一些概念，例如 Connection、Command 和 DataSet 对象。ADO.NET 是真正适用于当前和未来应用程序的数据访问技术。

## ADO和ADO.NET的区别

以下是 ADO 和 ADO.NET 的区别的详细信息。

|对比项|ADO|ADO.NET|
| ------------------| ------------------------------------------------------------------------------------| --------------------------------------------------------------------------------------------------------------------------------------------------|
|数据表示|在 ADO 中，数据由一个 RecordSet 对象表示，该对象一次可以保存来自一个数据源的数据。|在 ADO.NET 中，数据由一个 DataSet 对象表示，该对象可以保存来自各种数据源的数据，将其集成，并将所有数据组合起来后，可以将其写回一个或多个数据源。|
|数据读取|在 ADO 中，数据是按顺序读取的。|在 ADO.NET 中，数据以顺序或非顺序方式读取。|
|断开访问|ADO 首先调用 OLE DB 提供程序。它支持连接访问，由与数据库的连接表示。|ADO.NET使用标准化调用(如 DataSetCommand 对象)与数据库和 OLE DB 提供程序进行通信。|
|创建游标|ADO只允许创建客户端游标。|ADO.NET 要么创建客户端游标，要么创建服务器端游标。|
|锁定|ADO 具有锁定功能。|ADO.NET 中不使用此功能。|
|XML 集成|此功能在 ADO 中未使用。|此功能用于 ADO.NET。|
|多个表之间的关系|它需要SQL Join 和UNION 来将来自多个表的数据组合到一个RecordSet 中。|ADO.NET使用 DataRelation 对象来组合来自多个表的数据，而无需使用 SQL Join 和 UNION。|
|转换|ADO 需要将数据转换为接收系统支持的数据类型。|ADO.NET 不需要浪费处理器时间的复杂转换。|
|数据存储|在 ADO 中，数据以二进制形式存储。|在 ADO.NET 中，数据以 XML 格式存储。通过使用 XML，它可以更轻松地存储数据。|
|防火墙|在 ADO 中，防火墙通常配置为阻止系统级请求，例如 COM 封送处理。|在 ADO.NET 中支持防火墙，因为 ADO.NET 数据集对象使用 XML 来表示数据。|
|通信|对象以二进制模式进行通信。|ADO.NET使用 XML 进行通信。|
|可编程性|它使用连接对象将命令传输到具有定义数据结构的数据源。|ADO.NET 不需要数据构造，因为它使用 XML 的强类型特征。|
|连接模型|ADO 仅支持一种连接模型，即面向连接的数据访问架构模型。|ADO.NET 支持两种连接模型，即面向连接的数据访问架构和面向断开连接的数据访问架构。|
|环境|ADO 适用于两层架构。|ADO.NET 解决了多层体系结构。|
|元数据|ADO在运行时根据元数据自动派生信息。|在 ADO.NET 中，在设计时派生有关元数据的信息，以提供更好的运行时性能。|
|多个事务|在 ADO 中，不能使用单个连接发送多个事务。|在 ADO.NET 中，可以使用单个连接发送多个事务。|
|数据类型|ADO的数据类型数量较少。|ADO.NET拥有庞大而丰富的数据类型集合。|
|序列化|ADO 在专有协议中序列化数据。|ADO.NET 使用 XML 序列化数据。|
|安全性|ADO 的安全性较低。|与 ADO 相比，ADO.NET 更加安全。|
|基于|ADO 是基于组件对象建模。|ADO.NET 是一个基于公共语言运行时的库。|
|可扩展性|ADO 技术的可扩展性较差。|ADO.NET 通过使用锁定机制具有更大的可扩展性。|
|资源|ADO 技术比 ADO.NET 使用更多的资源。|ADO.NET 节省了有限的资源。|
|性能|ADO 的性能较差。|与 ADO 相比，在 ADO.NET 中的性能要好得多。|

‍
