# 子网掩码和网关DNS

例子：

　　以Windows系统中IP地址设置界面为参考(如图1)，IP地址, 子网掩码, 默认网关 和 DNS服务器, 这些都是什么意思呢？

　　​![image|414](image-20230823102653-2qangmb.png)​　　

‍

　　学习IP地址的相关知识时还会遇到网络地址,广播地址,子网等概念,这些又是什么意思呢 ？

一 IP地址  
概述

计算机要实现网络通信，就必须要有一个用于快速定位的网络地址。IP地址就是计算机在网络中的唯一身份ID，与现实世界中快递的配送需要有具体的住宅地址是一个道理。

ip地址以圆点分隔号的四个十进制数字表示，每个数字从0到255，如某一台主机的ip地址为：128.20.4.1

IP地址的组成

IP地址 = 网络地址 + 主机地址(又称：主机号和网络号组成)

想想，为什么会有行政区划的划定（国家、省市区、街道等），为了更加高效的进行管理、定位；

相同的，我们通常将网络也可以分为很多的子网络，每个子网络有自己的网络地址，每个子网络由很多的计算机组成（当然也可以包含另外一个子网络）。

我们要找到指定的IP地址，只要先找到指定的网络地址，然后再该网络内找到对应的主机地址即可。

　IP地址是一个 4 * 8bit（1字节）由 0/1 组成的数字串（IP4协议）

以文章开头 win7 截图中 的 IP地址 192.168.1.168, 子网掩码 255.255.255.0(下文有详解) 为例,  这个地址中包含了很多含义:

192.168.100.168（IP地址） = 192.168.1.0 (网络地址) + 0.0.0.168（主机地址）

网络地址、主机地址是怎么计算出来的呢？我们需要先简单学习下子网掩码

二 子网掩码(subnet mask)参照：《 百度百科-子网掩码》  
　　IP中的网络地址和主机地址各是多少位表示呢？如果不指定，就不知道哪些位是网络号、哪些是主机号，这就需要通过子网掩码来实现。

　　概述

　　子网掩码又叫网络掩码、地址掩码、子网络遮罩，是一个 4 * 8bit（1字节）由 0/1 组成的数字串。

　　它的作用是屏蔽（遮住）IP地址的一部分以划分成网络地址和主机地址两部分，并说明该IP地址是在局域网上，还是在远程网上。

　　通过子网掩码，可以把网络划分成子网，即VLSM（可变长子网掩码），也可以把小的网络归并成大的网络即超网。

　　子网掩码不能单独存在，它必须结合IP地址一起使用。

‍

　　子网掩码的规则

　　　　长度 为 4 * 8bit（1字节），由 连续的1 以及 连续的0 两部分组成，

　　　　　　 例如：11111111.11111111.11111111.00000000，对应十进制：255.255.255.0

　　假设，局域网中 计算机A 的IP地址为 192.168.1.1，子网掩码为 255.255.255.0， 如下图所示：

​![image](image-20230823102725-apsf95s.png)​

　　

网络地址: IP 地址中被 连续的1 遮住的部分，即 11000000.10101000.00000001.00000000, 对应的网络地址：192.168.1.0

主机地址: IP 地址中被 连续的0 遮住的部分，即 00000000.00000000.00000000.00000001, 对应的网络地址：0.0.0.1

排除 该网络 两个特殊地址：

　　 广播地址：192.168.1.255　　（主机号全为11111111）

　　网络地址：192.168.1.0 　　　（主机号全为00000000）

该子网最大的主机数：2的8次方 256 - 2

注意点1、子网掩码与ip地址作 与 运算，可以得到该网络的网络号,也就是网络地址，仅有ip地址我们是无法得知要进行通信的计算机处在哪个网络地址上的，所以ip地址必须要搭配子网掩码来使用；处于同一网络上的两台计算机之间是可以直接通信的，而不同网络上的计算机之间是无法直接通信的，此时就需要网关上场啦！

注意点2、同一网络不是指物理连接,而是指网络地址。用网线将两台计算机直接连接起来，但是在两台计算机上分别设置不同的网络地址，则它们之间也是无法直接进行通信的！

其他信息：

