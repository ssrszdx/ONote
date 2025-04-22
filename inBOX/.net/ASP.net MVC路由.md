# ASP.net MVC路由

# [ASP.NET MVC5路由系统机制详细讲解](https://www.cnblogs.com/itliuyang/p/6872027.html)

请求一个ASP.NET mvc的网站和以前的web form是有区别的，ASP.NET MVC框架内部给我们提供了路由机制，当IIS接受到一个请求时，会先看是否请求了一个静态资源（.html,css,js,图片等），这一步是web form和mvc都是一样的，如果不是则说明是请求的是一个动态页面，就会走asp.net的管道，mvc的程序请求都会走路由系统，会映射到一个Controller对应的Action方法，而web form请求动态页面是会查找本地实际存在一个aspx文件。下面通过一个ASP.NET MVC5项目来详细介绍一下APS.NET MVC5路由系统的机制。

## 一、认识Global.asax.cs

当我们创建一个APS.NET MVC5的项目的时候会在项目的根目录中生成一个Global.asax文件。

![复制代码](0.9331693407557982-20220831222539-9kery20.png)[&quot;复制代码&quot;]("复制代码")

```
 1 public class MvcApplication : System.Web.HttpApplication
 2 {
 3         protected void Application_Start()
 4         {
 5             //注册 ASP.NET MVC 应用程序中的所有区域
 6             AreaRegistration.RegisterAllAreas();
 7             //注册 全局的Filters
 8             FilterConfig.RegisterGlobalFilters(GlobalFilters.Filters);
 9             //注册 路由规则
10             RouteConfig.RegisterRoutes(RouteTable.Routes);
11             //注册  打包绑定（js,css等）
12             BundleConfig.RegisterBundles(BundleTable.Bundles);
13         }
14 }
```

![复制代码](0.5053070324832032-20220831222539-jzhx6fp.png)[&quot;复制代码&quot;]("复制代码")

这个Application_Start方法会在网站启动的自动调用，其中我们看到：RouteConfig.RegisterRoutes(RouteTable.Routes);这个就是向ASP.NET MVC 框架注册我们自定义的路由规则，让之后的URL能够对应到具体的Action。接下来我们再来看看RegisterRoutes方法做了些什么？

![复制代码](0.7930797448766957-20220831222539-arrp0lm.png)[&quot;复制代码&quot;]("复制代码")

```
 1 public class RouteConfig
 2 {
 3         public static void RegisterRoutes(RouteCollection routes)
 4         {
 5             routes.IgnoreRoute("{resource}.axd/{*pathInfo}");
 6             routes.MapRoute(
 7                 name: "Default",
 8                 url: "{controller}/{action}/{id}",
 9                 defaults: new { controller = "Home", action = "Index", id = UrlParameter.Optional }
10             );
11         }
12 }
```

![复制代码](0.6451763835681983-20220831222539-1kphkyw.png)[&quot;复制代码&quot;]("复制代码")

上面代码是vs自动为你们生成，只定义了一个默认规则。

1、规则名：Default

2、URL分段：{controller}/{action}/{id}，分别有三段，第一段对应controller参数，第段为action参数，第三段为id参数

3、URL段的默认值：controller为Home，action为Index，id = UrlParameter.Optional表示该参数为可选的。

之所以我们访问http://www.xx.com/ 这样的URL网址能正确返回，是因为我们设置了URL段的默认值，相当于访问：

http://www.xx.com/Home/Index

RegisterRoutes调用的是RouteCollection的MapRoute方法，RouteCollection是一个集合，继承于Collection<RouteBase>

![](0.09177290198851162-20220831222539-hxpalna.png)​

## 二、ASP.NET MVC默认的命名约定

### 1、Controller命名约定

Controller类必须以Controller结尾，比如：HomeController，ProductController。我们在页面上用HTML heper来引用一个Controller的时只需要前面Home，Product就可以，ASP.NET MVC框架自带的DefaultControllerFactory自动为我们在结尾加上Controller，并开始根据这个名字开始找对应的类。我们创建一个ASP.NET MVC项目新加的Controller，文件会自动放在根目录的Controllers文件夹里面，我们刚开始可以看到有一个HomeController.cs。当然你也可以实现接口IControllerFactory，定义自己的ControllerFactory来改变查找Controller文件的行为。我会再以后的文章中介绍。

