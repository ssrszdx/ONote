# Apache服务器

 1.Web服务器

Apache是世界使用排名第一的Web[服务器](http://baike.baidu.cn/view/899.htm)软件。它可以运行在几乎所有广泛使用的[计算机平台](http://baike.baidu.cn/view/2269685.htm)上，由于其[跨平台](http://baike.baidu.cn/view/469855.htm)和安全性被广泛使用，是最流行的Web服务器端软件之一。同时Apache音译为阿帕奇，是北美印第安人的一个部落，叫阿帕奇族，在美国的西南部。也是一个基金会的名称、一种武装直升机等等。

目录

[简介](http://baike.baidu.cn/view/28283.htm#1)​[发展历史](http://baike.baidu.cn/view/28283.htm#2)​[安装，配置与启用SSL安全](http://baike.baidu.cn/view/28283.htm#3)​[特点](http://baike.baidu.cn/view/28283.htm#4)​[相关替代品](http://baike.baidu.cn/view/28283.htm#5)​[版本发布](http://baike.baidu.cn/view/28283.htm#6)​[apache错误日志](http://baike.baidu.cn/view/28283.htm#7)

## 简介

 **Apache HTTP Server** （简称 **Apache** ）是[Apache软件基金会](http://baike.baidu.cn/view/7044910.htm)的一个开放源码的网页服务器，可以在大多数计算机操作系统中运行，由于其多平台和安全性被广泛使用，是最流行的Web服务器端软件之一。它快速、可靠并且可通过简单的API扩展，将Perl/Python等解释器编译到服务器中。^[1]^

[Apache](http://baike.baidu.cn/view/28283.htm)http server是世界使用排名第一的[Web服务器](http://baike.baidu.cn/view/460250.htm)软件。它可以运行在几乎所有广泛![Apache Server配置界面](http://d.hiphotos.baidu.com/baike/s%3D220/sign=7db1a0eafd1f4134e437027c151e95c1/962bd40735fae6cda02696680fb30f2442a70f2c.jpg "Apache Server配置界面")[http://baike.baidu.cn/picview/28283/5418752/0/79b1e936746c71080b55a94e.html](http://baike.baidu.cn/picview/28283/5418752/0/79b1e936746c71080b55a94e.html)Apache Server配置界面

使用的[计算机平台](http://baike.baidu.cn/view/2269685.htm)上。

Apache源于NCSAhttpd服务器，经过多次修改，成为世界上最流行的[Web服务器](http://baike.baidu.cn/view/460250.htm)软件之一。Apache取自“a patchy server”的读音，意思是充满补丁的服务器，因为它是[自由软件](http://baike.baidu.cn/view/20965.htm)，所以不断有人来为它[开发](http://baike.baidu.cn/view/522596.htm)新的功能、新的特性、修改原来的缺陷。Apache的特点是简单、速度快、性能稳定，并可做[代理服务器](http://baike.baidu.cn/view/751.htm)来使用。

本来它只用于小型或试验[Internet](http://baike.baidu.cn/view/11165.htm)网络，后来逐步扩充到各种[Unix](http://baike.baidu.cn/view/8095.htm)系统中，尤其对[Linux](http://baike.baidu.cn/view/1634.htm)的支持相当完美。Apache有多种产品，可以支持[SSL](http://baike.baidu.cn/view/16147.htm)技术，支持多个[虚拟主机](http://baike.baidu.cn/view/7383.htm)。Apache是以[进程](http://baike.baidu.cn/view/19746.htm)为基础的结构，进程要比[线程](http://baike.baidu.cn/view/1053.htm)消耗更多的系统开支，不太适合于多处理器环境，因此，在一个Apache Web站点扩容时，通常是增加[服务器](http://baike.baidu.cn/view/899.htm)或扩充群集节点而不是增加[处理器](http://baike.baidu.cn/view/50152.htm)。到目前为止Apache仍然是世界上用的最多的Web服务器，市场占有率达60%左右。世界上很多著名的网站如[Amazon](http://baike.baidu.cn/view/552703.htm)、Yahoo!、W3 Consortium、Financial Times等都是Apache的产物，它的成功之处主要在于它的源代码开放、有一支开放的开发队伍、支持[跨平台](http://baike.baidu.cn/view/469855.htm)的应用（可以运行在几乎所有的[Unix](http://baike.baidu.cn/view/8095.htm)、Windows、[Linux](http://baike.baidu.cn/view/1634.htm)系统平台上）以及它的可移植性等方面。

Apache的诞生极富有戏剧性。当NCSAWWW服务器项目停顿后，那些使用NCSA WWW服务器的人们开始交换他们用于该服务器的补丁程序，他们也很快认识到成立管理这些补丁程序的论坛是必要的。就这样，诞生了Apache Group，后来这个团体在[NCSA](http://baike.baidu.cn/view/209578.htm)的基础上创建了Apache。

Apache[web服务器](http://baike.baidu.cn/view/460250.htm)软件拥有以下特性：

支持最新的HTTP/1.1通信协议

拥有简单而强有力的基于文件的配置过程

支持通用网关接口

支持基于IP和基于域名的虚拟主机

支持多种方式的[HTTP](http://baike.baidu.cn/view/9472.htm)认证

集成[Perl](http://baike.baidu.cn/view/46614.htm)处理模块

集成[代理服务器](http://baike.baidu.cn/view/751.htm)模块

支持实时监视服务器状态和定制服务器日志

支持服务器端包含指令(SSI)

支持安全Socket层(SSL)

提供用户会话过程的跟踪

支持FastCGI

通过[第三方](http://baike.baidu.cn/view/1243841.htm)模块可以支持Java Servlets

如果你准备选择Web服务器，毫无疑问Apache是你的最佳选择。

## 发展历史

Apache 起初由伊利诺伊大学香槟分校的国家超级电脑应用中心（NCSA）开发。此后，Apache 被开放源代码团体的成员不断的发展和加强。Apache 服务器拥有牢靠可信的美誉，已用在超过半数的因特网站中－特别是几乎所有最热门和访问量最大的网站。

开始，Apache只是Netscape网页服务器（现在是Sun ONE）之外的开放源代码选择。渐渐的，它开始在功能和速度超越其他的基于Unix的HTTP服务器。1996年4月以来，Apache一直是Internet上最流行的HTTP服务器: 1999年5月它在 57% 的网页服务器上运行；到了2005年7月这个比例上升到了69%。在2005年11月的时候达到接近70%的市占率，不过随着拥有大量域名数量的主机域名商转换为微软IIS平台，Apache市占率近年来呈现些微下滑。而Google自己的网页服务器平台GWS推出后，加上Lighttpd这 个轻量化网页服务器软件使用的网站慢慢增加，反应在整体网页服务器市占率上，根据netcraft在2007年7月的最新统计数据，Apache的市占率已经降为52.65%，8月时又滑落到50.92%。尽管如此，它仍旧是现阶段因特网市场上，市占率最高的网页服务器软件。

广的解释是（也是最显而易见的）:这个名字来自这么一个事实:当Apache在1995年初开发的时候，它是由当时最流行的HTTP服务器NCSA HTTPd 1.3 的代码修改而成的，因此是“一个修补的（a patchy）”服务器。然而在服务器官方网站的FAQ中是这么解释的:“‘Apache’这个名字是为了纪念名为Apache(印地语)的美洲印第安人土著的一支，众所周知他们拥有高超的作战策略和无穷的耐性”。无论如何，Apache 2.x 分支不包含任何 NCSA 的代码。^[1]^

## 安装，配置与启用SSL安全

Apache 的安装无外乎两种方式：[源代码](http://baike.baidu.cn/view/60376.htm)安装和二进制包安装。这两种安装类型各有特色，二进制包安装不需要编译，而源代码安装则需要先配置编译再安装，二进制包安装在一个固定的位置下，选择固定的模块，而源代码安装则可以让你选择安装路径，选择你想要的模块。本文主要介绍二进制DEB包安装方式(此方法只适用于Debian GNU/Linux 及其衍生版)。

系统：GNU/Linux Debian/etch

Apache当前版本： 2.4.2

1、安装：

使用以下命令安装：

tony@tonybox:~$sudo aptitude update aptitude install apache2 apache2-utils

其中apache2-utils提供了我们在配置维护过程中非常有用的一些工具

安装完成后，可以使用下面的命令启动Apache 服务：

tony@tonybox:~$ sudo /etc/init.d/apache2 start

停止Apache服务则是：

tony@tonybox:~$ sudo /etc/init.d/apache2 stop

也可以直接用 kill 命令强制杀死apache2进程

tony@tonybox:~$ sudo killall apache2

如有需要， 可以通过rcconf来控制是否在系统启动是加载Apache 服务

启动完成后打开浏览器， 使用URL http://localhost/ 来访问已经启动的Apache服务器， 服务器将会跳转到 http://localhost/apache2-default/, 向浏览器返回一个Apache安装成功的页面。

注： 这取决于/etc/apache2/sites-available/default 配置文件中， 是否取消了

RedirectMatch ^/$ /apache2-default/

行的注释

2、 配置文件说明

在Debian下， 安装完成后， 软件包为我们提供的配置文件位于/etc/apache2目录下：

tony@tonybox:/etc/apache2$ ls -l

total 72

-rw-r--r-- 1 root root 12482 2006-01-16 18:15 apache2.conf

drwxr-xr-x 2 root root 4096 2006-06-30 13:56 conf.d

-rw-r--r-- 1 root root 748 2006-01-16 18:05 envvars

-rw-r--r-- 1 root root 268 2006-06-30 13:56 httpd.conf

-rw-r--r-- 1 root root 12441 2006-01-16 18:15 magic

drwxr-xr-x 2 root root 4096 2006-06-30 13:56 mods-available

drwxr-xr-x 2 root root 4096 2006-06-30 13:56 mods-enabled

-rw-r--r-- 1 root root 10 2006-06-30 13:56 ports.conf

-rw-r--r-- 1 root root 2266 2006-01-16 18:15 README

drwxr-xr-x 2 root root 4096 2006-06-30 13:56 sites-available

drwxr-xr-x 2 root root 4096 2006-06-30 13:56 sites-enabled

drwxr-xr-x 2 root root 4096 2006-01-16 18:15[ssl](http://baike.baidu.cn/view/16147.htm)

其中

apache2.conf

为apache2服务器的主配置文件， 查看此配置文件， 你会发现以下内容

# Include module configuration:

Include /etc/apache2/mods-enabled/*.load

Include /etc/apache2/mods-enabled/*.conf

# Include all the user configurations:

Include /etc/apache2/httpd.conf

# Include ports listing

Include /etc/apache2/ports.conf

# Include generic snippets of statements

Include /etc/apache2/conf.d/[^.#]*

有此可见， apache2 根据配置功能的不同， 对配置文件进行了分割， 这样更利于管理

conf.d

下为配置文件的附加片断，默认情况下， 仅提供了 charset 片断，

tony@tonybox:/etc/apache2/conf.d$ cat charset

AddDefaultCharset UTF-8

如有需要我们可以将默认编码修改为 GB2312, 即文件的内容为： AddDefaultCharset GB2312

httpd.conf

是个空文件

magic

文件中包含的是有关mod_mime_magic模块的数据， 一般不需要修改它。

ports.conf

则为服务器监听IP和端口设置的配置文件，

tony@tonybox:/etc/apache2$ cat ports.conf

Listen 80

mods-available

目录下是一些。conf和。load 文件， 为系统中可以使用的加载各种模块的配置文件， 而mods-enabled目录下则是指向这些配置文件的符号连接， 从配置文件apache2.conf 中可以看出， 系统通过mods-enabled目录来加载模块， 也就是说， 系统仅通过在此目录下创建了符号连接的mods-available 目录下的配置文件来加载模块。同时系统还提供了两个命令 a2enmod 和 a2dismod用于维护这些符号连接。这两个命令由 apache2-common 包提供。命令各式也非常简单： a2enmod [module] 或 a2dismod [module]

sites-available

目录下为配置好的站点的配置文件， sites-enabled 目录下则是指向这些配置文件的符号连接， 系统通过这些符号连接来起用站点 sites-enabled目录下的符号连接附有一个数字前缀， 如000-default, 这个数字用于决定启动顺序， 数字越小， 启动优先级越高。 系统提供了两个命令 a2ensite 和 a2dissite 用于维护这些符号连接。这两个命令由 apache2-common 包提供。

/var/[www](http://baike.baidu.cn/view/1453.htm)

默认情况下将要发布的网页文件应该置于/var/www目录下，这一默认值可以同过主配置文件中的DocumentRoot 选项修改。

注意:如果你在是[windows](http://baike.baidu.cn/view/4821.htm)下应用Apache服务器,并且已经安装IIS,那么在安装Apache时请注意给Apache换个端口来监听[比如](http://baike.baidu.cn/view/6814120.htm)8080,否则Apache占用的端口会和IIS冲突,造成Apache服务器不能正常启动。

3.启用SSL让apache更安全

apache加密TCP/IP网络产品的标准是SSL ，对于Internet上普遍使用的超文本传输协议（HTTP）而言，其加密后的协议称为 HTTPS，缺省采用443端口。HTTPS数据是加密以后传输的，因此能有效保护在网络上传输的个人隐私信息。

对apache配置支持SSL需要经过如下的操作：

第一步：下载所需的软件并解开到 /usr/local/src 目录

Apache 1.3.24

Mod_ssl 2.8.8-1.3.24

Openssl-0.9.6c 

每个 mod_ssl 的版本和特定的 Apache 版本有关，因此要下载相对应的 mod_ssl 版本。

第二步：编译和安装

安装 OpenSSL 到 /usr/local/ssl： # pwd

/usr/local/src/openssl-0.9.6c

# ./config

# make

# make test

# make install

安装 mod_ssl，编译进 Apache 的源码树： # pwd

/usr/local/src/mod_ssl-2.8.8-1.3.24

# ./configure --with-apache=/usr/local/src/apache_1.3.24 \

--with-ssl=/usr/local/ssl

以 DSO 方式编译 Apache： # pwd

/usr/local/src/apache_1.3.24

# ./configure --prefix=/usr/local/apache --enable-rule=SHARED_CORE \

--enable-module=ssl --enable-shared=ssl

# make

创建 SSL 证书，证书需要从商业的认证权威机构或者从内部的 CA 得到。

执行下面的步骤生成证书： # pwd

/usr/local/src/apache_1.3.24

# make certificate TYPE=custom

生成证书时会提示两遍下面的信息：<> 内为示范数据。

第一遍： Country Name (2-letters)

State or Province Name

Locality Name

Organization Name

Organizational Unit Name

Common Name

Email Address

Certificate Validity <365>

第一遍会产生一个用于测试的 CA。"Common Name" 可以为任意文本。第二遍 Country Name (2-letters)

State or Province Name

Locality Name

Organization Name

Organizational Unit Name

Common Name

Email Address

Certificate Validity <365>

第二遍产生的是实际可用的证书，能被商业机构或者内部 CA 认证， "Common Name" 为 Web 服务器的主机名。

安装并运行 Apache # pwd

/usr/local/src/apache_1.3.24

# make install

启动 Apache ，并测试 # pwd

/usr/local/apache/bin

# ./apachectl stop

# ./apachectl startssl

在浏览器上检查你的站点正常与否即可，至此即可让apache支持安全的SSL。

在Apache 1.4以后的版本，我们还可以用以下命令完成服务的完美重启：

#./apachectl graceful

## 特点

[apache](http://baike.baidu.cn/view/28283.htm)日志为什么不记录百度蜘蛛？这个问题相信很多初学者都基本碰到了，apache日志默认是不记录百度蜘蛛、谷歌和各大搜索引擎的蜘蛛程序的，但只需要修改一个地方就可以解决这个问题，现在就直接将答案写出来：

比如曾经有个朋友在百度知道中提问：

<IfModule log_config_module>LogFormat "%h %l %u %t "%r" %>s %b "%{Referer}i" "%{User-Agent}i"" combinedLogFormat "%h %l %u %t "%r" %>s %b" common <IfModule logio_module> LogFormat "%h %l %u %t "%r" %>s %b "%{Referer}i" "%{User-Agent}i" %I %O" combinedio </IfModule>CustomLog "logs/access.log" common</IfModule>这是我目前的设置，不记住主机名哪位给我提供个范本 记录访问明细和主机头记录蜘蛛的

1、打开httpd.conf文件找到以下部分：

LogFormat "%h %l %u %t "%r" %>s %b "%{Referer}i" "%{User-Agent}i"" combinedLogFormat "%h %l %u %t "%r" %>s %b" commonLogFormat "%{Referer}i -> %U" referer

LogFormat "%{User-agent}i" agent

具体有关LogFormat的用法请参照：

2、接着我们继续向下移动，找到[虚拟主机](http://baike.baidu.cn/view/7383.htm)配置段，也就是VirtualHost段，这个是由你自己来配置的。本站的虚拟主机的日志文件是这样设置的：

CustomLog /var/html/faq/logs/[linux](http://baike.baidu.cn/view/1634.htm)520-access.log combined如果你想记录百度蜘蛛的访问全称，就按ru如上部分设置，如果不想记录百度蜘蛛的头部分，则如下设置：CustomLog /var/html/faq/logs/linux520-access.log common

## 相关替代品

Apache是目前最流行的Web应用服务器，占据了互联网应用服务器70%以上的份额。Apache能取得如此成功并不足为奇：它免费、稳定且性能卓越；但Apache能取得如此佳绩的另一个原因是，当时互联网刚刚兴起时，Apache是第一个可用的Web应用服务器，人们没有其他的选择。

不可否认，Apache是一个优秀的全能Web服务器，但对于那些需要更强大的Web应用服务器（比如大小、可定制、响应速度、可扩展性等方面）的人而言，Apache明显不符合他们的要求，寻找Apache的替代者是更好的选择。

下面所列出的是当前可以替代Apache的几个热门Web应用服务器，他们的特点和适用的应用场景各不相同，但都是针对Apache所不够擅长的某一方面设计的。

**1、Lighttpd**

最流行的Apache服务器替代者，

Lig[http](http://baike.baidu.cn/view/9472.htm)d是一个单线程的针对大量持续连接做出专门优化的Web服务器（这正是多数高流量网站和应用程序需要的）。众多的流行Web站点选择 **Lighttpd** ，包括Youtube、SourceForge和维基百科。Lighttpd支持FastCGI、HTTP服务器端压缩、mod-rewrite和其他众多有用的功能。尽管Lighttpd拥有Apache的绝大多数功能，但它仍然保持轻量级（仅1MB）并且可以与Apache使用相同的配置。

**2、Nginx**

Nginx是一个来自[俄罗斯](http://baike.baidu.cn/view/2403.htm)的流行的Web应用服务器，它被应用于大量的俄罗斯的高并发站点，俄罗斯的搜索引擎网站Rambler就是基于Nginx构建的。Nginx对静态页面的支持相当出色，轻量且免费。Nginx不支持CGI，但是支持更灵活的FastCGI。PHP5.2及之前的版本比较多的是使用PHP-FPM来管理PHP FastCGI进程。PHP-FPM使用给PHP源码打补丁后编译的方式让新手多少有些难上手，但从PHP 5.3.2开始内置PHP-FPM，只需编译PHP时启用PHP-FPM。

**3、Boa**

很多的网站管理员对在硬件配置较低的服务器上使用轻量级的Boa作为Web服务器极其信赖。

oa是一个单[线程](http://baike.baidu.cn/view/1053.htm)的HTTP服务器，这意味着Boa只能依次完成用户的请求而不会fork新的[进程](http://baike.baidu.cn/view/19746.htm)来处理并发请求。Boa的设计目的是速度和安全，对于运行于单服务器的流行Web[站点](http://baike.baidu.cn/view/391109.htm)而言，Boa是一个好的选择。

**4、Jigsaw**

Jigsaw是W3C推出的开源的Web服务器平台，使用Java语言编写，可以安装在有Java运行环境的系统上。做为W3C（World Wide Web Consortium）[开发](http://baike.baidu.cn/view/522596.htm)的服务器产品，其作用主要是对新技术的实现做一个例示，而非一个全功能的商业服务器产品。

不过就Jigsaw 2.0版本而言，它的功能还是超过了目前Web[服务器](http://baike.baidu.cn/view/899.htm)的平均水平。最重要的是，它体现了未来[HTTP协议](http://baike.baidu.cn/view/70545.htm)和基于对象的Web服务器技术的发展。如果你希望你的平台支持所有下一代技术，Jigsaw是一个好的选择。

以上所提到的四个Apache Web服务器的替代者只是目前众多优秀应用服务器产品的一部分。

## 版本发布

2012年08月18日，Apache HTTP Server 2.4.3 发布。^[2]^

2012年08月23日 ，Apache HTTP Server 2.2.23 发布。^[3]^

2013年02月25日 ，Apache HTTP Server 2.4.4 发布。^[4]^

## apache错误日志

这是一个非常普遍的现象，检查错误方法:进入cmd 然后进入 Apache安装目录(具体为你自己的安装目录)\bin>httpd.exe -w -n “Apache2″ -k start^[5]^
