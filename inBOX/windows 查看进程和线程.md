# windows 查看进程和线程

# 

pslist是用命令行查看进程/线程；[ProcessExplorer](https://www.baidu.com/s?wd=ProcessExplorer&tn=24004469_oem_dg&rsv_dl=gh_pl_sl_csd)是图形化的查看进程/线程。

pslist v1.29下载地址：[http://technet.microsoft.com/en-us/sysinternals/bb896682.aspx](http://technet.microsoft.com/en-us/sysinternals/bb896682.aspx) ,内附帮助文档。

ProcessExplorer v15.11 下载地址：[http://technet.microsoft.com/en-us/sysinternals/bb896653](http://technet.microsoft.com/en-us/sysinternals/bb896653) ，内附帮助文档。

====================================================================================================================================

**1.查看进程：利用window自带的工具（命令）  tasklist 或者是**pslist**工具。下面以****pslist命令为例，****  
**

C:> **pslist -t**

**Name                             Pid** Pri**Thd  Hnd      VM      WS    Priv
Idle                               0   0   2    0       0      28       0
  System                           4   8  69 1222    1824     308       0
    smss                         832  11   3   20    3748     408     172
      csrss                      900  13  12  807   72428   16152    2568
      winlogon                   924  13  21  516   61272    4704    8536
        services                 968   9  15  280   22556    4516    1868
          avp                    256   8  36 7185  190528   22332   50308
explorer                        2060   8  16  575  122880   13400   17752
  msnmsgr                       1604   8  33  778  222560   19240   32792
  cmd                           3680   8   1   31   31084    3004    2164
    pslist                      5476  13   2   91   30500    2744    1236
  notepad                       4276   8   1   45   33692    3956    1344
  IEXPLORE                      5184   8  61 2143  403392   31236  105436
  eclipse                       6088   8   1   33   29884    3184     960
    javaw                       4484   8  40 1197  729124  139424  193496
      javaw                     4252**   8 11(十一个线程)  310  187820    8080   13908

**2.查看进程中的线程                      **                                                   [http://technet.microsoft.com/en-us/sysinternals/bb896682.aspx](http://technet.microsoft.com/en-us/sysinternals/bb896682.aspx)

**C:&gt;**​**pslist -dmx 4252                                                                     //dmx是pslist命令的参数**

**Name**                Pid      VM      WS    Priv Priv Pk   Faults   NonP Page  
**javaw**              4252  202224   21848   23968   24476     7927      4   47

 **Tid** Pri    Cswtch            State     User Time   Kernel Time   Elapsed Time  
5428   8      2617     Wait:UserReq  0:00:01.312   0:00:00.515    0:06:41.625  
5312  15       614     Wait:UserReq  0:00:00.078   0:00:00.000    0:06:41.484  
1380  15         7     Wait:UserReq  0:00:00.000   0:00:00.000    0:06:41.468  
2012  10         7     Wait:UserReq  0:00:00.000   0:00:00.000    0:06:41.468  
3876   9      1037     Wait:UserReq  0:00:00.046   0:00:00.187    0:06:41.187  
5884   9        65     Wait:UserReq  0:00:00.000   0:00:00.015    0:06:41.187  
4444  10       236     Wait:UserReq  0:00:00.000   0:00:00.015    0:06:41.171  
4564  15        12     Wait:UserReq  0:00:00.000   0:00:00.000    0:06:40.953  
4644  15       270     Wait:UserReq  0:00:00.234   0:00:00.015    0:06:40.953  
4292   8         5     Wait:UserReq  0:00:00.000   0:00:00.000    0:06:40.953  
5964  15      6422   Wait:DelayExec  0:00:00.000   0:00:00.000    0:06:40.937

====================================================================================================================================

以下是非Windows系统本身附带的外部命令，均存放于光盘 PE 系统目录下的 System32 目录下。

AUTORAMRESIZER.EXE 自动根据物理内存调整[虚拟盘](https://www.baidu.com/s?wd=%E8%99%9A%E6%8B%9F%E7%9B%98&tn=24004469_oem_dg&rsv_dl=gh_pl_sl_csd)大小（PE）。  
CHOICE.EXE DOS 选择命令支持扩展  
DEVCON.EXE 设备控制台命令行工具  
FINDPASS.EXE 查找系统管理员口令的命令行工具（可能有病毒虚警）  
FPORT.EXE TCP/IP与端口检测工具  
HWPnp.exe 重新检测即插即用硬件的实用工具，可激活移动存储器等  
KEYBOARD.EXE 更改键盘区域属性的命令行工具  
KEYDOWN.EXE 检测键盘按键的命令行工具  
NC.EXE 大名鼎鼎的网络强力命令行工具！  
NETCFG.EXE Windows PE 环境的网络配置命令行工具  
PASSWDRENEW.EXE Windows 口令离线修改工具  
PENETCFG.EXE 牛人编写的 PE 网络环境配置工具  
PSINFO.EXE 本地和远程系统信息检测命令行工具  
PSKILL.EXE 结束本地或远程进程的命令行工具  
PSLIST.EXE 系统进程查看工具  
PSPASSWD.EXE 更改本地或远程系统口令命令行工具  
PSSERVICE.EXE 管理系统服务的命令行工具  
PULIST.EXE 系统进程列表查看  
TCPVCON.EXE 查看活动进程的TCP连接状态  
TFTPD32.EXE 简单的 TFTP 工具  
WGET.EXE 功能强大的命令行下载工具  
XCACLS.EXE 文件及目录访问控制列表的命令行加强工具  
XNVIEW.EXE 袖珍图像查看工具（小巧够用，不需升级）

另外参见：

http://blog.chinaunix.net/uid-116213-id-81617.html

http://blog.itpub.net/519536/viewspace-611379/

原文地址：[http://blog.csdn.net/haiross/article/details/14450373](http://blog.csdn.net/haiross/article/details/14450373)