### 2、View命名约定

ASP.NET MVC的视图View默认情况是放在根目录的Views文件下的，规则是这样的：/Views/ControllerName/ActionName.cshtml。比如：HomeController的Action名字为Index的视图对应文件为：/Views/Home/Index.cshtml

因此是通过Controller和Action的名字来确定视图文件的位置的。采用这个命名约定的好处是在Action返回视图的时候会MVC框架会按照这个约定找到默认的视图文件。比如在ProductController的Action方法List最后是这样的代码：

return View();

会自动去路径，/Views/Product/找文件List.cshtml（或者List.aspx如果使用的老的视图引擎）。

当然也可以指定视图的名字：

return View("~/Views/Product/List.cshtml")

或者

return View("MyOtherView")

MVC框架在查找具体的默认视图文件时，如果在/Views/ControllerName/下面没有找到，会再在/Views/Shared下面找，如果都没找到就会找错：找不到视图。

## 三、ASP.NET MVC的URL规则说明

最开始我们在网站的Application_Start事件中注册一些路由规则routes.MapRoute，当有请求过来的时候，mvc框架会用这些路由规则去匹配，一旦找到了符合要求就去处理这个URL。例如有下面这个URL：

http://mysite.com/Admin/Index

URL可以分为几段，除去主机头和url查询参数，MVC框架是通过/来把URL分隔成几段的。上面的URl分为两段。如下图：

![](0.6885958744573943-20220831222539-38zvdni.png)​

第一段的值为Admin，第二段的值为Index，我们是很容易看出Admin对应就是Controller，Index就是Action。但是我们要告诉MVC框架这样的规则，因此为下面的Application_Start有下面的代码：

```
1 routes.MapRoute(
2                 name: "Default",
3                 url: "{controller}/{action}/{id}",
4                 defaults: new { controller = "Home", action = "Index", id = UrlParameter.Optional }
5 );
```

上面表示URL规则是：

{controller}/{action}

这个路由规则有两个段，第一个是controller，第二个是action。声明url段每个部分要且{}括起来，相当于占位符，是变量。

当一个URL请求到来的时候MVC路由系统就负责把它匹配到一个具体的路由规则，并把URL每段的值提取出来。这里说“一个具体的路由规则”，是因为可能会注册多个路由规则，MVC路由系统会根据注册顺序一个一个的查找匹配，直到到为止。

默认情况，URL路由规则只匹配与之有相同URL段数量的URL。如下表：

|**URL**|**URL段**|
| --------------------------------------| -------------------------------------|
|http://mysite.com/Admin/Index|controller = Adminaction = Index|
|http://mysite.com/Index/Admin|controller = Indexaction = Admin|
|http://mysite.com/Apples/Oranges|controller = Applesaction = Oranges|
|http://mysite.com/Admin|无匹配-段的数量不够|
|http://mysite.com/Admin/Index/Soccer|无匹配-段的数量超了|

## 四、mvc创建一个简单的Route规则

我们在前面注册路由规则都是通过下面的方式：

```
1 public static void RegisterRoutes(RouteCollection routes) {
2  
3     routes.MapRoute("MyRoute", "{controller}/{action}");
4 }
```

用到了RouteCollection的MapRoute方法。其实我们还可以调用 Add方法，传一个Route的实例给它一样的达到相同的效果。

```
1 public static void RegisterRoutes(RouteCollection routes) {
2  
3     Route myRoute = new Route("{controller}/{action}", new MvcRouteHandler());
4     routes.Add("MyRoute", myRoute);
5 }
```

## 五、mvc路由的默认值的设定

之前有说：URL路由规则只匹配与之有相同URL段数量的URL，这种是严格，但是我们又想有些段不用输入，让用户进入指定的页面。像，http://www.xx.com/Home/就是进入进入Home的Index。只需要设定mvc路由的默认值就可以了。

