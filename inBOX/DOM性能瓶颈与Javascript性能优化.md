# DOM性能瓶颈与Javascript性能优化

 这两天比较闲，写了两篇关于JS性能缺陷与解决方案的文章（《[JS特性性能缺陷及JIT的解决方案](http://www.cnblogs.com/hyddd/archive/2013/02/06/2908110.html)》，《[Javascript垃圾回收浅析](http://www.cnblogs.com/hyddd/archive/2013/02/07/2908598.html)》），主要描述了untyped，GC带来的问题与JIT引擎的解决方案。但相对于Js引擎的问题，我认为DOM导致的性能问题更值得关注。

**一.Dom的性能瓶颈及原因**

1. 为什么是DOM

    标准的xml/html的文本解析协议，常见的有DOM与SAX。在解析速度及内存占用上，SAX比DOM有优势，但为什么浏览器选择DOM解析html？

    （1）DOM VS SAX

    SAX提供一次性解析文本，不生成对象，Iterator模式访问元素，event-based，PUSH模式触发，简单说：App需要向Parser注册，当Parser遍历xml时，触发调用APP 。想深入体验，用下javax.xml.parsers.SAXParser。这里说个题外话，改进版StAX是PULL模式，但这都不重要了，重要是：一次性文本解析，不生成对象。

    DOM解析文本后，生成DOM树。即：一次性文本解析，生成对象。

    （2）浏览器选择了DOM

    单次效率DOM不如SAX，但SAX不生成对象，浏览器很多操作很难满足，比如：元素定位，元素样式渲染……所以DOM是必然之选。
2. DOM的性能问题

    【1】核心问题

    当解析的html文件很大时，生成DOM树占用内存较大，同时遍历（不更新）元素耗时也更长。但这都不是重点，DOM的核心问题是： **DOM修改导致的页面重绘、重新排版** ！重新排版是用户阻塞的操作，同时，如果频繁重排，CPU使用率也会猛涨！

    **DOM操作会导致一系列的重绘（repaint）、重新排版（reflow）操作。为了确保执行结果的准确性，所有的修改操作是按顺序同步执行的。大部分浏览器都不会在JavaScript的执行过程中更新DOM。相应的，这些浏览器将对对 DOM的操作放进一个队列，并在JavaScript脚本执行完毕以后按顺序一次执行完毕。也就是说，在JavaScript执行的过程，直到发生重新排版，用户一直被阻塞。**

 **    一般的浏览器中（不含IE），repaint的速度远快于reflow，所以避免reflow更重要** 。

     **导致repaint、reflow的操作** ：

* DOM元素的添加、修改（内容）、删除( Reflow + Repaint)*
* 仅修改DOM元素的字体颜色（只有Repaint，因为不需要调整布局）*
* 应用新的样式或者修改任何影响元素外观的属性*
* Resize浏览器窗口、滚动页面*
* 读取元素的某些属性（offsetLeft、offsetTop、offsetHeight、offsetWidth、scrollTop/Left/Width/Height、clientTop/Left/Width/Height、getComputedStyle()、currentStyle(in IE))*

    【2】其他

    某些Javascript框架中，CSS选择器，如：var el = $('.hyddd');由于IE6、7不支持，所以Javascript框架必须通过遍历整个DOM树来寻找对象。

**二. 针对DOM问题，Javascript的应对方案**

1. 针对repaint、reflow，Nicholas 大叔在他的《[Speed up your JavaScript, Part 4](http://www.nczonline.net/blog/2009/02/03/speed-up-your-javascript-part-4/)》做了详细介绍，这里我也整理一下：

    **解决问题的关键是：减少因DOM操作，引起的reflow。**Nicholas总结了一些方法：

    【1】在DOM外，执行尽量多的变更操作。Demo：

‍

```
// 不好的做法
for (var i=0; i < items.length; i++){
    var item = document.createElement("li");
    item.appendChild(document.createTextNode("Option " + i);
    list.appendChild(item);
}   
```

```
// 更好的做法
// 使用容器存放临时变更， 最后再一次性更新DOM
var fragment = document.createDocumentFragment();
for (var i=0; i < items.length; i++){
    var item = document.createElement("li");
    item.appendChild(document.createTextNode("Option " + i);
    fragment.appendChild(item);
}
list.appendChild(fragment);
```

     【2】操作DOM前，先把DOM节点删除或隐藏，因为隐藏的节点不会触发重排。Demo如下：

```
list.style.display = "none";  
for (var i=0; i < items.length; i++){  
    var item = document.createElement("li");  
    item.appendChild(document.createTextNode("Option " + i);  
    list.appendChild(item);  
}  
list.style.display = "";  
```

    【3】一次性，修改样式属性。Demo如下：

```
// 不好的做法
// 这种做法会触发多次重排
element.style.backgroundColor = "blue";  
element.style.color = "red";  
element.style.fontSize = "12em";
```

```
// 更好的做法是，把样式都放在一个class下
.newStyle {  
    background-color: blue;  
    color: red;  
    font-size: 12em;  
}  
element.className = "newStyle";
```

    【4】使用缓存，缓存临时节点。

```
// 不好的做法
document.getElementById("myDiv").style.left = document.getElementById("myDiv").offsetLeft +  
document.getElementById("myDiv").offsetWidth + "px";  
```

```
// 更好的做法
var myDiv = document.getElementById("myDiv");  
myDiv.style.left = myDiv.offsetLeft + myDiv.offsetWidth + "px";  
```

2. 针选择其的问题，我只能说：没办法…… : (

**三. 参考资源**

1.《[Web 2.0应用客户端性能问题十大根源](http://www.infoq.com/cn/news/2010/08/web-performance-root/)》

2. 《[Speed up your JavaScript, Part 4](http://www.nczonline.net/blog/2009/02/03/speed-up-your-javascript-part-4/)》
