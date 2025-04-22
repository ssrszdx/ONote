# Global.asax 文件是什么

Global.asax 文件，有时候叫做 ASP.NET 应用程序文件，提供了一种在一个中心位置响应应用程序级或模块级事件的方法。你可以使用这个文件实现应用程序安全性以及其它一些任务。下面让我们详细看一下如何在应用程序开发工作中使用这个文件。

**概述**

   Global.asax 位于应用程序根目录下。虽然 Visual Studio .NET 会自动插入这个文件到所有的 ASP.NET 项目中，但是它实际上是一个可选文件。删除它不会出问题——当然是在你没有使用它的情况下。.asax 文件扩展名指出它是一个应用程序文件，而不是一个使用 aspx 的 ASP.NET 文件。

   Global.asax 文件被配置为任何（通过 URL 的）直接 HTTP 请求都被自动拒绝，所以用户不能下载或查看其内容。ASP.NET 页面框架能够自动识别出对Global.asax 文件所做的任何更改。在 Global.asax 被更改后ASP.NET 页面框架会重新启动应用程序，包括关闭所有的浏览器会话，去除所有状态信息，并重新启动应用程序域。

**编程**

   Global.asax 文件继承自HttpApplication 类，它维护一个HttpApplication 对象池，并在需要时将对象池中的对象分配给应用程序。Global.asax 文件包含以下事件：

·         Application_Init：在应用程序被实例化或第一次被调用时，该事件被触发。对于所有的HttpApplication 对象实例，它都会被调用。

·         Application_Disposed：在应用程序被销毁之前触发。这是清除以前所用资源的理想位置。

·         Application_Error：当应用程序中遇到一个未处理的异常时，该事件被触发。

·         Application_Start：在HttpApplication 类的第一个实例被创建时，该事件被触发。它允许你创建可以由所有HttpApplication 实例访问的对象。

·         Application_End：在HttpApplication 类的最后一个实例被销毁时，该事件被触发。在一个应用程序的生命周期内它只被触发一次。

·         Application_BeginRequest：在接收到一个应用程序请求时触发。对于一个请求来说，它是第一个被触发的事件，请求一般是用户输入的一个页面请求（URL）。

·         Application_EndRequest：针对应用程序请求的最后一个事件。

·         Application_PreRequestHandlerExecute：在 ASP.NET 页面框架开始执行诸如页面或 Web 服务之类的事件处理程序之前，该事件被触发。

·         Application_PostRequestHandlerExecute：在 ASP.NET 页面框架结束执行一个事件处理程序时，该事件被触发。

·         Applcation_PreSendRequestHeaders：在 ASP.NET 页面框架发送 HTTP 头给请求客户（浏览器）时，该事件被触发。

·         Application_PreSendContent：在 ASP.NET 页面框架发送内容给请求客户（浏览器）时，该事件被触发。

·         Application_AcquireRequestState：在 ASP.NET 页面框架得到与当前请求相关的当前状态（Session 状态）时，该事件被触发。

·         Application_ReleaseRequestState：在 ASP.NET 页面框架执行完所有的事件处理程序时，该事件被触发。这将导致所有的状态模块保存它们当前的状态数据。

·         Application_ResolveRequestCache：在 ASP.NET 页面框架完成一个授权请求时，该事件被触发。它允许缓存模块从缓存中为请求提供服务，从而绕过事件处理程序的执行。

·         Application_UpdateRequestCache：在 ASP.NET 页面框架完成事件处理程序的执行时，该事件被触发，从而使缓存模块存储响应数据，以供响应后续的请求时使用。

·         Application_AuthenticateRequest：在安全模块建立起当前用户的有效的身份时，该事件被触发。在这个时候，用户的凭据将会被验证。

·         Application_AuthorizeRequest：当安全模块确认一个用户可以访问资源之后，该事件被触发。

·         Session_Start：在一个新用户访问应用程序 Web 站点时，该事件被触发。

·         Session_End：在一个用户的会话超时、结束或他们离开应用程序 Web 站点时，该事件被触发。

这个事件列表看起来好像多得吓人，但是在不同环境下这些事件可能会非常有用。

使用这些事件的一个关键问题是知道它们被触发的顺序。Application_Init 和Application_Start 事件在应用程序第一次启动时被触发一次。相似地，Application_Disposed 和 Application_End 事件在应用程序终止时被触发一次。此外，基于会话的事件（Session_Start 和 Session_End）只在用户进入和离开站点时被使用。其余的事件则处理应用程序请求，这些事件被触发的顺序是：

·         Application_BeginRequest

·         Application_AuthenticateRequest

·         Application_AuthorizeRequest

·         Application_ResolveRequestCache

·         Application_AcquireRequestState

·         Application_PreRequestHandlerExecute

·         Application_PreSendRequestHeaders

·         Application_PreSendRequestContent

·         <<执行代码>>

·         Application_PostRequestHandlerExecute