```
1 public static void RegisterRoutes(RouteCollection routes) {
2     routes.MapRoute("MyRoute", "{controller}/{action}", new { action = "Index" });  
3 }
```

要设置Controller和Action的默认值。

```
1 public static void RegisterRoutes(RouteCollection routes) {
2  
3 routes.MapRoute("MyRoute", "{controller}/{action}",
4         new { controller = "Home", action = "Index" });
5 } 
```

下面是一个具体的Url对应的Route映射。

|**Url段的数量**|**实例**|**Route映射**|
| ---| --------------------------------| -------------------------------------|
|0|mydomain.com|controller = Homeaction = Index|
|1|mydomain.com/Customer|controller = Customeraction = Index|
|2|mydomain.com/Customer/List|controller = Customeraction = List|
|3|mydomain.com/Customer/List/All|无匹配—Url段过多|

## 六、mvc使用静态URL段

前面定义路由规则都是占位符的形式，{controller}/{action}，我们也可以使用在使用静态字符串。如：

![复制代码](0.5716210968855451-20220831222539-e6tqmt0.png)[&quot;复制代码&quot;]("复制代码")

```
1 public static void RegisterRoutes(RouteCollection routes) {
2  
3     routes.MapRoute("MyRoute", "{controller}/{action}",
4         new { controller = "Home", action = "Index" });
5  
6     routes.MapRoute("", "Public/{controller}/{action}",
7        new { controller = "Home", action = "Index" });
8 }
```

![复制代码](0.017516743503534382-20220831222539-2hwoh0u.png)[&quot;复制代码&quot;]("复制代码")

上面匹配：http://mydomain.com/Public/Home/Index

路由："Public/{controller}/{action}"只匹配有三段的url,第一段必须为Public，第二和第三可以是任何值，分别用于controller和action。

除此这外，路由规则中可以既包含静态和变量的混合URL段，如：

![复制代码](0.1368764127483919-20220831222539-0od8omk.png)[&quot;复制代码&quot;]("复制代码")

```
 1 public static void RegisterRoutes(RouteCollection routes) {
 2  
 3     routes.MapRoute("", "X{controller}/{action}");
 4  
 5     routes.MapRoute("MyRoute", "{controller}/{action}",
 6         new { controller = "Home", action = "Index" });
 7  
 8     routes.MapRoute("", "Public/{controller}/{action}",
 9        new { controller = "Home", action = "Index" });
10  
11 } 
```

![复制代码](0.7951047116487555-20220831222539-mhjpoy6.png)[&quot;复制代码&quot;]("复制代码")

## 七、mvc的路由中自定义参数变量

mvc框架除了可以定义自带的controller和action的参数之外，还可以定义自带的变量。如下：

```
1 public static void RegisterRoutes(RouteCollection routes) {
2  
3     routes.MapRoute("MyRoute", "{controller}/{action}/{id}",
4         new { controller = "Home", action = "Index", id = "1" });
5 } 
```

上面定义了一个id，默认值我们设为1。

这个路由可以匹配0-3个url段的url，第三个url段将被用于id。如果没有对应的url段，将应用设置的的默认值。

自定义参数变量使用：

方法一、

```
1 public ViewResult CustomVariable() {
2  
3 ViewBag.CustomVariable = RouteData.Values["id"];
4     return View();
5 }
```

MVC框架从URL获取到变量的值都可以通过RouteData.Values["xx"]，这个集合访问。

方法二、

```
1 public ViewResult CustomVariable(int id) {
2  
3 ViewBag.CustomVariable = id;
4     return View();
5 }
```

MVC框架使用内置的Model绑定系统将从URL获取到变量的值转换成Action参数相应类型的值。这种转换除了可以转换成基本int，string等等之外还可以处理复杂类型，自定义的Model,List集合等。

## 八、mvc定义可选URL段、可选参数

asp.net mvc定义 参数是也可以设置为可选的，这样用户可以不用输入这部分的参数。

### 1、注册路由时定义可选URL段

public static void RegisterRoutes(RouteCollection routes) {

    routes.MapRoute("MyRoute", "{controller}/{action}/{id}",

        new { controller = "Home", action = "Index", id = UrlParameter.Optional });

}

