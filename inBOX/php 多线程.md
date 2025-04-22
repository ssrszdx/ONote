# php 多线程

1. [php](https://www.isolves.com/it/cxkf/yy/php/) 与 多线程

php的多线程，对于phper是一个较冷门的知识。相信很多工作了很多年的[程序员](https://www.isolves.com/it/cxkf/cxy/)，没用过php多线程的大有人在。所以可以认为php是单线程。

![php为什么没有多线程？](http://www.isolves.com/d/file/p/2019/10-08/c7b069d726496a0233b8743033bde124.jpg)多线程示意

2. php是单线程，多进程模型

多线程有它的优点，

a.可以充分利用cpu，

b.调度的系统开销比进程小，

c.多线程共享[内存](https://www.isolves.com/it/yj/nc/)空间，不需要额外的IPC通信。

但多线程可能会引入其他问题，如数据同步，资源竞争，死锁等，处理不好容易出问题；

多进程也可以使用cpu多核，不一定非要多线程不可，PHP-FPM就是一个C实现的多进程FastCGI服务

对一个请求来说PHP是单线程的，但是多个请求间是并发的，像[Apache](https://www.isolves.com/e/tags/?tagname=Apache)，[Nginx](https://www.isolves.com/e/tags/?tagname=Nginx)都是多线程处理用户请求的。

当有很多用户 请求php时，是多个进程来处理用户请求的，瓶颈多数都是读写数据库，文件，session等这些会加锁的地方，而解决这些问题，

业界都已经有了很成熟的方案。和php是不是多线程没有什么关系。

3. php为什么选择多进程，而不是多线程

首先，定位不同，php不像c++ ,[JAVA](https://www.isolves.com/it/cxkf/yy/JAVA/),定位计算密集场景，php就是为web开发而生的语言，更侧重开发效率。web应用就是IO密集型的场景，数据库操作，文件读写，网络

IO，这些对于系统的进程调度，进程切换消耗时间都要大得多，所以phper在写代码的时候，没必要把精力放到性能上，而是功能的实现，虽然很多人

说php不适合大型应用，应对高并发场景乏力，确实以前是有这样的问题，但是现在php在处理大数据，高并发都有了很成熟的解决方案。

4. 那么问题来了，php缺陷 ，这样一门语言，我们如何取舍，还要不要用呢，

首先，php开发效率高，上手容易，保证一用你就会爱上她，如果结合其他性能较高的语言，可以扬长避短；

另外，php现在有了swoole 、workerman这些高性能的扩展，这些都是常驻内存的，性能很高，对性能要求高的场景可以用，对phper来说就是无缝接入

完全没有门槛。

最后，对于大多数程序员来说，换个go java这些原生就支持多线程的语言，也不是什么难事。

4.场景

我和很多phper一样，如果问我，有什么场景，必须要php多线程来实现呢，

php也有多线程pthreads扩展，PHP 默认并不支持多线程，要使用多线程需要安装 pthread 扩展，而要安装 pthread 扩展，必须使用 --enable-maintainer-zts 参数重新编译 PHP，

这个参数是指定编译 PHP 时使用线程安全方式。如果有用过java之类语言的多线程，用起来php多线程也是信手拈来的事情。

PHP扩展

https://www.zhihu.com/zvideo/1409432296419799040
