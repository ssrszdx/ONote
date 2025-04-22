# Ajax的工作原理以及优点、缺点 （汇总）

**最近空闲时间，有朋友问我关于Ajax的工作原理，在这里我结合自己的工作经验和网上大佬的经验做一个总结，如有不足，请各位业内大佬指正**

**在我们了解Ajax之前，我们先来了解一下Javascript的执行原理：**

Javascript语言的执行环境是"单线程"（single thread），所谓的[**“单线程”**]()（参考https://blog.csdn.net/baidu_24024601/article/details/51861792）也就是说，就是指一次只能完成一件任务。如果有多个任务，就必须排队，前面一个任务完成，再执行后面一个任务，以此类推。了解了Javascript的执行原理以后，我们再来看看Ajax。

**1、什么是**​****​**？**

AJAX全称为“Asynchronous JavaScript and XML”（异步JavaScript和XML），是一种创建交互式网页应用的网页开发技术，通过在后台与服务器进行少量数据交换，AJAX 可以使网页实现异步更新。这意味着可以在不重新加载整个网页的情况下，对网页的某部分进行更新。

**2、为什么要使用**​****​**?**

 **这儿我们可以说到Ajax的优点之一：** 它可以在不刷新整个页面的情况下与服务器通信保持原有页面状态，说的简单易懂一点。举个例子：在我们浏览网页的时候会有两种情况

1.点击链接，页面白屏，等待跳转到另一个页面。

2.页面不刷新，局部出现新内容获得更好的用户体验。

**3、工作原理：**

我们先通过一张图片来大致的了解一下Ajax向服务器请求数据的过程。

![](0.6452411253818695-20220831215650-dxqm1bb.png)​

有人会问，图片中的[**“XHR”**]()是什么东东，别急，我们慢慢来

所谓的 **“XHR”** （浏览器内置对象”XMLHttpRequest ”），也就是Ajax功能实现所依赖的对象，AJAX就是通过浏览器的内置对象XHMHttpResquest来发送异步请求的，异步请求不会妨碍客户端的任何操作。

**异步：**

XHR相当于是一个通信兵，来负责客户端与服务器之间的通信传输。举个形象生动的例子：

要打仗了， **前方阵地** （客服端）不可能只等着 **通信兵** （XHR）传递消息其他什么也不干吧，所以前方阵地还在干着自己的事情然后派通信兵去请求 **后方指挥部** （服务器）的命令，指挥部下达命令指挥，通信兵再把命令传到前方阵地，然后前方阵地再执行命令相关的操作（客户端把数据渲染到页面），这也就是Ajax的异步原理。

**再来说说同步：**

所谓的同步就是前方阵地和通信兵一起去向服务器请求数据，直到通信兵请求到数据，我才开始渲染页面，在请求的过程中页面一直是白屏等待的。

**AJAX既然是通过浏览器的内置对象XMLHttpRequest来处理异步请求的那我们先来了解下他又哪些方法和属性：**

注：写在这里的为必选参数或者经常用到的可选参数

**方法：**

**一、open();**

解释：发送请求的页面在不刷新的情况能将参数传给一个服务器进行处理, 这个方法就是将这些个参数传送过去

**参数:**

1, method:用于指定请求的类型  "GET"或者"POST"

2, url:用于请求的地址, 可相对可绝对

3, asyncFlag:指定请求方式为同步还是异步, true为异步, false为同步

**二、send();**

解释：这个东西就是将一些参数以键值对的方式传送给服务器, 异步的话将立即返回服务器的响应, 做到不刷新页面进行数据处理就是用来发送参数的, GET方法下可以在url的后面写上参数的值, POST方法下只能在send()方法里面写上参数的键值对

**三、setRequestHeader(&quot;header&quot;,&quot;value&quot;)**

解释：用于为请求的Http头设置值

**四, getResponseHeader(&quot;headerLabel&quot;);**

解释: 返回设置的Http头信息

**五, abort();**

个人理解: 使用了这个请求之后会直接停止getResult的回调函数, 让readyState属性的返回值直接为0

**六, getAllResponseHeaders();**

解释：以字符串的形式返回完整的字符串信息

**属性：**

**一, onreadystatechange**

解释: 用于指定状态改变时所触发的事件处理器（在设置回调函数的时候经常用到, 所有的状态改变的时候都会触发这个事件处理器）

**二, readyState**

解释: 用于获取请求的状态（ 通过返回的代码是多少来判断当前的状态是什么情况）

返回值有：0: 未初始化; 1: 正在加载; 2:已加载; 3:交互中; 4:完成

**三, responseText**

书上解释: 获取服务器的响应, 表示为字符串（response.getWrite().append("");将这个语句的内容返回到用户页面）

**四, responseXML**

解释: 用于获取服务器的响应, 表示为字符串

**五, status**

返回Http状态码——200:表示成功; 202:表示请求被接受, 但尚未成功; 400:错误的请求; 404:文件未找到; 500:内部服务器错误

**六, statusText**

返回Http状态码的文本信息

