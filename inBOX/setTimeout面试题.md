# setTimeout面试题

先看题：

![复制代码](0.7026345424819738-20220206093630-w6nrhay.png)[&quot;复制代码&quot;]("复制代码")

```js
1 for (var i = 0; i < 3; i++) {
2     setTimeout(function() {
3         console.log(i);
4     }, 0);
5     console.log(i);
6 }
```

![复制代码](0.31475210515782237-20220206093630-xntzmum.png)[&quot;复制代码&quot;]("复制代码")

结果是：0 1 2 3 3 3  
很多公司面试都爱出这道题，此题考察的知识点还是蛮多的。 都考察了那些知识点呢？  
**异步、作用域、闭包。**  
我们来简化此题：

```
1 setTimeout(function() {
2         console.log(1);
3 }, 0);
4 console.log(2);   //先打印2，再打印1
```

因为是setTimeout是异步的。  
正确的理解setTimeout的方式(注册事件)： 有两个参数，第一个参数是函数，第二参数是时间值。 调用setTimeout时，把函数参数，放到事件队列中。等主程序运行完，再调用。

就像我们给按钮绑定事件一样：

```
1 btn.onclick = function() {
2         alert(1);
3 };
```

这么写完，会弹出1吗。不会！！只是绑定事件而已！ 必须等我们去触发事件，比如去点击这个按钮，才会弹出1。

setTimeout也是这样的！只是绑定事件，等主程序运行完毕后，再去调用。

setTimeout的时间值是怎么回事呢？

```
1 setTimeout(fn, 2000)
```

程序会不会报错？ 不会！而且还会准确得打印1。为什么？ 因为真正去执行console.log(i)这句代码时，var i = 1已经执行完毕了！

所以我们进行dom操作。可以先绑定事件，然后再去写其他逻辑。

![复制代码](0.010356518905609846-20220206093630-2qepggg.png)[&quot;复制代码&quot;]("复制代码")

```
1 window.onload = function() {
2         fn();
3 }
4 var fn = function() {
5         alert('hello')
6 };
```

![复制代码](0.6152239451184869-20220206093630-d97uzu3.png)[&quot;复制代码&quot;]("复制代码")

这么写，完全是可以的。因为异步！

es5中是没有块级作用域的。

```
1 for (var i = 0; i < 3; i++) {}
2 console.log(i); //3，也就说i可以在for循环体外访问到。所以是没有块级作用域。
```

这回我们再来看看原题。  
原题使用了for循环。循环的本质是干嘛的？ 是为了方便我们程序员，少写重复代码。

原题等价于：

![复制代码](0.4035169668495655-20220206093630-sar5ic3.png)[&quot;复制代码&quot;]("复制代码")

```
 1 var i = 0;
 2 setTimeout(function() {
 3     console.log(i);
 4 }, 0);
 5 console.log(i);
 6 i++;
 7 setTimeout(function() {
 8     console.log(i);
 9 }, 0);
10 console.log(i);
11 i++;
12 setTimeout(function() {
13     console.log(i);
14 }, 0);
15 console.log(i);
16 i++;
```

![复制代码](0.33588275383226573-20220206093630-d7mj2tt.png)[&quot;复制代码&quot;]("复制代码")

因为setTimeout是注册事件。根据前面的讨论，可以都放在后面。  
原题又等价于如下的写法：

![复制代码](0.9636759136337787-20220206093630-ip7p3ej.png)[&quot;复制代码&quot;]("复制代码")

```
 1 var i = 0;
 2 console.log(i);
 3 i++;
 4 console.log(i);
 5 i++;
 6 console.log(i);
 7 i++;
 8 setTimeout(function() {
 9     console.log(i);
10 }, 0);
11 setTimeout(function() {
12     console.log(i);
13 }, 0);
14 setTimeout(function() {
15     console.log(i);
16 }, 0);  //弹出 0 1 2 3 3 3
```

![复制代码](0.04128778027370572-20220206093630-q7qcql8.png)[&quot;复制代码&quot;]("复制代码")

怎么保证能弹出0，1， 2呢？

![复制代码](0.4538141442462802-20220206093630-sa9a4a0.png)[&quot;复制代码&quot;]("复制代码")

```
1 for (var i = 0; i < 3; i++) {
2     setTimeout((function(i) {
3         return function() {
4             console.log(i);
5         };
6     })(i), 0);  //改为立即执行的函数7     console.log(i);  
8 }
```

![复制代码](0.2353273939806968-20220206093630-sy7dq1c.png)[&quot;复制代码&quot;]("复制代码")