　A类地址来说，默认的子网掩码是255.0.0.0；对于B类地址来说默认的子网掩码是255.255.0.0；对于C类地址来说默认的子网掩码是255.255.255.0。

三 公网IP 私网IP  
公网IP和私网IP的区别：

1、在Internet网络上有上千百万台主机，为了能够将这些主机区分开来，于是就给每台主机都分别配了一个专门的地址，称为IP地址。

2、通过IP地址就可以访问到每一台主机。IP地址由4部分数字组成，ghost win7每部分数字对应于8位二进制数字，各部分之间用小数点分开。

3、固定IP：固定IP地址是长期固定分配给一台计算机使用的IP地址，一般是特殊的服务器才拥有固定IP地址。动态IP：因为IP地址资源非常短缺，通过电话拨号上网或普通宽带上网用户一般不具备固定IP地址，而是由ISP动态分配暂时的一个IP地址。

4、公有地址(Public address)由Inter NIC(Internet Network Information Center 因特网信息中心)负责。这些IP地址分配给注册并向Inter NIC提出申请的组织机构。通过它直接访问因特网。

5、私有地址(Private address)属于非注册地址，专门为组织机构内部使用。

最大区别是公网IP世界只有一个，私网IP可以重复，但是在一个局域网内不能重复，访问互联网是需要IP地址的，IP地址又分为公网IP和私网IP，访问互联网需要公网IP作为身份的标识，而私网IP则用于局域网，在公网上是不能使用私网IP地址来实现互联网访问的。公网IP在全球内是唯一的。

私网IP是专门给一些局域网内用的。也就是说在网络上是不唯一的，公网上是不能通这个私有IP来找到对应的设备的。

四 通过子网掩码计算网络地址

　　参考：《IP地址，子网掩码，默认网关，DNS服务器详解》

计算方法

计算过程是这样的:

　　1. 将IP地址和子网掩码都换算成二进制；

　　2. 将两者进行 与 运算，得到网络地址。

　　　　计算过程：上下对齐,  1位1位的算, 1与1=1 , 其余组合都为0

​![image|189](image-20230823102748-d7dxwb5.png) 	

假设 IP地址为 192.168.1.168，子网掩码为 255.255.255.0， 则网络地址换算步骤如下:

　　1)将IP地址和子网掩码分别换算成二进制 　　

　　　　192.168.1.168 换算成二进制为 11000000.10101000.00000001.10101000  
　　　　255.255.255.0 换算成二进制为 11111111.11111111.11111111.00000000

　　2)将二者进行与运算

​![image|313](image-20230823102805-ichwejv.png)​

　　3) 将运算结果换算成十进制： 192.168.1.0

立即实践

以用网线直接将两台计算机连起来为例：

​![image|203](image-20230823102815-2vesd84.png)​

　　

‍

下面是几种IP地址设置, 看看在不同设置下网络是通还是不通.

实验

编号

1号机器	2号机器	网络连通  
IP地址	子网掩码	网络地址	IP地址	子网掩码	网络地址  
1	192.168.0.1	255.255.255.0	192.168.0.0	192.168.0.200	255.255.255.0	192.168.0.0	Y  
2	192.168.0.1	255.255.255.0	192.168.0.0	192.168.1.200	255.255.255.0	192.168.1.0	N  
3	192.168.0.1	255.255.255.192	192.168.0.0	192.168.0.200	225.225.225.192	192.168.0.192	N  
说明：第1种情况能通是因为这两台计算机处在同一网络192.168.0.0, 所以能通,而2,3种情况下两台计算机处在不同的网络,所以不通.

网络地址的计算过程同上，不再赘述。

结论:

　　用网线直接连接 或 通过 HUB（集线器）、普通交换机链接的计算机必须处于同一网络(网络地址) 并且主机地址必须不一样 才能通信。

　　注意：同一网络不是指物理连接，而是指网络地址.

　　举个例子，两台计算机链接到相同路由器（简单理解为同一个链路），如果他们设置的网络地址不一致，则他们也是不能通信的。

　　扩展：IP网段表示法

　　　　举例说明：192.168.0.0/24

　　　　192.168.0.0: 网络地址

　　　　24: 表示子网掩码二进制表示法中，连续的 1 的 个数，这里为：11111111·11111111·11111111·00000000，即 255.255.255.0