### 2、通过Action参数来定义可选参数

public ViewResult CustomVariable(string id = "DefaultId") {

ViewBag.CustomVariable = id;

    return View();

}

通过Action参数来定义可选参数是没有加默认值的，而通过注册路由时定义可选URL段是加了默认值的，是利用c#参数的默认参数特性。这样如果用户没有输入这部分url段，就会默认值就会被使用。

## 九、mvc使用*来定义变长数量的URL段

除了在路由规则中声明固定的数量的URL段，我们也可以定义变长数量的URL段，如下面代码：

public static void RegisterRoutes(RouteCollection routes) {

    routes.MapRoute("MyRoute", "{controller}/{action}/{id}/{*catchall}",

        new { controller = "Home", action = "Index", id = UrlParameter.Optional });

}

通过变量前面加一个星号（*）开头就能匹配任意变长数量的URL。

匹配URL如下：

|**Url段的数量**|**实例**|**Route映射**|
| ---| --------------------------------------------| -------------------------------------------------------------------|
|0|mydomain.com|controller = Homeaction = Index|
|1|mydomain.com/Customer|controller = Customeraction = Index|
|2|mydomain.com/Customer/List|controller = Customeraction = List|
|3|mydomain.com/Customer/List/All|controller = Customeraction = Listid = All|
|4|mydomain.com/Customer/List/All/Delete|controller = Customeraction = Listid = Allcatchall = Delete|
|5|mydomain.com/Customer/List/All/Delete/Perm|controller = Customeraction = Listid = Allcatchall = Delete /Perm|

## 十、mvc使用命名空间来为路由的Controller类定优先级

当一个用户输入一个URL请求ASP.NET MVC的网站时，ASP.NET MVC会根据URL的获取请求是找到是哪一个Controller类，如果一个项目有多相同的类名的Controller，就会有问题。比如：当请求的变量controller的值为Home时，MVC框架就会去找一个Controller名字为HomeController的类，这个类（HomeController）默认是不受限制的，如果多个命名空间都有名字为HomeContoller的类， ASP.NET MVC就不知道怎么办了。当这种情况发生是，就会报错：

“/”应用程序中的服务器错误。

找到多个与名为“Home”的控制器匹配的类型。如果为此请求(“{controller}/{action}/{id}”)提供服务的路由没有指定命名空间以搜索与此请求相匹配的控制器，则会发生这种情况。如果是这样，请通过调用带有 'namespaces' 参数的 "MapRoute" 方法的重载来注册此路由。

“Home”请求找到下列匹配的控制器:

WebApplication1.Controllers.HomeController

WebApplication1.Controllers1.HomeController

