# JavaScript动态加载

JavaScript动态加载(JavaScript Object Dynamic Loading) - 之所以叫做动态，是应为其有别与通常的静态加载形式。

典型的JavaScript静态加载方式，是通过<script>标签将我们可能需要的所有JS文件依次嵌入到一个HTML页面中，当 浏览器执行到<script> 标签，就会到我们指定的地方去加载JavaScript并运行，这时，文件中定义的无论方法、类、对象等，已经存在与浏览器，等待被使用。除非HTML页 面被Unload，否则这些东西就一直存在。

而典型的动态加载方式，是不需要任何提前的准备，只有当需要一个JavaScript对象来为我们服务时，我们就临时去加载它所属的JS文件，然后使用它，使用完毕，销毁即可。正所谓“呼之即来，挥之即去”。

就因为这“呼之即来，挥之即去”的能力，使得JavaScript真正的活了起来。

JS动态加载解决了什么问题

给系统减了肥：可以将不必要的JavaScript代码延迟加载，未用到的功能不加载。  
模块化得以实现：可以将原本大块的功能或类，分解成合理的小块，便于管理与维护。  
方便的值传递：双向传值，无拘束。基于JSON，可以传递大对象。  
可以实现Service:可以基于Service理念,通过核心JavaScript对象使用其他的服务功能对象,完成业务的组装。  
JS动态加载应该有什么能力

可以异步的，或同步的，随心所遇加载外部JS文件。  
加载进来的JS文件的内容可以有效管理，包括作用域，开放性，生命周期。  
可以支持面向对象，以对象为基本点而不是方法或变量等。  
有好的封装，可以通过一个句柄操作整个被加载的JS。  
传统JS动态加载的实现

关于JS动态加载的实现，我见过很多种方法。早期的时候，人们想过很多办法。这些办法有自己发挥的空间，可以解决一些问题。但是都或多或少的存在弊端，以及实现上的繁琐，最大的问题就是，它们基本都不是面向对象的。下面就列举主要的2类：

用嵌套iFrame的方式来加载JS文件:这种方式早期最多，通过在页面中包含一个iFrame，来调用一个另一个包含了<script>标签的HTML页面，实现JS的动态加载。最大的弊端就是无法传递参数，实现较繁琐。  
临时创建<script>标签:这种方式有很多人用。利用JavaScript操作页面Dom模型，临时创建一 个<script>标签，然后appendChild到<body>上，或者动态改变已存在<scirpt>标签的 src属性。弊端是只能同步加载，必须等待JS文件被读取完毕，才能进行下一步工作。且传值是通过调用被加载的JS内的变量实现，双向传值困难，作用域弱 化。  
可以看出以上两种常见方式，都无法满足我们的要求。

更优秀的解决方案

得益于神奇如阿拉丁神灯般的 eval() ,我们就可以实现以上所有的愿望 :) 。

绕了那么大圈子，想了那么多办法。其实最简单的方法就在眼皮底下.

第一部分-调用方 main.js文件：

//动态读取JS对象的方法

[//@FileName]() :要读取的JS对象URL,可以是本地路径。

[//@TheParameters]() :需要传递给被加载对象的参数，可以是任何对象。

function loadJSObj(FileName,TheParameters){  
    Ext.Ajax.request({   //异步调用方法  
        url: FileName,   //调用URL  
        scope: this,<br />        success: function(response){   //成功后的回调方法response为返回内容  
            var remoteObj = eval(response.responseText);  
            var mod = null;  
            mod = new remoteObj(TheParameters);

            return mod;  
        },  
        failure:function(){   //失败后的回调方法  
            alert(name+' - 读取失败，请检查网络或文件。');

            return null;  
        }  
    });  
}

这段代码使用Extjs类库封装的异步读取方法Ext.Ajax.request()。只是为了方便。如果你使用未封装的XMLHttpRequest对象也没什么问题。

先大概说下loadJSObj方法干的事情是：

用XMLHttpRequest来了一次Ajax请求，将想要加载的JS文件源代码读过来。  
利用eval函数将刚才读到的远程JavaScript类实例化成为本地的对象（期间传递了构造参数）。  
返回这个对象，供使用。  
此处关键的代码在于

            var remoteObj = eval(response.responseText);  
            var mod = null;  
            mod = new remoteObj(TheParameters);

            return mod;

可以看到，我们直接将被读取的JS文件传进eval 函数执行。并返回一个叫做 remoteObj的东西，这个东西其实就是被读取的JS的句柄（一个类，定义在被读取JS中的类，后面会详细说到），通过将remoteObj实例化， 即mod = new remoteObj(参数) 就可以通过 mod 对象对被读取的JS随心所欲了操作了。

如此一来，远程的JS就被我们按照参数中指定的要求实例化成本地对象了，可以使用了。但是…这只是一相情愿。因为被读取的JS得符合我们的要求，才能“两厢情愿”，最终得到结果…

第二部分 - 被调用方 login.js文件：

要想符合要求，被读取的JS文件也必须按照一定的规则来写。

//用户登录类

Ext.extend(eueuy.module,{  
    init:function(){<br />        //==Variables==//  
        var userName = this.parameters.un;  //获取传递进来的参数un  
        var passWord = this.parameters.pwd;   //获取传递进来的参数pwd

        //==Methods==//  
        //登录方法  
        function loginOn(){  
            if (userName=='eueuy' || passWord=='123'){<br />                return true;<br />            }else{  
                return false;  
            }  
        }  
        //登出方法<br />        function loginOff(){<br />            alert('欢迎再来');

        }  
    }  
});

我们先看看这个登录类干了什么：

继承自eueuy.moudle，并重写了构造函数。  
接收了2个传递进来的初始化参数，un 和 pwd。  
定义了2个成员方法和一个成员变量。  
没有任何特别的东西，就是Ext.extend方法用的有点怪,区别于平常的 XXX = Ext.extend();

没有赋值符号和类名。这恰恰是一个关键点：

类的名字必须留空

为什么这样写？因为这样一来，这个没有类名的类， 就可以在eval()函数执行他的时候，再给他定义类名，这样就将类名的定义留在了调用的时候，也就是main.js文件中。这个小技巧也将使调用方(main.js)可以直接控制被调用方(login.js)的类。

最后，就可以在main.js中用下面的代码，通过动态加载用户登录类，进行用户登录验证的工作：

var loginModel = loadJSObj('http://www.eueuy.name/project1/js/login.js',{un:’eueuy’,pwd:’123’});

if (loginModel.loginOn()){

    alert('登录成功,欢迎'+loginModel.userName);

}else{

    alert('登录失败，错误的用户名密码');

}

可以看到，在上面的代码中，我们通过loginModel这个变量，控制了login.js中的登录类，可以访问其中的方法loginOn() ,还可以访问其中的变量 userName。[第三方物流](http://expert.cgfreight.cn/question_rank__1_76_5_1.htm)

结尾

至此，对象动态加载技术的理论已经讲解的差不多了，再下一篇博文中，我还会依据这些理论，给大家贴出一个基于动态加载的的项目框架。这个框架可以方便的实现One-Page，还可以看到一些具体的应用。