‍

五. 默认网关（地址）  
　　参考：《 百度百科-网关》

什么是网关？

　　（可以联想下海关？什么是海关？）

　　连接两个不同的网络的设备都可以叫网关设备；网关的作用就是实现两个网络之间进行通讯与控制。

　　网关设备可以是 交互机（三层及以上才能跨网络）、路由器、启用了路由协议的服务器、代理服务器、防火墙等

网关地址就是网关设备的IP地址。

　　假设我们有两个网络：

　　网络A的IP地址范围为“192.168.1.1~192.168.1.254”，子网掩码为255.255.255.0

　　网络B的IP地址范围为“192.168.2.1~192.168.2.254”，子网掩码为255.255.255.0

要实现这两个网络之间的通信，则必须通过网关。

如果网络A中的主机发现数据包的目的主机不在本地网络中，就把数据包转发给它自己的网关，再由网关转发给网络B的网关，网络B的网关再转发给网络B的某个主机（如附图所示）。网络A向网络B转发数据包的过程。

　　　​![image](image-20230823102815-2vesd84.png)​

只有设置好网关的IP地址，TCP/IP协议才能实现不同网络之间的相互通信。

默认网关

一台主机可以有多个网关。默认网关的意思是一台主机如果找不到可用的网关，就把数据包发给默认指定的网关，由这个网关来处理数据包。现在主机使用的网关，一般指的是默认网关。

扩展：自动设置默认网关

自动设置就是利用DHCP（Dynamic Host Configuration Protocol, 动态主机配置协议）服务器来自动给网络中的计算机分配IP地址、子网掩码和默认网关 。

一旦网络的默认网关发生了变化时，只要更改了DHCP服务器中默认网关的设置，那么网络中所有的计算机均获得了新的默认网关的IP地址。这种方法适用于网络规模较大、TCP/IP参数有可能变动的网络。

另外一种自动获得网关的办法是通过安装代理服务器软件（如MS Proxy）的客户端程序来自动获得，其原理和方法和DHCP有相似之处。

　　扩展说明

　　问：在网上看到一些人提问：连接到相同（二层）交换机或集线器上的计算机，如果设置不同的网络地址，为什么不能通信。

　　答：

　　　　在 TCP/IP 协议中，网络层（通过IP地址识别通信方）封包完成交给下一层数据链路层（通过MAC地址识别通信方）时，需要通过 ARP 广播 获取目标 IP 对应的 MAC 地址。

　　但因为 ARP 报文只能在相同网络地址内广播，如果目标计算机与源计算机处于不同网络，则无法进行响应，因此源计算机无法完成链路层数据的封装。

　　ARP 协议相关信息可见  这里。

特别说明1：一台主机可以有多个网关，默认网关的意思就是当一台主机找不到可用的网关时，就会将数据包发给默认网关，由默认网关来处理数据包；

特别声明2：网卡(又叫网络适配器)负责的是物理层和数据链路层，网卡上有MAC地址，对应着网络层中的ip地址，同时网卡还负责将数字信号转换为光电等信号与电缆、光缆进行通信；   而网关是网络层的应用，主要负责ip地址的路由选择；

六 网卡介绍  
网关和网卡不一样，具体区别如下：

1、含义上的区别

网关：就是一个网络连接到另一个网络的“关口”。也就是网络关卡。

网卡：是一块被设计用来允许计算机在计算机网络上进行通讯的计算机硬件。

2、主要功能上的区别

网关：是一种充当转换重任的计算机系统或设备。使用在不同的通信协议、数据格式或语言，甚至体系结构完全不同的两种系统之间，网关是一个翻译器。网关既可以用于广域网互联，也可以用于局域网互联。

网卡：的主要功能是数据的封装与解封，发送时将上一层传递来的数据加上首部和尾部，成为以太网的帧；链路管理，主要是通过CSMA/CD协议来实现；数据编码与译码，是一种常用的的二元码线路编码方式之一，被物理层使用来编码一个同步位流的时钟和数据。

​![image](image-20230823102859-bug8gza.png)​

3、分类上的区别

