# JavaScript异步加载与同步加载

**关于同步加载与异步加载的区别**

同步加载：同步模式，又称阻塞模式，会阻止浏览器的后续处理，停止了后续的解析，因此停止了后续的文件加载（如图像）、渲染、代码执行。

异步加载：异步加载又叫非阻塞，浏览器在下载执行 js 同时，还会继续进行后续页面的处理。

**为何使用异步加载原因：**

优化脚本文件的加载提高页面的加载速度，一直是提高页面加载速度很重要的一条。因为涉及到各个浏览器对解析脚本文件的不同机制，以及加载脚本会阻塞其他资源和文件的加载，因此更多的采用异步加载。

**使用异步加载的方式：**

1.动态生成script标签

加载方式在加载执行完之前会阻止 onload 事件的触发，而现在很多页面的代码都在 onload 时还要执行额外的渲染工作等，所以还是会阻塞部分页面的初始化处理

![复制代码](0.8137540294103764-20220205161253-7xnwcbr.png)[&quot;复制代码&quot;]("复制代码")

```
<!DOCTYPE html>
<html>
<head>
    <title>阻止onload事件的触发</title>
</head>
<body>
    this is a test
    <script type="text/javascript">
        ~function() {
            //function async_load(){
                var s = document.createElement('script');
                s.src = 'http://china-addthis.googlecode.com/svn/trunk/addthis.js';
                document.body.appendChild(s);
            //}
            //window.addEventListener('load',async_load,false);
        }();
        // window.onload = function() {
           //  var txt = document.createTextNode(' hello world');
           //  document.body.appendChild(txt);
        //  };
    </script>
    <script type="text/javascript" src='http://libs.baidu.com/jquery/2.0.0/jquery.min.js'>  
    </script>
</body>
</html>
```

![复制代码](0.011939543065804914-20220205161253-sy1pxsi.png)[&quot;复制代码&quot;]("复制代码")

2.解决了阻塞 onload 事件触发的问题

![复制代码](0.6208098328815344-20220205161253-qguvdiz.png)[&quot;复制代码&quot;]("复制代码")

```
<!DOCTYPE html>
<html>
<head>
    <title>解决了阻塞onload事件触发的问题</title>
</head>
<body>
    this is a test
    <script type="text/javascript">
        ~function() {
            function async_load(){
                var s = document.createElement('script');
                s.src = 'http://china-addthis.googlecode.com/svn/trunk/addthis.js';
                document.body.appendChild(s);
            }
            window.addEventListener('load',async_load,false);
        }();
        window.onload = function() {
            var txt = document.createTextNode(' hello world');
            document.body.appendChild(txt);
        };
    </script>
    <script type="text/javascript" src='http://libs.baidu.com/jquery/2.0.0/jquery.min.js'>  
    </script>
</body>
</html>
```

![复制代码](0.7017438573533452-20220205161253-jc5mf0w.png)[&quot;复制代码&quot;]("复制代码")

3.在支持async属性的浏览器

![复制代码](0.17186219275231118-20220205161253-wuufoho.png)[&quot;复制代码&quot;]("复制代码")

```
<!DOCTYPE html>
<html>
<head>
    <script type="text/javascript" async src='http://china-addthis.googlecode.com/svn/trunk/addthis.js'></script>
    <script type="text/javascript" async src='http://libs.baidu.com/jquery/2.0.0/jquery.min.js'></script>
    <title>使用async属性</title>
</head>
<body>
    this is a test
</body>
</html>
```

![复制代码](0.3313061379290707-20220205161253-zc0bkqg.png)[&quot;复制代码&quot;]("复制代码")

**补充：**

DOMContentLoaded : 页面(document)已经解析完成，页面中的dom元素已经可用。但是页面中引用的图片、subframe可能还没有加载完。

OnLoad：页面的所有资源都加载完毕（包括图片）。浏览器的载入进度在这时才停止。

**图解执行过程：**

![](0.5833526603652588-20220205161253-lxum0sj.png)

蓝色线代表网络读取，红色线代表执行时间，这俩都是针对脚本的；绿色线代表 HTML 解析。
