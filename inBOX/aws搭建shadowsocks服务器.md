# aws搭建shadowsocks服务器

[烂泥：aws搭建shadowsocks服务器](http://blog.chinaunix.net/uid-21710354-id-5151838.html)

分类： LINUX

2015-08-08 11:48:23

前段时间一直在忙openvpn的事情，现在手头有台aws服务器，打算利用起来。

如何利用呢？利用这台服务器进行科学上网，实际需求如下。

**一、实际需求**

在国内搜索技术文章，如果使用baidu的话，你懂的。因此就转向google，但是由于众所周知的原因，我们无法访问google。

刚好现在有一台aws的服务器，就想弄个shadowsocks服务器进行科学上网。

为什么使用shadowsocks进行科学上网呢？

shadowsocks有如下特点：

快速（异步I/O和事件驱动程序）

安全（所有的流量都经过加密算法加密，支持自定义算法）

支持移动客户端（专为移动设备和无线网络优化）

跨平台（可运行于包括PC，Mac，手机（Android和iOS）和路由器（OpenWrt）在内的多种平台上）

开源、易于维护

**二、安装shadowsocks**

aws服务器，我使用的是亚马逊的自己的系统Amazon Linux AMI，如下：

cat /etc/system-release

![clip_image001](0.46432994939521927-20220205171151-shcml5j.png "clip_image001")[http://blog.chinaunix.net/attachment/201508/8/21710354_1439005514GQJs.png](http://blog.chinaunix.net/attachment/201508/8/21710354_1439005514GQJs.png)

仔细对比了下，其实这个系统就是centos系统，不过应该是经过定制的。

有关shadowsocks的安装，我们可以通过以下网址进行查看，如下：[https://github.com/shadowsocks/shadowsocks](https://github.com/shadowsocks/shadowsocks)

centos下安装命令如下：

yum install -y python-setuptools

easy_install pip

pip install shadowsocks

如果是ubuntu系统，安装命令如下：

sudo apt-get -y install python-gevent python-pip

sudo pip install shadowsocks

sudo apt-get -y install python-m2crypto

**三、配置shadowsocks**

无论是centos系统还是ubuntu系统，shadowsocks配置都是一样的。

shadowsocks安装完毕后，可以查看使用ssserver命令进行查看。如下：

ssserver -h

![clip_image002](0.8278166840835091-20220205171151-2dkh836.png "clip_image002")[http://blog.chinaunix.net/attachment/201508/8/21710354_1439005517Ykiq.png](http://blog.chinaunix.net/attachment/201508/8/21710354_1439005517Ykiq.png)

如果没有ssserver命令的话，一般是在/usr/local/bin目录下，如下：

which ssserver

![clip_image003](0.07576469200770933-20220205171151-llsnwtt.png "clip_image003")[http://blog.chinaunix.net/attachment/201508/8/21710354_1439005523vdNv.png](http://blog.chinaunix.net/attachment/201508/8/21710354_1439005523vdNv.png)

我们只需要把/usr/local/bin加入到/etc/profile文件中即可。

创建shadowsocks目录，并创建其配置文件，如下：

mkdir /etc/shadowsocks

vim /etc/shadowsocks/config.json

{

"server":"0.0.0.0",

"server_port":1194,

"local_address":"127.0.0.1",

"local_port":1080,

"password":"asto!@#123456",

"timeout":300,

"method":"aes-256-cfb",

"fast_open":false,

"workers": 1

}

![clip_image004](0.8807460823185693-20220205171151-weto6h3.png "clip_image004")[http://blog.chinaunix.net/attachment/201508/8/21710354_1439005527CmMp.png](http://blog.chinaunix.net/attachment/201508/8/21710354_1439005527CmMp.png)

shadowsocks配置文件的内容，我就不做过多的介绍了，很简单的。

配置文件弄好后，我们现在来启动shadowsocks，如下：

ssserver -c /etc/shadowsocks/config.json -d start

netstat -tunlp

![clip_image005](0.545652690522955-20220205171151-egwa3dd.png "clip_image005")[http://blog.chinaunix.net/attachment/201508/8/21710354_1439005532hzK0.png](http://blog.chinaunix.net/attachment/201508/8/21710354_1439005532hzK0.png)

如果要停止shadowsocks服务的话，我们可以使用如下命令：

ssserver -c /etc/shadowsocks/config.json -d stop

![clip_image006](0.7947950195757726-20220205171151-v8uma8u.png "clip_image006")[http://blog.chinaunix.net/attachment/201508/8/21710354_1439005537n4Wp.png](http://blog.chinaunix.net/attachment/201508/8/21710354_1439005537n4Wp.png)

**四、连接shadowsocks服务**

shadowsocks服务器搭建完毕后，我们现在来客户端连接shadowsocks服务器。

shadowsocks客户端有Windows版本和Linux版本，我们分别介绍其使用方法。

**4.1 Windows版本**

windows版本，我们可以从如下网址进行下载，如下：

https://github.com/shadowsocks/shadowsocks-windows/releases

下载完毕后，双击Shadowsocks.exe，如下：

![clip_image007](0.28691104941352713-20220205171151-eifjwqq.png "clip_image007")[http://blog.chinaunix.net/attachment/201508/8/21710354_14390055462ZRW.png](http://blog.chinaunix.net/attachment/201508/8/21710354_14390055462ZRW.png)

在弹出的窗口中，填入Shadowsocks服务器的IP、Shadowsocks服务器端口已经密码，就可以连接Shadowsocks服务器，如下：

![clip_image008](0.5294785523498352-20220205171151-ckkpnmk.png "clip_image008")[http://blog.chinaunix.net/attachment/201508/8/21710354_1439005550FrIG.png](http://blog.chinaunix.net/attachment/201508/8/21710354_1439005550FrIG.png)

正确连接Shadowsocks服务器后，我们就可以进行科学上网了。

如下：

![clip_image009](0.8976383380545796-20220205171151-5h09q9m.png "clip_image009")[http://blog.chinaunix.net/attachment/201508/8/21710354_14390056852usz.png](http://blog.chinaunix.net/attachment/201508/8/21710354_14390056852usz.png)

当然我们也可以设置为全局代理，如下：

![clip_image010](0.4020420838660047-20220205171151-jq3iggj.png "clip_image010")[http://blog.chinaunix.net/attachment/201508/8/21710354_1439005687uu8v.png](http://blog.chinaunix.net/attachment/201508/8/21710354_1439005687uu8v.png)

![clip_image011](0.6119172702040473-20220205171151-5au1rtb.png "clip_image011")[http://blog.chinaunix.net/attachment/201508/8/21710354_143900569079QQ.png](http://blog.chinaunix.net/attachment/201508/8/21710354_143900569079QQ.png)

通过上图，我们可以看到目前我们的IP地址就是aws服务器所在的地址了。

我们也可以把这个代理让局域网中的其他用户使用，如下：

![clip_image012](0.2170644061672479-20220205171151-i28w8py.png "clip_image012")[http://blog.chinaunix.net/attachment/201508/8/21710354_143900569220H2.png](http://blog.chinaunix.net/attachment/201508/8/21710354_143900569220H2.png)

经过这样设置后，只要局域网中的机器连接我这台机器的1080端口，就可以使用代理了。

**4.2 Linux版本**

linux版本的shadowsocks客户端，我们一般选用cow这款软件。下载地址如下：

https://github.com/cyfdecyf/cow

安装就比较简单了，执行如下命令即可：

curl -L git.io/cow | bash

安装完毕后，我们来修改.cow/rc文件，如下：

grep -vE "^#|^$" .cow/rc

![clip_image013](0.34937610911560607-20220205171151-etw1632.png "clip_image013")[http://blog.chinaunix.net/attachment/201508/8/21710354_14390056935IEz.png](http://blog.chinaunix.net/attachment/201508/8/21710354_14390056935IEz.png)

这样的话，局域网的其他机器只要连接该机器的7778端口就可以进行科学上网了。

其实，我们还有一个类似这个软件MEOW，地址如下：

[https://github.com/renzhn/MEOW](https://github.com/renzhn/MEOW)

使用方法和cow一样，再次就不做讲解。

**4.3 Linux设置全局代理**

我们再来讲解一个在linux环境使用全局代理的方案，因为有时候我们的软件更新需要使用到国外的站点，但是众所周知的原因，不能更新。那么我们就只能通过VPN或者其他方式进行更新软件了，这样比较麻烦。

今天介绍一个比较方便的方法，就是把所有本机的请求都通过代理走。配置如下：

vim /etc/profile

http_proxy=192.168.1.180:1080

export http_proxy

https_proxy=192.168.1.180:1080

export https_proxy

ftp_proxy=192.168.1.180:1080

export ftp_proxy

**以上几行命令的意思是：无论是http、https、ftp请求，我们都让它通过192.168.1.180:1080的通过，而192.168.1.180:1080就是我们局域网的代理服务器的地址。**

![clip_image014](0.018843282253880888-20220205171151-eg36ddx.png "clip_image014")[http://blog.chinaunix.net/attachment/201508/8/21710354_1439005695z78P.png](http://blog.chinaunix.net/attachment/201508/8/21710354_1439005695z78P.png)

source /etc/profile

![clip_image015](0.8389497531009629-20220205171151-he7ato2.png "clip_image015")[http://blog.chinaunix.net/attachment/201508/8/21710354_14390056974MAl.png](http://blog.chinaunix.net/attachment/201508/8/21710354_14390056974MAl.png)

然后我们在使用curl cip.cc进行测试如下：

curl cip.cc

![clip_image016](0.1707886098509259-20220205171151-hqowi47.png "clip_image016")[http://blog.chinaunix.net/attachment/201508/8/21710354_1439005701ZFo5.png](http://blog.chinaunix.net/attachment/201508/8/21710354_1439005701ZFo5.png)

通过上图，我们可以很清楚的看到目前这台linux服务器已经使用的是shadowsocks服务器地址。

如果是代理服务器需要用户名和密码的话，可以使用如下格式：export http_proxy=http://username:password@ip:port
这样就达到了我们对linux使用全局代理的功能。