[InvalidOperationException: 找到多个与名为“Home”的控制器匹配的类型。如果为此请求(“{controller}/{action}/{id}”)提供服务的路由没有指定命名空间以搜索与此请求相匹配的控制器，则会发生这种情况。如果是这样，请通过调用带有 'namespaces' 参数的 "MapRoute" 方法的重载来注册此路由。

“Home”请求找到下列匹配的控制器:

**解决办法：**

![复制代码](0.08692694659361933-20220831222539-24w2luh.png)[&quot;复制代码&quot;]("复制代码")

```
1 public static void RegisterRoutes(RouteCollection routes) {
2  
3     routes.MapRoute("Default",
4                 "{controller}/{action}/{id}",
5                  new { controller = "Home", action = "Index", id = UrlParameter.Optional },
6                  new string[] { "WebApplication1.Controllers" }
7             );
8 }
```

![复制代码](0.051182784744753906-20220831222539-7ac4u2n.png)[&quot;复制代码&quot;]("复制代码")

上面MapRoute的最后一个参数，new string[] { "WebApplication1.Controllers" }就是指定先去命名空间为WebApplication1.Controllers查找在controller，如果找到就停止往下找，没找到还是会去其它命名空间中去找的。因此当你指定的这个命名空间如果没存在要找的controller类，而在其它命名空间是有的，是会正常执行的，所以这里指定命名空间并不是限定了命名空间，而只是设了一个优先级而已。

## 十一、mvc定义路由规则的约束

在前面我们介绍了为mvc路由的规则设置路由默认值和可选参数，现在我们再深入一点，我们要约束一下路由规则。

### 1、用正则表达式限制asp.net mvc路由规则

![复制代码](0.13568002756299058-20220831222539-a5xaje8.png)[&quot;复制代码&quot;]("复制代码")

```
1 public static void RegisterRoutes(RouteCollection routes) {
2  
3      routes.MapRoute("MyRoute", "{controller}/{action}/{id}/{*catchall}",
4         new { controller = "Home", action = "Index", id = UrlParameter.Optional },
5         new { controller = "^H.*"},
6         new[] { "URLsAndRoutes.Controllers"});
7 }
```

![复制代码](0.25852756169316526-20220831222539-fkiomm5.png)[&quot;复制代码&quot;]("复制代码")

上面用到正则表达式来限制asp.net mvc路由规则，表示只匹配contorller名字以H开头的URL。

### 2、把asp.net mvc路由规则限制到到具体的值

![复制代码](0.5705650367154587-20220831222539-7ppk7x0.png)[&quot;复制代码&quot;]("复制代码")

```
1 public static void RegisterRoutes(RouteCollection routes) {  
2  
3     routes.MapRoute("MyRoute", "{controller}/{action}/{id}/{*catchall}",
4         new { controller = "Home", action = "Index", id = UrlParameter.Optional },
5         new { controller = "^H.*", action = "^Index$|^About$"},
6         new[] { "URLsAndRoutes.Controllers"});
7 } 
```

![复制代码](0.7030770297535325-20220831222539-smhuao9.png)[&quot;复制代码&quot;]("复制代码")

上例在controller和action上都定义了约束，约束是同时起作用是，也就是要同时满足。上面表示只匹配contorller名字以H开头的URL，且action变量的值为Index或者为About的URL。

### 3、把asp.net mvc路由规则限制到到提交请求方式（POST、GET）

![复制代码](0.5743344136985753-20220831222539-l4fg9uw.png)[&quot;复制代码&quot;]("复制代码")

```
1 public static void RegisterRoutes(RouteCollection routes) {
2  
3     routes.MapRoute("MyRoute", "{controller}/{action}/{id}/{*catchall}",
4         new { controller = "Home", action = "Index", id = UrlParameter.Optional },
5         new { controller = "^H.*", action = "Index|About",
6             httpMethod = new HttpMethodConstraint("GET") },
7         new[] { "URLsAndRoutes.Controllers" });
8 }
```

![复制代码](0.5146406111178352-20220831222539-b6x6zoi.png)[&quot;复制代码&quot;]("复制代码")

上面表示只匹配为GET方式的请求。

### 4、使用接口IRouteConstraint自定义一个asp.net mvc路由约束

下面我自定义一个约束对特定浏览器进行处理。

UserAgentConstraint.cs：

![复制代码](0.6783545259189334-20220831222539-azk7c5b.png)[&quot;复制代码&quot;]("复制代码")

```
 1 using System.Web;
 2 using System.Web.Routing;
 3  
 4 namespace URLsAndRoutes.Infrastructure {
 5  
 6     public class UserAgentConstraint : IRouteConstraint {
 7         private string requiredUserAgent;
 8  
 9         public UserAgentConstraint(string agentParam) {
10             requiredUserAgent = agentParam;
11         }
12  
13         public bool Match(HttpContextBase httpContext, Route route, string parameterName,
14                           RouteValueDictionary values, RouteDirection routeDirection) {
15  
16             return httpContext.Request.UserAgent != null &&
17                 httpContext.Request.UserAgent.Contains(requiredUserAgent);
18         }
19     }
20 } 
```

![复制代码](0.5057947746109068-20220831222539-mfjxzux.png)[&quot;复制代码&quot;]("复制代码")

asp.net mvc自定义路由约束的使用：

![复制代码](0.6375281236077512-20220831222539-yruiimh.png)[&quot;复制代码&quot;]("复制代码")

```
 1 public static void RegisterRoutes(RouteCollection routes) {
 2  
 3     routes.MapRoute("MyRoute", "{controller}/{action}/{id}/{*catchall}",
 4         new { controller = "Home", action = "Index", id = UrlParameter.Optional },
 5         new {
 6             controller = "^H.*", action = "Index|About",
 7             httpMethod = new HttpMethodConstraint("GET", "POST"),
 8             customConstraint = new UserAgentConstraint("IE")
 9         },  
10         new[] { "URLsAndRoutes.Controllers" });
11 } 
```

![复制代码](0.2237526758242081-20220831222539-b9y4057.png)[&quot;复制代码&quot;]("复制代码")

上面表示这个路由规则只匹配用户使用IE浏览器的请求。利用这点我们就可以实现不同浏览器使用不同的Controller，进行不同的处理。虽然这样做的意义不大，但是不排除有时会有这种变态的需求。

## 十二、mvc将URL路由到磁盘文件

mvc的网站并不是所以的url请求都是对应controller，action，我们仍然要一种方式来提供一些静态内容，比如：html文件，css,图片,javascript文件些，其实默认情况下mvc框架在在收到url请求时会先判断这个url是否是对应一个磁盘中真实存在的文件，如果是直接返回，这时路由是没有使用到的，如果不是真实存在的文件时才会走路由系统，再去匹配注册的路由规则。

这种默认的处理url机制顺序我们也可以改变它，让在检查物理文件之前就应用路由，如下：

![复制代码](0.05237258769397801-20220831222539-6ux2nw3.png)[&quot;复制代码&quot;]("复制代码")

```
 1 public static void RegisterRoutes(RouteCollection routes) {
 2  
 3     routes.RouteExistingFiles = true;
 4  
 5     routes.MapRoute("DiskFile", "Content/StaticContent.html",
 6         new {
 7             controller = "Account", action = "LogOn",
 8         },
 9         new {
10             customConstraint = new UserAgentConstraint("IE")
11         });
12  
13     routes.MapRoute("MyRoute", "{controller}/{action}/{id}/{*catchall}",
14         new { controller = "Home", action = "Index", id = UrlParameter.Optional },
15         new {
16             controller = "^H.*", action = "Index|About",
17             httpMethod = new HttpMethodConstraint("GET", "POST"),
18             customConstraint = new UserAgentConstraint("IE")
19         },
20         new[] { "URLsAndRoutes.Controllers" });
21 }
```

![复制代码](0.31125528023989046-20220831222539-j46f050.png)[&quot;复制代码&quot;]("复制代码")

我们把RouteExistingFiles属性设置为true，表示存在的文件也走路由，上面我们把Content/StaticContent.html这个文件映射到controller 为Account，action 为LogOn中了，而并不是指磁盘中存在的文件。基于asp.net mvc的这个特性我们就可以实现mvc以.html结尾的伪静态

## 十三、mvc跳过、绕开路由系统设定

上面我们用使用routes.RouteExistingFiles = true，让所有的请求都走路由系统过一下，难免有一些性能影响，因为一些图片，一些真正的html，文件是没有必要的。我们可以对些文件做一些起特殊设定让它们跳过、绕开路由系统。下面就是让Content目录下的所有文件都绕开mvc的路由系统：

![复制代码](0.38484642289572735-20220831222539-zf6h302.png)[&quot;复制代码&quot;]("复制代码")

```
 1 public static void RegisterRoutes(RouteCollection routes) {
 2  
 3     routes.RouteExistingFiles = true;
 4  
 5     routes.MapRoute("DiskFile", "Content1/StaticContent.html",
 6         new {
 7             controller = "Account", action = "LogOn",
 8         },
 9         new {
10             customConstraint = new UserAgentConstraint("IE")
11         });
12  
13     routes.IgnoreRoute("Content/*{filename}");
14      routes.MapRoute("", "{controller}/{action}");  
15     
16 }
```

![复制代码](0.5112390281958707-20220831222539-y5nkskh.png)[&quot;复制代码&quot;]("复制代码")

来源： [https://www.cnblogs.com/itliuyang/p/6872027.html](https://www.cnblogs.com/itliuyang/p/6872027.html)