网关分为传输网关、应用网关、信令网关、中继网关、协议网关等。应用网关在应用层上进行协议转换。传输网关用于在2个网络间建立传输连接。利用传输网关，不同网络上的主机间可以建立起跨越多个网络的、级联的、点对点的传输连接。

网卡支持的计算机种类分为标准以太网卡和PCMCIA网卡。标准以太网卡用于台式计算机联网，而PCMCIA网卡用于笔记本电脑。按网卡所支持的总线类型可以分为ISA、EISA、PCI等。

七. DNS服务器  
　　参考：《 DNS原理及其解析过程》

　　域名与DNS

　　我们访问一个网站的时候，往往使用的是域名（相对IP来说更加语义清晰、更加容易记忆，例如 www.baidu.com)。

　　域名是由一串用点分隔的名字组成的，通常包含组织名，而且始终包括两到三个字母的后缀，以指明组织的类型或该域所在的国家或地区。

　　然而计算机之间的通信网络通信是通过IP进行的， 因此需要将域名解析为对应的IP，DNS就是进行域名解析的服务器。

DNS 维护着 域名(domain name)和IP地址 (IP address)的对照表表，以解析消息的域名。

　　DNS 查询的过程如下图所示

​![image](image-20230823102909-u9vurir.png)​

DNS 维护着 域名(domain name)和IP地址 (IP address)的对照表表，以解析消息的域名。

1、在浏览器中输入www.qq.com域名，操作系统会先检查自己本地的hosts文件是否有这个网址映射关系，如果有，就先调用这个IP地址映射，完成域名解析。

2、如果hosts里没有这个域名的映射，则查找本地DNS解析器缓存，是否有这个网址映射关系，如果有，直接返回，完成域名解析。

3、如果hosts与本地DNS解析器缓存都没有相应的网址映射关系，首先会找TCP/ip参数中设置的首选DNS服务器，在此我们叫它本地DNS服务器，此服务器收到查询时，如果要查询的域名，包含在本地配置区域资源中，则返回解析结果给客户机，完成域名解析，此解析具有权威性。

4、如果要查询的域名，不由本地DNS服务器区域解析，但该服务器已缓存了此网址映射关系，则调用这个IP地址映射，完成域名解析，此解析不具有权威性。

5、如果本地DNS服务器本地区域文件与缓存解析都失效，则根据本地DNS服务器的设置（是否设置转发器）进行查询，如果未用转发模式，本地DNS就把请求发至13台根DNS，根DNS服务器收到请求后会判断这个域名(.com)是谁来授权管理，并会返回一个负责该顶级域名服务器的一个IP。本地DNS服务器收到IP信息后，将会联系负责.com域的这台服务器。这台负责.com域的服务器收到请求后，如果自己无法解析，它就会找一个管理.com域的下一级DNS服务器地址(qq.com)给本地DNS服务器。当本地DNS服务器收到这个地址后，就会找qq.com域服务器，重复上面的动作，进行查询，直至找到www.qq.com主机。

6、如果用的是转发模式，此DNS服务器就会把请求转发至上一级DNS服务器，由上一级服务器进行解析，上一级服务器如果不能解析，或找根DNS或把转请求转至上上级，以此循环。不管是本地DNS服务器用是是转发，还是根提示，最后都是把结果返回给本地DNS服务器，由此DNS服务器再返回给客户机。

八. 附录  
　　未在文中说到的相关知识点：

　　《 IP地址分类（A/B/C/D/E/F类）》

　　《 计算机网络七层结构模型： 开放式系统互联通信参考模型（ 简称为 OSI模型）》

　　《 TCP/IP 协议族》

　　《 ARP协议的简明工作流程》

　　《 ARP协议处理详细过程-交换机工作原理-及广播风暴问题分析》

　　《 交换机、路由器、网关的概念以及用途》

　　《 交换机与路由器的区别，以及2层-7层交换机的区别》

　　工具：

　　《 eNSP(Enterprise Network Simulation Platform) 》

一款由华为提供的免费的、可扩展的、图形化操作的网络仿真工具平台，主要对企业网路由器、交换机进行软件仿真，完美呈现真实设备实景，支持大型网络模拟，让广大用户有机会在没有真实设备的情况下能够模拟演练，学习网络技术。

---

‍
