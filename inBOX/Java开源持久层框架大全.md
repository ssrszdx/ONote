# Java开源持久层框架大全

#### Hibernate

Hibernate是一个开放源代码的对象关系映射框架，它对JDBC进行了非常轻量级的对象封装，使得Java程序员可以随心所欲的使用对象编程思维来操纵数据库。 Hibernate可以应用在任何使用JDBC的场合，既可以在Java的客户端程序实用，也可以在Servlet/JSP的Web应用中使用，最具革命意义的是，Hibernate可以在应用EJB的J2EE架构中取代CMP，完成数据持久化的重任。Eclipse平台下的Hibernate辅助开发工具:【[Hibernate Synchronizer](http://www.open-open.com/open5704.htm)】【[MiddlegenIDE](http://www.open-open.com/open115804.htm)】[更多Hibernate信息](http://www.open-open.com/open2618.htm)

#### IBATIS

使用ibatis 提供的ORM机制，对业务逻辑实现人员而言，面对的是纯粹的Java对象， 这一层与通过Hibernate 实现ORM 而言基本一致，而对于具体的数据操作，Hibernate 会自动生成SQL 语句，而ibatis 则要求开发者编写具体的SQL 语句。相对Hibernate等 “全自动”ORM机制而言，ibatis 以SQL开发的工作量和数据库移植性上的让步，为系统 设计提供了更大的自由空间。作为“全自动”ORM 实现的一种有益补充，ibatis 的出现显 得别具意义。[更多IBATIS信息](http://www.open-open.com/open2918.htm)

#### JPOX

是一个 Java Data Objects (JDO)实现，提供了Java对象透明的一致性。JPOX 支持多维数据库(OLAP) 和RDBMS 数据库。也支持现存的模式[更多JPOX信息](http://www.open-open.com/open16718.htm)

#### Apache Torque

Apache Torque是一个使用关系数据库作为存储手段的Java应用程序持久化工具,是 Apache 的公开源代码项目，Torque是一个开源项目，由Web应用程序框架Jakarta Apache Turbine发展而来，但现在已完全独立于Turbine。 Torque 主要包含两部分：一部分是 Generator，它可以产生应用程序需要的所有数据库资源，包括 sql 和 java 文件；另外一部分是 Runtime，提供使用这些代码访问数据库的运行时环境。目前 Torque 支持的数据库包括 DB2、SQL Server、Oracle、PostgreSQL等。[更多Apache Torque信息](http://www.open-open.com/open16818.htm)

#### Castor

Castor 一个Java开放源码数据绑定框架,它主要目标是提供Java对象与XML 的绑定, Java到SQL的持久化等.[更多Castor 信息](http://www.open-open.com/open16918.htm)

#### Jaxor

Jaxor是一个简单但功能强大的创建到关系映像层对象的工具。它允许开发者轻松地在表中插入、更新、删除行，但也可被扩展为创建一个可扩展的映像层，这个层可创建一个完全的域模型，透明地映射到数据库表。[更多Jaxor信息](http://www.open-open.com/open17018.htm)

#### jdbm

jdbm是为Java提供的一个事务持久层，它旨在为用Perl, Python, C等作为GDBM 的Java应用程序使用，这是简单的持久层引擎是轻型而快速的。[更多jdbm信息](http://www.open-open.com/open17118.htm)

#### pBeans

这是基于Java的持久层。提供了自动的对象/关系（O/R)映射，可以将JavaBeans 映射到数据库表。功能有持久性、自动表创建、修改，许多查询，等等。[更多pBeans信息](http://www.open-open.com/open17218.htm)

#### Speedo

Speedo实现了JDO规范。它使用了objectweb设计的Jorm, Medor和Perseus 框架。[更多Speedo信息](http://www.open-open.com/open17318.htm)

#### XORM

XORM是为Java应用程序提供的一个可扩展的对象关系映射层。它使用Java Data Objects (JDO) API为RDBMS 提供了基于接口的持久性，同时也允许开发人员集中在对象模型，而不是物理层。[更多XORM信息](http://www.open-open.com/open17418.htm)

#### Apache Cayenne

除 Hibernate 之外的另一个开源 O/R 框架 Cayenne ，被成功用于商业生产环境。NHL.com 就是使用的 Cayenne ，每天超过 5 million 次的访问量。Apache Cayenne是一个强大而易于掌握的Java  ORM框架。Cayenne提供了 Java 对象到关系型数据库的持久化映射管理，单方法调用查询和更新（包括细粒度的更新所有被修改的对象），无缝隙的把多种数据库集成到单一虚拟数据源中。Cayenne 由 CayenneModeler 分配——完整的 GUI 映射工具。Cayenne 已被成功部署在高负载的生产环境中。  
[更多Apache Cayenne信息](http://www.open-open.com/open37218.htm)

#### Torque

Torque项目是Apache的公开源代码项目,主要用于生成访问数据库的资源和java代码、提供使用这些代码访问数据库的运行时(runtime)环境。通过使用Torque，你可以使用面向对象方式访问数据库，不再需要编写任何SQL语句。目前Torque支持的数据库包括mysql、oracle、sqlserver、db2等，还包括对weblogic的数据源的支持，[更多Torque信息](http://www.open-open.com/open48218.htm)

#### Voruta

Voruta是一个简单数据库访问框架。它通过特定的javadoc来封装sql的数据操作。其主页上有简单的Demo代码。  
[更多Voruta信息](http://www.open-open.com/open53718.htm)

#### JORM

JORM（Java对象存储映射)Java开源的持久性框架。它为JOnAS J2EE应用服务器提供EJB 2.0 CMP。JORM还与[Speedo](http://speedo.objectweb.org/) JDO实现结合。[更多JORM信息](http://www.open-open.com/open54318.htm)

#### Java Ultra-Lite Persistence

一个很小(少于50kB)持久层框架[更多Java Ultra-Lite Persistence信息](http://www.open-open.com/open56418.htm)

#### SimpleORM

SimpleORM是Java对象关系映射的开源项目.它在JDBC的基础上提供了一个简单但高效能的O/R映射.它甚至不需要XML配置文件.[更多SimpleORM信息](http://www.open-open.com/open56518.htm)

#### Prevayler

Prevayler一个把Java对象都保持在内存中的持久层框架,不需要数据库。可以这么说到目前为止对于POJOs(Plain Old Java Objects )是最快的，最显然的对象持久化，具有容错机制，提供负载平衡的框架。Prevayler在Eclipse下的插件[Preclipse](http://www.preclipse.de/web/index.html)​[更多Prevayler信息](http://www.open-open.com/open56718.htm)

#### DBCPersistence

DBCPersistence同样也是一个OR映射框架,但它在实现方式和API上与其它同类型框架有着不同之处.它的代码是使用字节码产生.这框架产生实现JDBC逻辑的CLASSES同样也是特殊的一对table/bean.DBCPersistence在运行期当需要的时候生成持久类.使个整个开发过程变得不太重要.这整个框架都是通过API进行配置,这样较大的改善了启动时间和减少了整个包的大小.[更多DBCPersistence信息](http://www.open-open.com/open56818.htm)

#### TJDO

TJDO是一个实现了Sun's JDO(JSR 12)规范的开源持久层框架.TJDO自从2001年以来已经成功地部署与运行在许多商业应用上。它具有以下特性：  
*经测试支持的数据库有Cloudscape, DB2, Firebird, MySQL, Oracle, PostgreSQL, SAP DB, 与MS SQL Server.  
*支持JDO 1.0.1。  
*实现所有JDOQL查询语言和其它一些有用的[方法](http://tjdo.sourceforge.net/docs/query_extensions.html)  
*自动创建所有需要的schema elements(表格，索引，外键)依据你的程序Class和JDO metadata  
*这是一个轻量级快速的框架  
[更多TJDO信息](http://www.open-open.com/open58618.htm)

#### TranQL

TranQL是一个开放源码,持久化引擎框架。通过JDBC支持SQL-92 和 SQL-03数据库引擎。支持EJB2.1。该项目被用来支持Geronimo J2EE应用服务器的持久化机制。[更多TranQL信息](http://www.open-open.com/open65018.htm)

#### TranQL

TranQL是一个开放源码,持久化引擎框架。通过JDBC支持SQL-92 和 SQL-03数据库引擎。支持EJB2.1。该项目被用来支持Geronimo J2EE应用服务器的持久化机制。[更多TranQL信息](http://www.open-open.com/open65118.htm)

#### O/R Broker

O/R Broker也是一个O/R映射工具,它允许使用构造函数,setter方法,JavaBean属性,直接域访问.开发者可以灵活地控制SQL,并允许执行细粒度的操作.[更多O/R Broker信息](http://www.open-open.com/open70018.htm)

#### Butler

Butler数据库框架是一个面向表格的Java对象模型(model).它基于JDBC可以让开发数据库程序变得容易.Butler有一组数据库swing组件与一个JSP标签库.[更多Butler信息](http://www.open-open.com/open80618.htm)

#### OJB

ObJectRelationalBridge-OJB是基于XML的对象/关系映射工具.OJB提供一些高级的特性如:对象缓存,延迟加载,利用事务隔离级别的结构进行分布式管理,支持悲观与乐观锁.OJB还提供了一个灵活的配置与插件机制以便可以扩展加入自己的功能.[更多OJB信息](http://www.open-open.com/open80818.htm)

#### PriDE

PriDE一个高性能的对象/关系映射框架.它有没有遵循任何持久层管理标准如JDO与EJB-CMP而是依赖于在J2SE与 J2EE环境中被实际证实可用的公认的设计模式.[更多PriDE信息](http://www.open-open.com/open80918.htm)

#### subPersistence

subPersistence是一个抽象(abstract)的,轻量级的而且灵活的对象/关系持久层框架.它提供了类似于Hibernate或Castor功能.[更多subPersistence信息](http://www.open-open.com/open81018.htm)

#### PAT

PAT是一个持久层工具包,像许多其它框架一样它简化了商业应用程序的持久层开发.PAT使用一些Java技术如:OO,AOP (JBossAOP),Java,Prevayler,Ant,JUnit,Log4j等为应用程序提供一个透明的数据层.它能够与web应用程序(Struts,Tomcat,JBoss AS)很好的相结合.[更多PAT信息](http://www.open-open.com/open86218.htm)

#### Mr.Persister

Mr.Persister是一个既简单又小的O/R映射API。可以从关系型数据库读取Java对象，也可以把Java对象写到数据库中。Mr. Persister主要的特点:  
 没有映射文件也不需要手动映射。  
 没有自己特有的查询语言。  
 Jar文件只有97KB。  
 Mr. Persister的运行体系都是以组件的方式实现。[更多Mr.Persister信息](http://www.open-open.com/open86618.htm)

#### Compass

Compass是一个强大的,事务的,高性能的对象/搜索引擎映射(OSEM:object/search engine mapping)与一个Java持久层框架.Compass包括:

* 搜索引擎抽象层(使用Lucene搜索引荐),
* OSEM (Object/Search Engine Mapping) 支持,
* 事务管理,
* 类似于Google的简单关键字查询语言,
* 可扩展与模块化的框架,
* 简单的API.[更多Compass信息](http://www.open-open.com/open88318.htm)

#### Bhavaya

Bhavaya是一个Java库它提供实时地与最新状态地(up-to-date)访问数据库数据.它一个包含持久层.这个框架利用数据库中的数据来填充Java对象并保持对象中的数据是最新的的.这个类库也提供许多当处理频繁地数据交换时经常要用的用户接口与工具类.[更多Bhavaya信息](http://www.open-open.com/open88418.htm)

#### Ammentos

Ammentos是一个适用于JDK5,轻量级的,开源的持久层框架. 它与JDK5注释(annotations)相结,支持事务,支持事件驱动编程,不需要配置,使用简单等.[更多Ammentos信息](http://www.open-open.com/open95018.htm)

#### Simple persistence

Simple persistence是一个O/R映射框架。它使用简单，没有XML映射文件、不需要创建表格(将自动创建)、不用生成ID、不用理会关键字，只需把它指向数据库,就可以实现新增、修改、删除、查询操作。Simple persistence支持事务，有自己的简单查询语言(类似于Hibernate的HQL)，并能够处理对象关联，lists和maps。[更多Simple persistence信息](http://www.open-open.com/open139418.htm)

#### EasyDBO

EasyDBO是一个非常适合中小型软件数据库开发的数据持久层框架，系统参考hibernate、JDO等，结合中小项目软件的开发实际，实现简单的对象-关系数据库映射。[更多EasyDBO信息](http://www.open-open.com/open143618.htm)

#### Speedframework

Speed 快速J2EE 开发框架Speedframework是一个完全基于JDBC开发的轻量级持久层框架. 它可以直接调用SQL,也可以直接对POJO进行CRUD操作,代码与ORM相当.调试方便,不用配置,内置JCS缓存,能有效降低数据库压力.  
speed框架具有如下特点：  
1.免配置持久层，免配置可以减少开发中配置带来的烦恼，调试带来的烦恼。  
2.完全是jdbc封装操作，性能完全没问题。  
3.jcs cache实现，对于数据库操作对象缓存减轻数据库压力。  
4.自带分页组件，完全可以直接传入一条sql即可完成困难的分页逻辑，可以由客户自定义。  
5.结合表、视图实体逻辑设计模式可以实现xp开发。  
6.speed能自动识别表字段pk的自增主键，并可以返回自增字段值。  
7.实现了jdbc的批处理封装，存储过程调用等jdbc api常用的封装。  
8.降低了入门门槛，有利于初期开发和中后期维护，适用于开发程序员经常更换的团队。[更多Speedframework信息](http://www.open-open.com/open155318.htm)

#### Ebean ORM

Ebean是一个对象/关系映射持久层框架。它与EJB3相类似，但该框架简单易于学习和使用。它特点： 1.兼容EJB3 ORM映射。2.支持级联保存和删除。3.支持懒加载。4.事务管理和日记功能。5.Statement Batching 5.支持缓存。6.Clustering。7.集成Lucene文本搜索。[更多Ebean ORM信息](http://www.open-open.com/open171018.htm)

#### Velosurf

Velosurf是一个基于Apache Velocity模板引擎的Java数据库映射层。它以一种非传统的方式来自动映射数据库表格和字段，而且还能够很方便定制自定义实体，查询和SQL行为。Velosurf主要特性包括：易于使用的模板语法，代码分离：SQL查询都集中在同一个地方并且看起来像标准的对象属性。动态映射：当数据库有变动时不需要重新编译。自动连接恢复。基本数据类型映射。事务控制。当需要的时候能够覆盖默认的Java映射对象。提供一些基础功能包括：权限控制机制，国际化支持，数据校验机制。[更多Velosurf信息](http://www.open-open.com/open173118.htm)

#### OpenJPA

OpenJPA是Apache组织的一个Java EE持久层开源项目，它实现了EJB3.0中的JPA标准，为开发者提供功能强大、 使用简单的持久化数据管理框架。OpenJPA封装了和关系型数据库交互的操作，让开发者把注意力集中在编写业务逻辑上。OpenJPA既可以作为独立的POJO持久层框架使用，也可以与所有符合EJB 3.0标准的容器或者其它轻量级框架相集成。[更多OpenJPA信息](http://www.open-open.com/open192618.htm)

#### Dcoat

Dcoat：Java持久层框架。Dcoat的理念就是：  
1，易学易用。不把在开发ORM框架本身中冒出的问题或概念带到用户面前。  
2, 高性能。在不用cache的情况下，保持与Jdbc同级的速度；设计高效率的cache，在有限空间里，解决或最大程度上缓解用户的性能问题。  
3，提倡清洁舒心编程。提供一套最小完整的接口和一些代码自动生成工具。  
4，高效率。这是为（dcoat的）客户提供的核心价值之一，也是我们开发dcoat中一直关注，强调和实施的重要目标。  
[更多Dcoat信息](http://www.open-open.com/open194818.htm)

#### jLynx

jLynx是一个简单、轻量级、高性能的持久层框架。它非常适合于中小应用程序开发，其jar文件大小只有32K并且不依赖任何第三方组件。jLynx的API远比Hibernate、EJB 或JPA来得简单。POJO与java.util.Map持久化都是使用现有JDBC标准。经测试支持的数据库包括：Microsoft SQL Server 2000+、Oracle 9i、10g、IBM DB2/UDB、MySQL和HSQL。 支持通过XML定义SQL查询。提供完整的示例包括POJO与JSP代码生成。  
![](0.623557986347667-20220205163634-8qd1h62.png)  
[更多jLynx信息](http://www.open-open.com/open197218.htm)

#### Floggy

Floggy是一个适用于J2ME/MIDP应程序的对象持久化框架。该框架封装了数据持久化的详细细节，减少了开发与维护的成本。  
Floggy由两个模块组成：* [Framework](http://floggy.sourceforge.net/persistence-framework/index.html)：负责提供持久方法比如saving、removing和finding object等。

* [Weaver](http://floggy.sourceforge.net/persistence-weaver/index.html)：负责分析、生成与编排字节码到持久化classe文件中。

[更多Floggy信息](http://www.open-open.com/open197418.htm)

#### jPersist

jPersist是一个非常强大，轻量级，对象-关系数据库持久API，所以不需要用到配置文件和注释（automatic）。映射是自动的。jPersist使用JDBC所以兼容任何关系型数据和任何类型连接资源。jPersist使用从数据库获得的消息来处理数据库与Java对象的映射。[更多jPersist信息](http://www.open-open.com/open198918.htm)

#### SeQuaLite

SeQuaLite是一个轻量级java数据库访问框架。具有的特性包括：提供CRUD操作、懒加载（Lazy-Load）、级联操作（Cascading）、分页（Paging）、动态SQL生成等。它能够帮助有效地减少开发时间。[更多SeQuaLite信息](http://www.open-open.com/open199518.htm)

#### ActiveObjects

ActiveObjects是一个纯Java ORM框架。AO有一套非常易于使用和简单的API。AO能自动根据用户指定的实体接口生成数据库schema。由于采用原生懒加载加上成熟的缓存机制，使得ActiveObjects与其它ORM框架相比较具有更高的性能。[更多ActiveObjects信息](http://www.open-open.com/open207718.htm)

#### Slice

Slice扩展自OpenJPA用于分布式数据库的一个开源项目。Slice以插件的方式附加至OpenJPA runtime，通过配置一个持久单元就能够激活多个数据库支持。一旦配置好Slice，现有OpenJPA应用程序就能够在同一个事务中利用多个数据库进行处理。查询也将依赖所有数据库并行执行，任何更新也会提交至相应的数据库。[更多Slice信息](http://www.open-open.com/open209118.htm)

#### DataNucleus Access Platform

DataNucleus Access Platform是一个符合标准的Java持久化引擎。它完全符合JDO1，JDO2，JDO2.1与JPA1 Java标准。此外它还遵循OGC简单要素规范（Simple Feature Specification）用于地理空间数据类型的持久化。DataNucleus支持当前所有流行RDBMS和db4o，LDAP，Excel文件，XML数据库。[更多DataNucleus Access Platform信息](http://www.open-open.com/open216218.htm)

#### COPE

相对于其它持久层框架，COPE能够让应用程序开发变得高效、快速。特性：不需要编写任何XML文件，所有配置都在java源代码中指定。不需要创建数据库Table，COPE自动创建。透明加载和存储持久对象。提供易于使用的搜索API用于复杂查询。完全与数据库隔离，消除SQL注入安全攻击。自带一个Web应用程序用于维护persistent schema并且不会丢失数据。经测试支持的数据库包括 [HSQLDB](http://www.hsqldb.org/)，[MySQL](http://www.mysql.com/)，Oracle和[PostgreSQL](http://www.postgresql.org/)。[更多COPE 信息](http://www.open-open.com/open218418.htm)

#### SeQuaLite

SeQuaLite是一个轻量级，java数据存取框架。支持CRUD操作。支持对象懒加载，通过创建代理对象或空对象来代替，等有需要时再加载。支持级联保存与级联删除操作。SeQuaLite使用 prepared statement来执行查询，因此它更快，更安全。使用SeQuaLite能够避免SQL注入安全威胁。SeQuaLite能够创建和执行复杂的查询/DML，并支持分页。[更多SeQuaLite信息](http://www.open-open.com/open219718.htm)

#### Objective Database Abstraction Layer

Objective database abstraction layer (ODAL) 是一个高性能的数据操作框架。特性包括：查询API，O-R映射，数据校验与类型转换，存储过程支持，代码生成，启动速度快。  
![](0.7008992194219812-20220205163634-xp48zwk.png)[更多Objective Database Abstraction Layer信息](http://www.open-open.com/open229418.htm)

#### Butterfly Persistence

Butterfly Persistence是一个简单，注重实效的Java持久层框架。它的特性包括：可自动或手动管理连接；通过提供类似于Spring的JDBC模板来简化JDBC操作；简单的对象/关系映射；支持多种映射方式（自动/注释/编程）。  
![Butterfly_Persistence.jpg](0.6836777534462952-20220205163634-mafr5gr.png)[更多Butterfly Persistence信息](http://www.open-open.com/open249618.htm)

#### Ujorm

Ujorm是一个开源的对象-关系映射实现框架（ORM ）。拥有一个类型安全的查询语言，可以让java编译器检查语法错误。支持懒加载，拥有比Hibernate更高的性能。ORM模型既可以通过Java源代码配置，也通过注释或XML文件配置。ORM可映射数据库中的表格，视图或自定义的SQL查询。JDBC查询参数通过问号传递给PreparedStatement，以提高安全性。所有内部对象缓存都基于WeakHashMap类实现，所以在处理大量事务的时候不会引会内存溢出错误。[更多Ujorm信息](http://www.open-open.com/open251618.htm)

#### Apache Empire-db

Apache Empire-db是一个开源的关系型数据持久化组件，能够实现数据库无关的动态查询定义，简便的数据读取和更新。与其它持久化组件相比如：Hibernate、TopLink、iBATIS或JPA实现，Empire-db更注重编译期类型安全，减少冗余，开发效率的改进。Empire-db所有的数据库实体都通过动态bean进行管理，因此允许在运行期改变数据模型。[更多Apache Empire-db信息](http://www.open-open.com/open255618.htm)

#### guzz

guzz是一种用来进行快速开发和高性能网站设计的框架，用于替代或者补充hibernate或ibatis的持久化实现，并提供更多的大型系统架构设计支持。guzz的目标是使得大型化网站设计更加简单，团队分工更加明确，框架在使用时更少出问题。主要设计理念：* 更容易的团队管理和人员分工

* 现代大规模系统设计
* 支持像hibernate一样的对象持久，映射和方便的增删改查
* 支持像ibatis一样，让dba参与sql设计的复杂数据库操作和优化
* 支持大量的数据库和主从分离
* 支持数据表在多组机器中水平分布（Shard）
* 组件化服务（SOA），构建企业基础服务平台
* 提高xx%倍效率的快速开发
* 支持配置管理服务器，对所有应用程序的配置进行统一管理

[更多guzz信息](http://www.open-open.com/open260118.htm)

#### Express-Persist

Express-Persist是一个能够减少JDBC复杂性的持久层框架。这个框架只需要DAO接口不需要任何JDBC代码。所有SQL操作都用Java5注释写在DAO接口中。在运行期能够动态创建DAO接口的实现不需要JDBC代码。支持本地事务处理。[更多Express-Persist信息](http://www.open-open.com/open260318.htm)

#### SimpleJPA

SimpleJPA是Java Persistence API（JPA）的一个实现，用于Amazon的SimpleDB云数据库。支持多对一、一对多映射，支持映射对象和集合的懒加载，利用Amazon S3实现LOB支持，提供缓存实现二次查询的快速响应，支持JPA Queries查询语法。[更多SimpleJPA信息](http://www.open-open.com/open264818.htm)

#### Ar4j

Ar4j是一个轻量级的持久层框架基于Rails中的ActiveRecord设计模式。所有JDBC操作都是使用Spring的简单JDBC框架执行。使用DB感知的POJOs来与数据库交互。支持自定义类型。提供最基本的CRUD操作（find、count、save、reload、delete）。要使用这些功能只需实现一个接口，不用继承特定的类。基于约定（Convention）的配置，一些需要细粒度控制则采用注释实现。支持原生SQL查询和名称查询。支持事务控制。[更多Ar4j信息](http://www.open-open.com/open269318.htm)

#### Persistence4j

Persistence4j是一个非常简单，轻便的持久层框架。映射关系采用Java注释实现。[更多Persistence4j信息](http://www.open-open.com/open270318.htm)

#### AutoDAO

AutoDAO的目标是让Java DAO类的创建变得尽可能简单。只要设计DAO接口，并在接口中利用注释编写必要的HQL，就能够实现所需要的功能。不需要编写实现代码和复杂的XML配置。对于Common DAO查询可以不用写任何持久化代码，支持Hibernate/JPA，支持在代码编译的时候就能够检查CRUD操作。支持分页，命名参数，命名查询和HQL校验以实现复杂HQL语句的简单化。[更多AutoDAO信息](http://www.open-open.com/open271718.htm)

#### Empire

Empire提供了一个基于SPARQL与SeRQL查询语言，类似于标准JPA风格的接口来访问RDF数据库。Empire的目标是尽可能多的实现JPA API，从而为RDF提供一个简单ORM持久层。收录时间：2010-10-11 10:10:19

[更多Empire信息](http://www.open-open.com/open280018.htm)

#### ORMLite

ORMLite是一个轻量级对象关系映射持久层框架。ORMLite支持MySQL、Postgres、Microsoft SQL Server、H2、Derby、HSQLDB和Sqlite。提供灵活的QueryBuilder来构建复杂的查询。强大的DAO抽象类，让你的数据库读写类只需5行代码，能够自动生成SQL来创建和删除数据库表格。![ORMLite.jpg](0.9307164672643367-20220205163634-o6dqep6.png)  
收录时间：2010-11-10 22:36:01

[更多ORMLite信息](http://www.open-open.com/open285618.htm)

#### GORM

GORM是Grails对象关联映射(GORM)的实现。在底层，它使用 Hibernate3，但是因为Groovy天生的动态性，实际上，对动态类型和静态类型两者都支持，由于Grails的规约，只需要很少的配置涉及Grails domain 类的创建。 你同样可以在Java中编写 Grails domain 类。 请参阅在 Hibernate 集成上如果在Java中编写 Grails domain 类， 不过，它仍然使用动态持久方法。收录时间：2010-11-25 13:52:38

[更多GORM信息](http://www.open-open.com/open288118.htm)

#### Morphia

Morphia是一个轻量级的类型安全的Java类库，用来将在MongoDB 和Java对象之间进行映射。支持类型安全查询，采用Java注释描述映射关系。收录时间：2010-12-19 17:32:39

[更多Morphia信息](http://www.open-open.com/open292918.htm)

#### DAO Fusion

DAO Fusion是一个轻量级，全面并且可扩展的Data Access Object（DAO）框架。基于Java Persistence API(JPA)和Hibernate构建。可以将DAO Fusion做为DAO层的一个基础框架，它封装了一些常用的数据库操作。![dataaccess.png](0.5624420588292987-20220205163634-qedv2sf.png)  
收录时间：2011-01-10 21:39:24

[更多DAO Fusion信息](http://www.open-open.com/open299218.htm)

#### Carbonado

Carbonado是一个可扩展、高性能的Java持久层框架。即使后台数据库不是基于SQL的，Carbonado仍然能够支持许多在任意关系型数据库中拥有的核心特性如： 查询, 关联、索引和执行查询优化。收录时间：2011-02-08 16:51:06

[更多Carbonado信息](http://www.open-open.com/open302518.htm)

#### Spring Data

Spring Data这个项目的目标主要是让访问No-SQL更加方便、支持map-reduce框架和云计算的数据服务。其第二个目标就是支持基于关系型数据库的数据服务，如Oracle RAC。对于拥有海量数据的项目，可以用Spring Data这样的项目来简化项目的开发，如Spring Framework刚诞生时支持JDBC，ORM一样，Spring Data会让数据的访问变得更加方便。Spring Data由多个子项目组成，支持CouchDB、MongoDB、Neo4J、Hadoop、HBase、Cassandra等。收录时间：2011-02-12 09:03:58

[更多Spring Data信息](http://www.open-open.com/open304318.htm)

#### SimpleJDBC

SimpleJDBC是一个用于简化JDBC代码的简单框架，需Spring集成。SimpleJDBC让你用简单的SQL语句完成增删改查，同时支持强类型和Java泛型，仅需注入一个Db实例。### 设计思想

1. 契约优于配置，表名和类名一致，字段名和属性名一致；
2. 不需编写DAO，为一两行SQL编写一个DAO方法不值；
3. 简单的SQL语句，而不是经过ORM改造的HQL；
4. 没有Attach/Detach状态，均为原始Bean无CGLIB代理；
5. 没有一级/二级Cache，Cache应当用memcached，用不上memcached则说明压力小到根本无需Cache；
6. 外键也映射到简单字段，而非对象，不支持一对多或多对一的级联查询，永远不用担心查出额外对象；
7. 泛型和强类型支持，有SQL语句，但无JDBC代码；
8. 不支持join等复杂查询，必须增加表的冗余以便使用简单查询。

收录时间：2011-03-11 08:50:42

[更多SimpleJDBC信息](http://www.open-open.com/open309618.htm)

#### JEPLayer

JEPLayer是一个构建在JDBC之上的简单ORM持久层框架。基于拦截监听模式。  
1)简单的API来避免处理乏味的JDBC任务。  
2) 多个可选的监听器来灵活定制JDBC持久化生命周期  
3) 构建简单和复杂DAOs的方法  
4) 一个非常简单，自动的，可配置的和无差错的方式来划分事务  
5) 它不会取代JDBC，而是当有需要的时候可以方便取得JDBC对象。  
6) Ever using PreparedStatement, ever secure  
7) PreparedStatement对象将自动缓存和复用。  
实际上JEPLayer是JBDC API反转控制版本。收录时间：2011-04-03 11:01:30

[更多JEPLayer信息](http://www.open-open.com/open313418.htm)

#### Gora

Gora是一个设计用于列存储数据库的ORM框架比较如：Apache HBase and Apache Cassandra。 特别专注于Hadoop。收录时间：2011-04-06 15:43:34

[更多Gora信息](http://www.open-open.com/open313618.htm)

#### 阿里巴巴CobarClient

CobarClient是一个轻量级分布式数据访问层（DAL）基于iBatis(已更名为MyBatis)和Spring框架实现。[中文文档](http://code.alibabatech.com/docs/cobarclient/zh/)1. 可以支持垂直和水平数据切分数据库集群的访问;

1. 支持双机热备的HA解决方案, 应用方可以根据情况选用数据库特定的HA解决方案(比如Oracle的RAC),或者选用CobarClient提供的HA解决方案.
2. 小数据量的数据集计(Aggregation), 暂时只支持简单的数据合并.
3. 数据库本地事务的支持, 目前采用Best Efforts 1PC模式的事务管理.
4. 数据访问操作相关SQL的记录, 分析等.(可以采用国际站现有Ark解决方案,但CobarClient提供扩展的切入接口)

收录时间：2011-04-23 10:44:08

[更多阿里巴巴CobarClient信息](http://www.open-open.com/open318118.htm)

#### UrSQL

UrSQL是一个用于Java桌面应用程序，简单的数据持久化存储API。UrSQL不需要任何类型的服务器或驱动器。它采用键-值对的方式来存储实体，让数据的存储尽可能简单。收录时间：2011-05-10 08:44:34

[更多UrSQL信息](http://www.open-open.com/open323218.htm)

#### kundera

kundera是一个开源的JPA1.0 ORM类库，主要用于NoSQL数据库：Cassandra/Hbase/MongoDB的数据持久化操作。收录时间：2011-05-17 10:17:26

[更多kundera信息](http://www.open-open.com/open325618.htm)

#### Easy Java Persistence

EJP是一个强大并且易于使用的关系数据库持久化Java API。EJP的主要特性包括：

  1、对象/关系（object/relational）自动映射(A-O/RM)  
  2、自动处理所有关联  
  3、自动持久化跟踪

EJP不需要映射注释或XML配置，并且不需要继承任何类或实现任何接口。EJP只用到了Plain Old Java Objects (POJOs)对象。到目前为止，EJP是Java开源中最简单的持久化API。  
收录时间：2011-06-15 23:07:48

[更多Easy Java Persistence信息](http://www.open-open.com/open332718.htm)

#### Persevere

Persevere是一组开源的工具用于持久化和分布式计算，使用一个直观基于HTTP REST、JSON-RPC、JSONPath和REST Channels标准JSON接口。Persevere项目的核心是Persevere Server，这个Persevere Server包含一个Persevere JavaScript Client，但是其基于标准的接口可以与任何框架集成使用或被任意客户端调用。Persevere Server是一个对象存储引擎和应用服务器(运行在Java/Rhino之上) ，它提供一个服务器JavaScript环境来实现动态JSON数据的持久化数据存储。支持通过标准JSON [HTTP/REST](http://www.ietf.org/rfc/rfc2616.txt) Web接口来创建、读取、更新和删除数据。收录时间：2011-06-21 09:07:28

[更多Persevere信息](http://www.open-open.com/open333718.htm)

#### Hibernate OGM (Object/Grid Mapper)

Hibernate Object/Grid Mapper (OGM)这个项目能够为NoSQL数据库提供Java Persistence(JPA)支持。它复用了Hibernate Core引擎将实体持久化至NoSQL数据存储中，而不是关系型数据库中。它还复用了Java Persistence Query Language(JP-QL)来搜索数据。这个项目现在还处于初期阶段，但随着时间的推移它的功能将逐渐增强。

短期目标是：

 1、支持Infinispan (已实现)  
 2、支持Hibernate Search全文搜索(已实现)  
 3、支持简单JP-QL查询

中期目标是：

 1、支持其它key/value存储  
 2、支持其它NoSQL数据库  
 3、支持复杂的关联和聚合 <Inﬁnispan 是个开源的数据网格平台。它公开了一个简单的数据结构（一个Cache）来存储对象。虽然可以在本地模式下运行Inﬁnspan，但其真正的价值在于分布 式，在这种模式下，Inﬁnispan可以将集群缓存起来并公开大容量的堆内存。这可比简单的复制强大的多，因为它会为每个结点分配固定数量的副本——服 务器故障的一种恢复手段——同时还提升了可伸缩性，这是由于存储每个结点所需的工作量是与集群大小息息相关的。

Inﬁnispan提供了一种简单的机制来利用大容量的堆内存。如果对每个结点维护一个拷贝，假如集群当中有100个结点，每个结点分配2GB的堆内存， 那么网格中的任何实例都能使用多达100GB的空间，这可都是内存，显然速度会非常快。同时Inﬁnispan还兼容于JTA，这样它就能很好地处理事务 了。我们还有一个超级强大的异步API，它可以保证同步的网络调用以及异步调用的并行性及可伸缩性。比方说：Future f = cache.putAsync(k, v) 可以阻塞线程，再调用f.get()可以让网络调用继续进行或是忽略掉f。更为重要的是，线程还可以做别的事情，这一点非常有用。然后再回来通过调用 f.get()来检查该网络调用是否能继续进行。可以将其看作是NIO与传统的阻塞性IO之间的关系。

Inﬁnispan公开了一个CacheStore接口和几个高性能的实现，包括JDBC CacheStores、基于文件系统的CacheStores以及Amazon S3 CacheStores等等。CacheStores可用作“温启动（warm starts）”或是确保网格中的数据在重启后依然可用，同时在内存耗尽时还能将数据写到磁盘上。

主要特点：

* 大量的堆体
* 极高的可扩展性
* 快速轻量级核心
* 不仅仅支持Java(PHP,Python,Ruby,C…)
* 支持Compute Grids
* 管理是关键：当你在grid上运行几百个服务时，实现管理是必须的

收录时间：2011-06-22 08:42:21

[更多Hibernate OGM (Object/Grid Mapper)信息](http://www.open-open.com/open333818.htm)

#### restSQL

restSQL是一个用于HTTP客户端的超轻量级数据访问层。实质上restSQL是一个持久层框架处于典型三层框架（客户端 - 应用服务器 - 数据库）中的中间层。它还可以作为一个Java类库嵌到其它应用的任何中间层中。restSQL使用简单的RESTful HTTP API来访问资源，基于XML或JSON数据格式。收录时间：2011-07-20 23:06:52

[更多restSQL信息](http://www.open-open.com/open336718.htm)

#### ActiveJDBC

ActiveJDBC是Active Record设计模式的一个Java实现。ActiveRecord ORM源于 Ruby on Rails。ActiveJDBC不是构建在Hibernate之上的一个持久层，也不是JPA的一个实现。它有自己的一套注释(可选)，大部分情况下是不用配置的，采用约定俗成的方式代替，将自动实现模型与数据库表格的映射。当前支持的数据库包括：MySQL、PostgreSQL、Oracle和H2。  
详细见：[http://java.sys-con.com/node/1912289](http://java.sys-con.com/node/1912289)  
收录时间：2011-07-21 08:59:19

[更多ActiveJDBC信息](http://www.open-open.com/open336818.htm)

#### MongoDB的Java开发框架 BuguMongo

BuguMongo是一个轻量级的MongoDB Java开发框架，它的主要功能包括：

1. 基于注解的对象-文档映射（Object-Document Mapping，简称ODM）。
2. DAO支持。提供了大量常用的DAO方法。
3. Query支持。提供了生成查询的简便方法。
4. 基于注解的Lucene索引。
5. 简单方便的Lucene搜索。支持关键词高亮显示。
6. 功能强大的GridFS文件系统管理。支持文件夹功能，支持文件的重命名、移动、排序等操作。
7. 简单方便的GridFS文件上传、读取。支持图片加水印、图片压缩。能用HTTP获取文件，并能使用HTTP缓存。

使用BuguMongo，可以让你：

1. 用面向对象的编程思维操纵MongoDB数据库。
2. 摆脱底层细节处理，专注于业务逻辑。
3. 大大减少代码量，提高开发效率。

收录时间：2011-10-20 22:55:12

[更多MongoDB的Java开发框架 BuguMongo信息](http://www.open-open.com/open345618.htm)

#### Burst

轻量级通用数据库开发框架(Java)框架的功能

1：对应Oracle, Db2, Sql Server, My sql四种数据库

2：使用Excel定义表结构，用宏自动创建表定义和数据模型的java类

3：自动创建和删除数据库表、索引、序列

4：封装了数据库连接池、CRUD、多表联合检索、多字段多匹配方式(equal,like,between….)、排序、分页检索、通过复杂条件update或delete数据 等常用数据库功能，直接操作对象，而不需要写任何sql

好处

1：框架非常简单，一看就会

2：开发迅速，简单的应用，Server端有个半天一天就够了

3：减少Bug，代码很规范，而且容易出错的地方都封装了

4：维护方便，就算是更改表结构，也很轻松

需要准备的开发环境

1：安装好数据库(以上4种之一，不推荐Sql server，比较麻烦)

2：安装好eclipse 或 myeclipse

3：安装好excel

使用方法

1：从google code下载项目文件和TblDesigner.xls

2：使用TblDesigner.xls定义表结构，运行宏，并将生成的目录覆盖到项目目录

3：配制db.properties文件

4：用eclipse打开项目，可以开始写业务逻辑了

5：functionTest包下有部分测试过程，可作参考