下面让我们通过一段代码实例来了解一下Ajax的使用方法：

![复制代码](0.9908380676739699-20220831215650-etrfccw.png)[&quot;复制代码&quot;]("复制代码")

```
 1 /**
 2  * ajax参数：
 3  * ajax({
 4  * method:post,get,
 5  * url,
 6  * param:参数，
 7  * callback:回调函数
 8  * })
 9  * 
10  */
11 var test = (function () {       //函数自执行    test存放返回的值，作为全局变量
12 
13     // ajax创建兼容封装
14     function getXlmHttp(obj) {
15         var xmlHttp;
16         if (window.XMLHttpRequest) { //兼容DOM
17             xmlHttp = new XMLHttpRequest();
18         } else if (window.ActiveXObject) { //兼容IE
19             xmlHttp = new ActiveXObject("Microsoft.XMLHTTP");
20         }
21         return xmlHttp;
22 
23     }
24 
25     // ajax get请求
26     function getAjax(obj) {
27         var xmlHttp = getXlmHttp();
28         xmlHttp.onreadystatechange = function () {
29             if (xmlHttp.readyState == 4 && xmlHttp.status == 200) {
30                 console.log(xmlHttp.responseText);
31                 obj.callback(xmlHttp);
32             }
33         }
34         // var obj = { "username": '张三', "password": "123" }
35         // 取对象的值有两种方式，1、"."" 2、"[]"
36         // 运算符要求是标识符，不能使变量，
37         var str;
38         for (key in obj.param) {
39             str = "key" + "=" + obj.param[key] + "&";
40         }
41         if (str.length > 0) {
42             str.url = obj.url + "?" + str;
43         }
44         xmlHttp.open(obj.method, obj.url)
45         xmlHttp.send(null)
46     }
47 
48     // ajax post请求
49     // var obj = { "username": '张三', "password": "123" }
50     function postAjax(obj) {
51         var xmlHttp = getXlmHttp();
52         xmlHttp.onreadystatechange = function () {
53             if (xmlHttp.readyState == 4 && xmlHttp.status == 200) {
54                 console.log(xmlHttp.responseText);
55                 obj.callback(xmlHttp);
56             }
57         }
58         var str;
59         for (key in obj.param) {
60             str = "key" + "=" + obj.param[key] + "&";
61         }
62         if (str.length > 0) {
63             str.url = obj.url + "?" + str;
64         }
65         xmlHttp.open(obj.method, obj.url);
66         xmlHttp.setRequestHeader("Content-type", "application/x-www-form-urlencoded");
67         xmlHttp.send(str);
68     }
69 
70     // 调用请求方式封装
71     function ajax(obj) {
72         if (obj.method = "post") {
73             postAjax(obj);
74         }
75         else if (obj.method = "get") {
76             getAjax(obj)
77         }
78     }
79 
80     // ajax Json封装
81     function getJson(obj) {
82         obj.callback = function (xmlHttp) {
83             var obj = JSON.parse(xmlHttp.responseText);
84             obj.callback(obj);
85         }
86         getAjax(obj);
87     }
88     return { "getAjax": getAjax, "postAjax": postAjax, "getJson": getJson, "ajax": ajax }    //返回值
89 })();
90 
91 test.getAjax() //调用封装的函数
```

![复制代码](0.4697444317064685-20220831215650-m4tftze.png)[&quot;复制代码&quot;]("复制代码")

**优点：**

1.最大的优点就是页面无需刷新，在页面内与服务器通信，非常好的用户体验。  
2.使用异步的方式与服务器通信，不需要中断操作。  
3.可以把以前服务器负担的工作转嫁给客户端，减轻服务器和带宽，可以最大程度减少冗余请求。  
4.基于标准化的并被广泛支持的技术，不需要下载插件或者小程序。

**缺点：**

1.AJAX干掉了Back和History功能，即对浏览器机制的破坏。  
在动态更新页面的情况下，用户无法回到前一个页面状态，因为浏览器仅能记忆历史记录中的静态页面。一个被完整读入的页面与一个已经被动态修改过的页面之间的差别非常微妙；用户通常会希望单击后退按钮能够取消他们的前一次操作，但是在Ajax应用程序中，这将无法实现。  
2.安全问题技术同时也对IT企业带来了新的安全威胁，ajax技术就如同对企业数据建立了一个直接通道。这使得开发者在不经意间会暴露比以前更多的数据和服务器逻辑。ajax的逻辑可以对客户端的安全扫描技术隐藏起来，允许黑客从远端服务器上建立新的攻击。还有ajax也难以避免一些已知的安全弱点，诸如跨站点脚步攻击、SQL注入攻击和基于credentials的安全漏洞等。  
3.对搜索引擎的支持比较弱。如果使用不当，AJAX会增大网络数据的流量，从而降低整个系统的性能。

**应用场景：**

场景 1. 数据验证

场景 2. 按需取数据

场景 3. 自动更新页面

来源： [https://www.cnblogs.com/weichao1996/p/9079028.html](https://www.cnblogs.com/weichao1996/p/9079028.html)