·         Application_ReleaseRequestState

·         Application_UpdateRequestCache

·         Application_EndRequest

这些事件常被用于安全性方面。下面这个 C# 的例子演示了不同的Global.asax 事件，该例使用Application_Authenticate 事件来完成通过 cookie 的基于表单（form）的身份验证。此外，Application_Start 事件填充一个应用程序变量，而Session_Start 填充一个会话变量。Application_Error 事件显示一个简单的消息用以说明发生的错误。

protected void Application_Start(Object sender, EventArgs e) {  
Application["Title"] = "Builder.com Sample";  
}  
protected void Session_Start(Object sender, EventArgs e) {  
Session["startValue"] = 0;  
}  
protected void Application_AuthenticateRequest(Object sender, EventArgs e) {  
// Extract the forms authentication cookie  
string cookieName = FormsAuthentication.FormsCookieName;  
HttpCookie authCookie = Context.Request.Cookies[cookieName];  
if(null == authCookie) {  
// There is no authentication cookie.  
return;  
}  
FormsAuthenticationTicket authTicket = null;  
try {  
authTicket = FormsAuthentication.Decrypt(authCookie.Value);  
} catch(Exception ex) {  
// Log exception details (omitted for simplicity)  
return;  
}  
if (null == authTicket) {  
// Cookie failed to decrypt.  
return;  
}  
// When the ticket was created, the UserData property was assigned  
// a pipe delimited string of role names.  
string[2] roles  
roles[0] = "One"  
roles[1] = "Two"  
// Create an Identity object  
FormsIdentity id = new FormsIdentity( authTicket );  
// This principal will flow throughout the request.  
GenericPrincipal principal = new GenericPrincipal(id, roles);  
// Attach the new principal object to the current HttpContext object  
Context.User = principal;  
}  
protected void Application_Error(Object sender, EventArgs e) {  
Response.Write("Error encountered.");  
}

这个例子只是很简单地使用了一些Global.asax 文件中的事件；重要的是要意识到这些事件是与整个应用程序相关的。这样，所有放在其中的方法都会通过应用程序的代码被提供，这就是它的名字为Global 的原因。

**资源**

Global.asax 文件是 ASP.NET 应用程序的中心点。它提供无数的事件来处理不同的应用程序级任务，比如用户身份验证、应用程序启动以及处理用户会话等。你应该熟悉这个可选文件，这样就可以构建出健壮的ASP.NET 应用程序。

附：[*.ascx *.asax *.aspx.resx *.asax.resx是什么文件](http://www.cnblogs.com/yaqiong/archive/2008/06/06/1215363.html)

sln：解决方案文件，为解决方案资源管理器提供显示管理文件的图形接口所需的信息。  
.csproj:项目文件，创建应用程序所需的引用、数据连接、文件夹和文件的信息。  
.aspx：Web 窗体页由两部分组成：视觉元素（HTML、服务器控件和静态文本）和该页的编程逻辑。Visual Studio 将这两个组成部分分别存储在一个单独的文件中。视觉元素在.aspx 文件中创建。  
.aspx.cs：Web 窗体页的编程逻辑位于一个单独的类文件中，该文件称作代码隐藏类文件（.aspx.cs）。  
.cs： 类模块代码文件。业务逻辑处理层的代码。  
.asax：Global.asax 文件（也叫做 ASP.NET 应用程序文件）是一个可选的文件，该文件包含响应 ASP.NET 或 HTTP 模块引发的应用程序级别事件的代码。  
.config：Web.config 文件向它们所在的目录和所有子目录提供配置信息。  
.aspx.resx/.resx：资源文件，资源是在逻辑上由应用程序部署的任何非可执行数据。通过在资源文件中存储数据，无需重新编译整个应用程序即可更改数据。  
.XSD:XML schema的一种.从DTD,XDR发展到XSD  
.pdb:PDB（程序数据库）文件保持着调试和项目状态信息，从而可以对程序的调试配置进行增量链接。  
.suo:解决方案用户选项,记录所有将与解决方案建立关联的选项，以便在每次打开时，它都包含您所做的自定义设置。  
.asmx:asmx 文件包含 WebService 处理指令，并用作 XML Web services 的可寻址入口点  
.vsdisco（项目发现）文件 基于 XML 的文件，它包含为 Web 服务提供发现信息的资源的链接 (URL)。  
.htc:一个HTML文件,包含脚本和定义组件的一系列HTC特定元素.htc提供在脚本中implement组件的机制

.ascx 是用户控件代码文件  
.aspx webform html脚本文件  
.cs 是c#类文件)  
.vb 是vb类文件)  
.aspx.cs 和你的webform相关的后台c#代码文件,其实跟.cs是一样的  
.aspx.vb 和你的webform相关的后台VB代码文件,其实跟.vb是一样的  
web.config 配置文件  
.xml xml文件  
.css 样式表文件
