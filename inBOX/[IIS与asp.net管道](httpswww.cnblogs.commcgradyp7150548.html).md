# [IIS与asp.net管道](https:www.cnblogs.commcgradyp7150548.html)

# [IIS与asp.net管道](https://www.cnblogs.com/mcgrady/p/7150548.html)

**阅读目录**

* [asp.net是什么](https://www.cnblogs.com/mcgrady/p/7150548.html#_label0)
* [HTTP协议](https://www.cnblogs.com/mcgrady/p/7150548.html#_label1)
* [IIS与asp.net](https://www.cnblogs.com/mcgrady/p/7150548.html#_label2)
* [asp.net管道](https://www.cnblogs.com/mcgrady/p/7150548.html#_label3)
* [参考资料](https://www.cnblogs.com/mcgrady/p/7150548.html#_label4)

　　我们在基于asp.net开发web程序，基本上都是发布部署到安装了IIS的windows服务器上，然后只要用户能够访问就算任务完成了，但是很少静下心来想想这背后到底发生了什么，那么这个系列就来总结下asp.net的基础原理。

[回到顶部](https://www.cnblogs.com/mcgrady/p/7150548.html#_labelTop)

## asp.net是什么

我们做web开发的可以说时时刻刻都在跟asp.net打交道，但很少总结asp.net是什么，可以用一句话总结：

**asp.net是一个开发web程序的平台。**

[回到顶部](https://www.cnblogs.com/mcgrady/p/7150548.html#_labelTop)

## HTTP协议

由于web程序是基于HTTP协议的，所以在继续深入了解asp.net之前有必要学习下HTTP协议。

HTTP协议是一个属于应用层，基于TCP/IP协议，用于从万维网服务器传输超文本到本地浏览器的传送协议。HTTP协议工作于客户端/服务端架构上，如下图。

​![](https://images2015.cnblogs.com/blog/311549/201707/311549-20170711151409056-1695799927.png)​

​![](https://images2015.cnblogs.com/blog/311549/201707/311549-20170711153610993-1024133775.png)​

​![](https://images2015.cnblogs.com/blog/311549/201707/311549-20170711153635134-1583182608.png)![](https://images2015.cnblogs.com/blog/311549/201707/311549-20170711153641853-1248060376.png)​

HTTP协议具有以下显著特点：

1，简单快速，客户端向服务端发起请求时，只需发送请求方法和url。常见请求方法包括：GET，POST等。

2，灵活，是因HTTP协议允许发送任意类型的数据对象，数据类型可以通过Content-Type指定。

3，无连接和无状态。无连接的意思是，每次连接只处理一个请求，服务端处理完请求并且客户端收到请求后即断开连接。无状态的意思是，每次的请求都是独立的。

4，一般都是基于B/S架构的。

[回到顶部](https://www.cnblogs.com/mcgrady/p/7150548.html#_labelTop)

## IIS与asp.net

 asp.net与IIS有着密不可分的联系的。

**1，IIS 5.x与asp.net**

IIS 5.x是集成在windows server 2000上的。

首先看下面的图，这张图展示了在IIS 5.x下如何处理asp.net程序请求的。

​![](https://images2015.cnblogs.com/blog/311549/201707/311549-20170711155116681-50712055.png)​

当检测到某个HTTP Request后，先根据扩展名判断请求的是否是静态资源（比如.html,.img,.txt,.xml等），如果是则直接将文件内容以HTTP Response的形式返回。如果是动态资源（比如.aspx,asp,php等等），则通过扩展名从IIS的脚本影射（Script Map）找到相应的ISAPI Dll。

**ISAPI**是Internet服务器API（Internet Server Application Programming Interface）的缩写，是一套本地的(Native)Win32 API，具有较高的执行性能,是IIS和其他动态Web应用或者平台之间的纽带。比如ASP ISAPI桥接IIS与ASP，而ASP.NET ISAPI则连接着IIS与ASP.NET。ISPAI定义在一个Dll中，ASP.NET ISAPI对应的Dll为**Aspnet_isapi.dll**,你可以在目录“%windir%\Microsoft.NET\Framework\{version no}\”中找到该Dll。

ISAPI支持ISAPI扩展（ISAPI Extension）和ISAPI筛选（ISAPI Filter），前者是真正处理HTTP请求的接口，后者则可以在HTTP请求真正被处理之前查看、修改、转发或者拒绝请求，比如IIS可以利用ISAPI筛选进行请求的验证（Authentication）。

如果我们请求的是一个基于ASP.NET的资源类型，比如：.aspx Web Page、 .asmx Web Service或者.svc WCF Service等，Aspnet_isapi.dll会被加载，ASP.NET ISAPI扩展会创建ASP.NET的工作进程（如果该进程尚未启动），对于IIS 5.x来说，该工作进程为aspnet.exe。IIS进程与工作进程之间通过命名管道（Named Pipes）进程通信，以获得最好的性能。

在工作进程初始化过程中，.NET 运行时（CLR）被加载，从而构建了一个托管的环境。对于某个Web应用的初次请求，CLR会为其创建一个AppDomain。在此AppDomain中，HTTP运行时（HTTP Runtime）被加载并用以创建相应的应用。对于寄宿于IIS 5.x的所有Web 应用都运行在同一个进程(工作进程aspnet_wp.exe)的不同AppDomain中。

**2，IIS 6.0与asp.net**

IIS 6.0是集成在windows server 2003上的。

**从上面可以看到，IIS 5.x仍存在以下两个问题：**

* **ISAPI Dll被加载到InetInfo.exe进程中，它和工作进程之间是一种典型的跨进程通信方式，尽管采用性能最好的命名管道，但是仍然会带来性能的瓶颈。**
* **所有的ASP.NET应用，运行在相同的进程（aspnet_wp.exe）中的不同的应用程序域（AppDomain）中，基于应用程序域的隔离级别不能从根本上解决一个应用程序对另一个程序的影响，在更多的时候，我们需要不同的Web应用运行在不同的进程中。**

那么IIS 6.0下是如何处理asp.net请求的呢，如下图。

​![](https://images2015.cnblogs.com/blog/311549/201707/311549-20170711165449072-1835634602.png)​

在IIS 6.0中，为了解决第一个问题，ISAPI.dll被直接加载到工作进程中。为了解决第2个问题，引入了应用程序池（Application Pool）的机制。我们可以为一个或者多个Web应用创建应用程序池，每一个应用程序池对应一个独立的工作进程，从而为运行在不同应用程序池中的Web应用提供基于进程的隔离级别。IIS 6.0的工作进程名称为w3wp.exe。

当然，除了上面两点改进之外，IIS 6.0还有其他一些值得称道的地方，其中最重要的一点就是创建了一个新的HTTP监听器：HTTP协议栈（HTTP Protocol Stack，HTTP.SYS）。HTTP.SYS运行在Windows的内核模式（Kernel Mode）下，作为驱动程序而存在。它是Windows 2003的TCP/IP网络子系统的一部分，从结构上，它属于TCP之上的一个网络驱动程序。严格地说，HTTP.SYS已经不属于IIS的范畴了，所以HTTP.SYS的配置信息并不保存在IIS的元数据库（Metabase），而是定义在注册表中。HTTP.SYS的注册表项位于下面的路径中：HKEY_LOCAL_MACHINE/SYSTEM/CurrentControlSet/Services/HTTP。HTTP.SYS能够带来如下的好处：

持续监听：由于HTTP.SYS是一个网络驱动程序，始终处于运行状态，对于用户的HTTP请求，能够及时作出反应；  
更好的稳定性：HTTP.SYS运行在操作系统内核模式下，并不执行任何用户代码，所以其本身不会受到Web应用、工作进程和IIS进程的影响；  
内核模式下数据缓存：如果某个资源被频繁请求，HTTP.SYS会把响应的内容进行缓存，缓存的内容可以直接响应后续的请求。由于这是基于内核模式的缓存，不存在内核模式和用户模式的切换，响应速度将得到极大的改进。  
图2体现了IIS的结构和处理HTTP请求的流程。从中可以看出，与IIS 5.x不同，W3SVC从InetInfo.exe进程脱离出来（对于IIS6.0来说，InetInfo.exe基本上可以看作单纯的IIS管理进程），运行在另一个进程SvcHost.exe中。不过W3SVC的基本功能并没有发生变化，只是在功能的实现上作了相应的改进。与IIS 5.x一样，元数据库（Metabase）依然存在于InetInfo.exe进程中。

当HTTP.SYS监听到用户的HTTP请求后，将其分发给W3SVC。W3SVC解析出请求的URL，并根据从Metabase获取的URL与Web应用之间的映射关系得到目标应用，并进一步得到目标应用运行的应用程序池或者工作进程。如果工作进程不存在（尚未创建或者被回收），则为该请求创建新的工作进程，工作进程的这种创建方式被称为请求式创建。在工作进程的初始化过程中，相应的ISAPI.dll被加载，对于ASP.NET应用来说，被加载的ISAPI.dll为Aspnet_ispai.dll。ASP.NET ISAPI再负责进行CLR的加载、AppDomain创建、Web Application的初始化等。

**所以，相比IIS 5.x，IIS 6.0的变化主要在以下方面：**

* **ISAPI.dll被直接加载到工作进程中，这解决了IIS 5.x的第1个问题。**
* **引入了应用程序池（Application Pool）的机制。**
* **创建了一个新的HTTP监听器，HTTP协议栈（HTTP Protocol Stack，即HTTP.sys）**

**3，IIS 7.0与asp.net**

IIS 7.0是集成在windows server 2008上的。

到了IIS 7.0，是如何处理asp.net请求的呢，如下图。

​![](https://images2015.cnblogs.com/blog/311549/201707/311549-20170711170437993-661274914.png)​

IIS 7.0对请求的监听和分发机制上又进行了革新性的改进，主要体现在对于Windows进程激活服务（Windows Process Activation Service，WAS）的引入，将原来（IIS 6.0）W3SVC承载的部分功能分流给了WAS。具体来说，通过上面的介绍，我们知道对于IIS 6.0来说，W3SVC主要承载着三大功能：

* HTTP请求接收：接收HTTP.sys监听到的HTTP请求；
* 配置管理：从元数据库（Metabase）中加载配置信息对相关组件进行配置；
* 进程管理：创建、回收、监控工作进程。

在IIS 7.0，后两组功能被移入WAS中，接收HTTP请求的任务依然落在W3SVC头上。WAS的引入为IIS 7.0一项前所未有的特性：同时处理HTTP和非HTTP请求。在WAS中，通过一个重要的接口：监听器适配器接口（Listener Adapter Interface）抽象出不同协议监听器监听到的请求。至于IIS下的监听器，除了基于网络驱动的HTTP.SYS提供HTTP请求监听功能外，WCF提供了3种类型的监听器：TCP监听器、命名管道（Named Pipes）监听器和MSMQ监听器，分别提供了基于TCP、命名管道和MSMQ传输协议的监听功能。与此3种监听器相对的，是3种监听器适配器（Adapter）提供监听器与监听器适配器接口之间的适配。从这个意义上讲，IIS 7.0中的W3SVC更多地为HTTP.SYS起着监听适配器的功能。WCF提供的这3种监听器和监听适配器定义在程序集SMHost.exe中，你可以通过下面的目录找到该程序集：%windir%\Microsoft.NET\Framework\v3.0\Windows Communication Foundatio。

WCF提供的这3种监听器和监听适配器最终以Windows Service的形式体现，虽然它们定义在一个程序集中，我们依然通过服务工作管理器（SCM，Service Control Manager）对其进行单独的启动、终止和配置。SMHost.exe提供了4个重要的Windows Service：

* NetTcpPortSharing：为WCF提供TCP端口共享，关于端口共享；
* NetTcpActivator：为WAS提供基于TCP的激活请求，包含TCP监听器和对应的监听适配器；
* NetPipeActivator：为WAS提供基于命名管道的激活请求，包含命名管道监听器和对应的监听适配器；
* NetMsmqActivator：为WAS提供基于MSMQ的激活请求，包含MSMQ监听器和对应的监听适配器。

下图为上述的4个Windows Service在服务控制管理器（SCM）中的呈现。

​![](https://images2015.cnblogs.com/blog/311549/201707/311549-20170711170741103-1858171576.png)​

第1张图揭示了IIS 7.0的整体构架以及整个请求处理流程。无论是从W3SVC接收到的HTTP请求，还是通过WCF提供的监听适配器接收到的请求，最终都会传递到WAS。如果相应的工作进程（或者应用程序池）尚未创建，其创建之；否则将请求分发给对应的工作进程进行后续的处理。WAS在进行请求处理过程中，通过内置的配置管理模块加载相关的配置信息对相关的组建进行配置，与IIS 5.x和IIS 6.0基于Metabase的配置信息存储不同的是，IIS 7.0大都将配置信息存放于XML形式的配置文件中。基本的配置存放在applicationHost.cofig中。

**所以，相比IIS 6.0，IIS 7.0的变化主要在以下方面：**

* **引入了Windows进程激活服务（Windows Process Activation Service，WAS），将原来（IIS 6.0）W3SVC承载的部分功能分流给了WAS，带来的好处是，可以同时处理HTTP请求和非HTTP请求。**
* **与IIS 5.x和IIS 6.0基于Metabase的配置信息存储不同的是，IIS 7.0大都将配置信息存放于XML形式的配置文件中。基本的配置存放在applicationHost.cofig中。**

[回到顶部](https://www.cnblogs.com/mcgrady/p/7150548.html#_labelTop)

## asp.net管道

先看两张图。

​![](https://images2015.cnblogs.com/blog/311549/201707/311549-20170711172319509-1411348762.png)　　　　　　　　asp.net管道

​![](https://images2015.cnblogs.com/blog/311549/201707/311549-20170711172353603-144998750.png)​

以IIS 6.0为例，在工作进程w3wp.exe中，利用Aspnet_ispai.dll加载.NET运行时（如果.NET运行时尚未加载）。IIS 6引入了应用程序池的概念，一个工作进程对应着一个应用程序池。一个应用程序池可以承载一个或者多个Web应用，每个Web应用映射到一个IIS虚拟目录。与IIS 5.x一样，每一个Web应用运行在各自的应用程序域中。

如果HTTP.SYS接收到的HTTP请求是对该Web应用的第一次访问，当成功加载了运行时后，会通过AppDomainFactory为该Web应用创建一个应用程序域（AppDomain）。随后，一个特殊的运行时IsapiRuntime被加载。IsapiRuntime定义在程序集System.Web中，对应的命名空间为System.Web.Hosting。IsapiRuntime会接管该HTTP请求。

IsapiRuntime会首先创建一个IsapiWorkerRequest对象，用于封装当前的HTTP请求，并将该IsapiWorkerRequest对象传递给ASP.NET运行时：HttpRuntime，从此时起，HTTP请求正式进入了ASP.NET管道。根据IsapiWorkerRequest对象，HttpRuntime会创建用于表示当前HTTP请求的上下文（Context）对象：HttpContext。

随着HttpContext被成功创建，HttpRuntime会利用HttpApplicationFactory创建新的或者获取现有的HttpApplication对象。实际上，ASP.NET维护着一个HttpApplication对象池，HttpApplicationFactory从池中选取可用的HttpApplication用户处理HTTP请求，处理完毕后将其释放到对象池中。HttpApplicationFactory负责处理当前的HTTP请求。

在HttpApplication初始化过程中，会根据配置文件加载并初始化相应的HttpModule对象。对于HttpApplication来说，在它处理HTTP请求的不同的阶段会触发不同的事件（Event），而HttpModule的意义在于通过注册HttpApplication的相应的事件，将所需的操作注入整个HTTP请求的处理流程。ASP.NET的很多功能，比如身份验证、授权、缓存等，都是通过相应的HttpModule实现的。

而最终完成对HTTP请求的处理实现在另一个重要的对象中：HttpHandler。对于不同的资源类型，具有不同的HttpHandler。比如.aspx页对应的HttpHandler为System.Web.UI.Page，WCF的.svc文件对应的HttpHandler为System.ServiceModel.Activation.HttpHandler。上面整个处理流程如第1图所示。

**HttpApplication**

HttpApplication是整个ASP.NET基础架构的核心，它负责处理分发给它的HTTP请求。由于一个HttpApplication对象在某个时刻只能处理一个请求，只有完成对某个请求的处理后，HttpApplication才能用于后续的请求的处理。所以，ASP.NET采用对象池的机制来创建或者获取HttpApplication对象。具体来讲，当第一个请求抵达的时候，ASP.NET会一次创建多个HttpApplication对象，并将其置于池中，选择其中一个对象来处理该请求。当处理完毕，HttpApplication不会被回收，而是释放到池中。对于后续的请求，空闲的HttpApplication对象会从池中取出，如果池中所有的HttpApplication对象都处于繁忙的状态，ASP.NET会创建新的HttpApplication对象。

HttpApplication处理请求的整个生命周期是一个相对复杂的过程，在该过程的不同阶段会触发相应的事件。我们可以注册相应的事件，将我们的处理逻辑注入到HttpApplication处理请求的某个阶段。我们接下来介绍的HttpModule就是通过HttpApplication事件注册的机制实现相应的功能的。表1按照实现的先后顺利列出了HttpApplication在处理每一个请求时触发的事件名称。

经典的asp.net管道的19个事件：

|名称|描述|
| -----------------------------------------------------| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|BeginRequest|HTTP管道开始处理请求时，会触发BeginRequest事件|
|AuthenticateRequest，PostAuthenticateRequest|ASP.NET先后触发这两个事件,使安全模块对请求进行身份验证|
|AuthorizeRequest，PostAuthorizeRequest|ASP.NET先后触发这两个事件,使安全模块对请求进程授权|
|ResolveRequestCache，PostResolveRequestCache|ASP.NET先后触发这两个事件，以使缓存模块利用缓存的直接对请求直接进程响应（缓存模块可以将响应内容进程缓存，对于后续的请求，直接将缓存的内容返回，从而提高响应能力）。|
|PostMapRequestHandler|对于访问不同的资源类型，ASP.NET具有不同的HttpHandler对其进程处理。对于每个请求，ASP.NET会通过扩展名选择匹配相应的HttpHandler类型，成功匹配后，该实现被触发|
|AcquireRequestState，PostAcquireRequestState|ASP.NET先后触发这两个事件，使状态管理模块获取基于当前请求相应的状态，比如SessionState|
|PreRequestHandlerExecute，PostRequestHandlerExecute|ASP.NET最终通过一请求资源类型相对应的HttpHandler实现对请求的处理，在实行HttpHandler前后，这两个实现被先后触发|
|ReleaseRequestState，PostReleaseRequestState|ASP.NET先后触发这两个事件，使状态管理模块释放基于当前请求相应的状态|
|UpdateRequestCache，PostUpdateRequestCache|ASP.NET先后触发这两个事件，以使缓存模块将HttpHandler处理请求得到的相应保存到输出缓存中|
|LogRequest，PostLogRequest|ASP.NET先后触发这两个事件为当前请求进程日志记录|
|EndRequest|整个请求处理完成后，EndRequest事件被触发|

对于一个ASP.NET应用来说，HttpApplication派生于global.asax文件，我们可以通过创建global.asax文件对HttpApplication的请求处理行为进行定制。global.asax采用一种很直接的方式实现了这样的功能，这种方式既不是我们常用的方法重写（Method Overriding）或者事件注册，而是直接采用方法名匹配。在global.asax中，我们按照这样的方法命名规则进行事件注册：Application_{Event Name}。比如Application_BeginRequest方法用于处理HttpApplication的BeginRequest事件。如果通过VS创建一个global.asax文件，下面是默认的定义。

​![复制代码](https://common.cnblogs.com/images/copycode.gif)[&quot;复制代码&quot;]()

```
1 <%@ Application Language="C#" %>
2 <script runat="server">
3    void Application_Start(object sender, EventArgs e) {}
4    void Application_End(object sender, EventArgs e) {}
5    void Application_Error(object sender, EventArgs e) {}
6    void Session_Start(object sender, EventArgs e) {}
7    void Session_End(object sender, EventArgs e) {}
8 </script>
```

​![复制代码](https://common.cnblogs.com/images/copycode.gif)[&quot;复制代码&quot;]()

**HttpModule**

ASP.NET为创建各种.NET Web应用提供了强大的平台，它拥有一个具有高度可扩展性的引擎，并且能够处理对于不同资源类型的请求。那么，是什么成就了ASP.NET的高可扩展性呢？ HttpModule功不可没。

从功能上讲，HttpModule之于ASP.NET，就好比ISAPI Filter之于IIS一样。IIS将接收到的请求分发给相应的ISAPI Extension之前，注册的ISAPI Filter会先截获该请求。ISAPI Filter可以获取甚至修改请求的内容，完成一些额外的功能。与之相似地，当请求转入ASP.NET管道后，最终负责处理该请求的是与请求资源类型相匹配的HttpHandler对象，但是在Handler正式工作之前，ASP.NET会先加载并初始化所有配置的HttpModule对象。HttpModule在初始化的过程中，会将一些功能注册到HttpApplication相应的事件中，那么在HttpApplication整个请求处理生命周期中的某个阶段，相应的事件会被触发，通过HttpModule注册的事件处理程序也得以执行。

所有的HttpModule都实现了IHttpModule接口，下面是IHttpModule的定义。其中Init方法用于实现HttpModule自身的初始化，该方法接受一个HttpApplication对象，有了这个对象，事件注册就很容易了。

```
1 public interface IHttpModule
2 {
3    void Dispose();
4    void Init(HttpApplication context);
5 }
```

ASP.NET提供的很多基础构件（Infrastructure）功能都是通过相应的HttpModule实现的，下面类列出了一些典型的HttpModule:

* OutputCacheModule：实现了输出缓存（Output Caching）的功能；
* SessionStateModule：在无状态的HTTP协议上实现了基于会话（Session）的状态；
* WindowsAuthenticationModule + FormsAuthenticationModule + PassportAuthentication- Module：实现了3种典型的身份认证方式：Windows认证、Forms认证和Passport认证；
* UrlAuthorizationModule + FileAuthorizationModule：实现了基于Uri和文件ACL（Access Control List）的授权。

而另外一个重要的HttpModule与WCF相关，那么就是System.ServiceModel. Activation.HttpModule。HttpModule定义在System.ServiceModel程序集中，在默认的情况下，HttpModule完成了基于IIS的寄宿工作。

除了这些系统定义的HttpModule之外，我们还可以自定义HttpMoudle。通过Web.config，我们可以很容易地将其注册到我们的Web应用中。

**HttpHandler**

如果说HttpModule相当于IIS的ISAPI Filter的话，我们可以说HttpHandler则相当于IIS的ISAPI Extension，HttpHandler在ASP.NET中扮演请求的最终处理者的角色。对于不同资源类型的请求，ASP.NET会加载不同的Handler来处理，也就是说.aspx page与.asmx web service对应的Handler是不同的。

所有的HttpHandler都实现了接口IHttpHandler。下面是IHttpHandler的定义，方法ProcessRequest提供了处理请求的实现。

```
1 public interface IHttpHandler
2 {
3    void ProcessRequest(HttpContext context);
4    bool IsReusable { get; }
5 }
```

对于某些HttpHandler，具有一个与之相关的HttpHandlerFactory，用于创建或者获取相应的HttpHandler。HttpHandlerFactory实现接口IHttpHandlerFactory，方法GetHandler用于创建新的HttpHandler，或者获取已经存在的HttpHandler。

```
1 public interface IHttpHandlerFactory
2 {
3    IHttpHandler GetHandler(HttpContext context, string requestType, string url, string pathTranslated);
4    void ReleaseHandler(IHttpHandler handler);
5 }
```

HttpHandler和HttpHandlerFactory的类型都可以通过相同的方式配置到Web.config中。下面一段配置包含对3种典型的资源类型的HttpHandler配置：.aspx,.asmx和.svc。可以看到基于WCF Service的HttpHandler类型为：System.ServiceModel.Activation.HttpHandler。

​![复制代码](https://common.cnblogs.com/images/copycode.gif)[&quot;复制代码&quot;]()

```
 1 <?xml version="1.0" encoding="utf-8" ?>
 2 <configuration>
 3     <system.web>
 4     <httpHandlers>
 5         <add path="*.svc" verb="*" type="System.ServiceModel.Activation.HttpHandler, System.ServiceModel, Version=3.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" validate="false"/>
 6         <add path="*.aspx" verb="*" type="System.Web.UI.PageHandlerFactory" validate="True"/>
 7         <add path="*.asmx" verb="*" type="System.Web.Services.Protocols.WebServiceHandlerFactory, System.Web.Services, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a" validate="False"/>
 8     </httpHandlers>
 9     </system.web>
10 </configuration>
```

​![复制代码](https://common.cnblogs.com/images/copycode.gif)[&quot;复制代码&quot;]()

[回到顶部](https://www.cnblogs.com/mcgrady/p/7150548.html#_labelTop)

## 参考资料

* [http://www.cnblogs.com/artech/archive/2009/06/20/1507165.html](http://www.cnblogs.com/artech/archive/2009/06/20/1507165.html)
* [http://www.cnblogs.com/OceanEyes/archive/2012/08/13/aspnetEssential-1.html](http://www.cnblogs.com/OceanEyes/archive/2012/08/13/aspnetEssential-1.html)
* [http://www.cnblogs.com/wenthink/archive/2013/05/06/HTTP_IIS_ASPNET_Pipeline.html](http://www.cnblogs.com/wenthink/archive/2013/05/06/HTTP_IIS_ASPNET_Pipeline.html)
