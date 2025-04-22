# 使用 Xdebug + Webgrind 分析 PHP 程序

使用 Xdebug + Webgrind 分析 PHP 程序

**更新于 2013-02-22 04:38:55**UEANER

#### 安装 xdeubg zend 扩展

```
# yum install php-pecl-xdebug
```

#### 配置 php.d/xdebug.ini

```
# vi /etc/php.d/xdebug.ini; 加入以下内容; profiler
```

```
xdebug.profiler_enable=1
```

```
xdebug.profiler_enable_trigger=1
```

```
xdebug.profiler_output_dir=/tmp/xdebug
```

```
xdebug.profiler_output_name=cachegrind.out.%p
```

```
```

```
; trace
```

```
xdebug.auto_trace=1
```

```
xdebug.show_exception_trace=1
```

```
xdebug.trace_output_dir=/tmp/xdebug
```

```
xdebug.trace_output_name=trace.%c
```

注：需手工创建 `/tmp/xdebug` 目录且所属用户应为 `php-fpm` 配置文件中的 `user` 用户。 配置完毕，重启 `service php-fpm restart`。

#### 下载 webgrind

```
$ cd /var/www/html/
```

```
$ git clone git://github.com/jokkedk/webgrind.git
```

#### 安装 graphviz

```
# yum install graphviz
```

`graphviz` 附带 `dot` 命令用于下文的配置。

#### 使用 webgrind

webgrind v1.1 的版本增加了 `Show Call Graph` 按钮，依赖 `python` 和 `dot` 命令， 类似 xhprof 的 `View Full Callgraph` 绘制程序调用流程图功能，相对于 xhprof 较简洁。 更多 xhprof 信息请查看 [使用 XHProf 分析你的 PHP 程序](http://blog.aboutc.net/php/17/php-profiler-xhprof)。

配置 webgrind 的绘制流程图功能，查看 `python` 和 `dot` 命令位置 `which python` `which dot`，编辑 `webgrind/config.php`，将 `$pythonExecutable = '/usr/bin/python'` 和`$dotExecutable = '/usr/bin/dot'` 替换为刚查到相应路径。

在浏览器访问任意一个项目路径，`/tmp/xdebug` 目录下会生成一个 `cachegrind.out.xxxxx` 文件作为 webgrind 的数据来源，打开 webgrind 页面，如：`http://localhost/webgrind/`， 点击右上角 `update` 按钮即会分析最后一次访问的项目地址的 PHP 程序，可以通过选择第二个 下拉框中的地址来分析相应的程序，点击 `Show Call Graph` 按钮就会显示当前分析的程序的 简明调用流程图。

表格中后三列的含义：

```
Invocation Count：函数调用次数Total Self Cost：函数本身花费的时间Total Inclusive Cost：包含内部函数花费的时间
```

#### 简析 webgrind 生成流程图

`webgrind` 生成流程图使用源码下的 library/[gprof2dot.py](http://code.google.com/p/jrfonseca/wiki/Gprof2Dot) `python` 脚本， 先将`cachegrind.out.xxxxx` 转换成 [DOT](http://zh.wikipedia.org/wiki/DOT%E8%AF%AD%E8%A8%80)，再使用 `dot` 命令转换成图片输出。

PHP 使用 [shell_exec](http://www.php.net/shell_exec) 函数执行：

```
/usr/bin/python library/gprof2dot.py -n 10 -f callgrind /tmp/xdebug/cachegrind.out.xxxxx | /usr/bin/dot -Tpng -o /tmp/xdebug/cachegrind.out.xxxxx-10.webgrind.png
```

```
转载请注明出处。
本文地址：http://blog.aboutc.net/profiling/18/php-profiler-xdebug-webgrind
```

Pasted from: [[http://blog.aboutc.net/profiling/18/php-profiler-xdebug-webgrind](http://blog.aboutc.net/profiling/18/php-profiler-xdebug-webgrind)](%5Bhttp://blog.aboutc.net/profiling/18/php-profiler-xdebug-webgrind%5D(http://blog.aboutc.net/profiling/18/php-profiler-xdebug-webgrind))
