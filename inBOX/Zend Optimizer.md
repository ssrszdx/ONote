# Zend Optimizer

## 什么是opcache? Zend Optimizer强势来临 (windows)

标签： [linux](https://www.programminghunter.com/tag/linux/ "linux")  [什么是opcache? Zend Optimizer强势来临](https://www.programminghunter.com/tag/%E4%BB%80%E4%B9%88%E6%98%AFopcache%3F+Zend+Optimizer%E5%BC%BA%E5%8A%BF%E6%9D%A5%E4%B8%B4/ "什么是opcache? Zend Optimizer强势来临")

PHP官方在2013-05-09日释放了最新版本的php, 5.5.0rc1正式发布, 同时发布的还有php 5.4.15正式版, 两版本均自带64位环境压缩包, 在当前大内存下, 64位编译包是非常可取的. 经过了4个版本的beta测试, rc1版本更新的内容不多, 都是细节异常修复. 可我们仍然能够朌望其中的一个加载件:Zend Optimizer, 官方在开发5.5.0时就放出消息, 会集成Zend Optimizer, 那Zend Optimizer是什么? 我们怎么测试呢.

正好在本地win8 x64位上安装了5.5.0rc1 x64, 尝试着对Zend Optimizer进行一次全面体验.

Zend Optimizer编译到php环境中名字为opcache, 即优化缓存的意思. 其中: php\ext目录中会有php_opcache.dll, php\php.ini-production.ini文件底部都有opcache的信息.

加载opcache:  
打开php.ini文件, 在最底部增加如下配置:

```
[opcache] 
zend_extension = "D:\xampp\php\ext\php_opcache.dll" 
opcache.memory_consumption=1024 
opcache.optimization_level=1 
opcache.interned_strings_buffer=8 
opcache.max_accelerated_files=4096 
opcache.revalidate_freq=60 
opcache.fast_shutdown=1 
opcache.enable=1 
opcache.enable_cli=1
```

还有许多其它的选项, 可以参考php.ini-production.ini来配置, 理解它的意思;  
重新启动apache, 打印phpinfo();信息后, 即可找到Zend OPcache信息. 

![image.png](image-20220205104446-6eufu2z.png)

在phpinfo()信息中, 目前来看有两条信息犹为重要:

```
Cache hits     (高级缓存命中) 
Cache misses  (高级缓存未命中)
```

在这两条信息中即可观察缓存运行情况, 一目了然  
高速缓存带来哪些优化呢? 对代码运行有多大帮助?  
我们做个测试, 验证一下什么是opcache.

```
<?php
  echo 'opcache';
?>
```

这是一段非常简单的php代码, 请保存为demo.php文件然后访问, 随意刷新, Cache hits数值会不停地增加, 说明起作用了,  
然后你修改代码为:

```
<?php
   echo 'cache new';
?>
```

再刷新demo.php, 应该可以看到效果, 打印出来的值仍然是opcache, 即源码被缓存了, 它不再解析demo.php文件, 试着不停地刷新, 检测多少秒后才更新.  
可设置: opcache.force_restart_timeout=180 的时间来控制更新速度.

这就类似于web项目中的静态文件缓存一下, 比如我们加载一个网页, 浏览器会自动帮我们把jpg, css缓存起来, 唯独php没有缓存, 每次均需要open文件, 解析代码,  执行代码这一过程, 而opcache即可解决这个问题, 代码会被高速缓存起来, 提升访问速度.

xampps环境包已经集成此功能, 不过没有开启, 欢迎大家试用.

个人建议: 本地环境非必要情况下不要开启opcache, 服务器上可以开启, 必竟不是天天更新. 缓存起来有它的历史意义.
