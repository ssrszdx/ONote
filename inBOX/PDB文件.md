# PDB文件

# [PDB文件：每个开发人员都必须知道的](http://www.cnblogs.com/itech/archive/2011/08/15/2136522.html)

PDB Files: What Every Developer Must Know  
http://www.wintellect.com/CS/blogs/jrobbins/archive/2009/05/11/pdb-files-what-every-developer-must-know.aspx

PDB文件：每个开发人员都必须知道的

一 什么是PDB文件

大部分的开发人员应该都知道PDB文件是用来帮助软件的调试的。但是他究竟是如何工作的呢，我们可能并不熟悉。本文描述了PDB文件的存储和内容。同时还描 述了debugger如何找到binay相应的PDB文件，以及debugger如何找到与binay对应的源代码文件。本文适用于所有的Native和 Managed的开发人员。

在开始前，我们先定义2个术语：private build， 用来表示在开发人员自己机器上生成的build；public build，表示在公用的build机器上生成的build。private build相对来说比较简单，因为PDB和binay在相同的地方，通常地我们遇到的问题都是关于public build。

所有的的开发人员需要知道的最重要的事情是”PDB文件跟源代码同样的重要“， 没有PDB文件，你甚至不能debugging。对于public build，需要symbol server存储所有的PDB，然后当用户报告错误的时候，debugger才可以自动地找到binay相应的PDB文件， visual studio 和 windbg都知道如何访问symbol server。在将PDB和binay存储到symbol server前，还需要对PDB运行进行source indexing， source indexing的作用是将PDB和source关联起来。

接下来的部分假设有已经设置好了symbol server和source server indexing。TFS2010中可以很简单地完成对一个新的build的source indexing 和 symbol server copying。

二 PDB文件的内容

正式开始PDB的内容，PDB不是公开的文件格式，但是Microsoft提供了API来帮助从PDB中获取数据。

Native C++ PDB包含了如下的信息：

* public，private 和static函数地址；
* 全局变量的名字和地址；
* 参数和局部变量的名字和在堆栈的偏移量；
* class，structure 和数据的类型定义；
* Frame Pointer Omission 数据，用来在x86上的native堆栈的遍历；
* 源代码文件的名字和行数；

.NET PDB只包含了2部分信息:

* 源代码文件名字和行数；
* 和局部变量的名字；
* 所有的其他的数据都已经包含在了.NET Metadata中了；

三 PDB如何工作

当你加载一个模块到进程的地址空间的时候，debugger用2中信息来找到相应的PDB文件。第一个毫无疑问就是文件的名字，如果加载 zzz.dll,debugger则查找zzz.pdb文件。在文件名字相同的情况下debugger还通过嵌入到PDB和binay的GUID来确保 PDB和binay的真正的匹配。 所以即使没有任何的代码修改，昨天的binay和今天的PDB是不能匹配的。可以使用dempbin.exe来查看binary的GUID。

在VisualStudio中的modules窗口的symbol file列可以查看PDB的load顺序。第一个搜索的路径是binary所在的路径，如果不在binary所在的路径，则查找binary中hardcode记录的build目录，例如obj\debug*.pdb, 如果以上两个路径都没有找到PDB，则根据symbol server的设置，在本地的symbol server的cache中查找，如果在本地的symbol server的cache中没有对应的PDB，则最后才到远程的symbol server中查找。通过上面的查找顺序我们可以看出为什么public build和private build的PDB查找不会冲突。

对于private build有时我们需要在别人的机器上debug的情况，需要将相应的PDB与binary一起拷贝，对于加入GAC的.NET的binary，需要将PDB文件拷贝到C:\Windows\assembly\GAC_MSIL\Example\1.0.0.0__682bc775ff82796a类似的binary所在的目录。另一个变通的方法是定义环境变量DEVPATH，从而代替使用命令GACUTIL将binary放入GAC中。在定义DEVPATH后，只需要将binary和PDB放到DEVPATH的路径，在DEVPATH下的binary相当于在GAC下。使用DEVPATH，首先需要创建目录且对当前build用户有写权限，然后创建环境变量DEVPATH且值为刚才创建的目录，然后在web.config，app.config或machine.config中开启development模式，启动对DEVPATH的使用  
<configuration>  
   <runtime>  
      <developmentMode developerInstallation="true"/>  
   </runtime>  
</configuration>

在你打开了development模式后，如果DEVPATH没有定义或路径不存在的话会导致程序启动时异常"Invalid value for registry"。而且如果在machine.config中开启DEVPATH的使用会影响其他的所有的程序，所以要慎重使用machine.config。

最后开发人员需要知道的是源代码信息是如何存储在PDB文件中的。对于public builds，在运行source indexing tool后，版本控制工具将代码存储到你设置的代码cache中。对于private builds，只是存储了PDB文件的全路径，例如在c:\foo下的源文件mycode.cpp，在pdb文件中存储的路径为c:\foo\mycode.cpp。对于private builds可以使用虚拟盘来增加PDB对绝对路径的依赖，例如可以使用subst.exe将源代码路径挂载为V:，在别人的机器上debug的时候也挂载V：。

来源： [https://blog.csdn.net/thanklife/article/details/84259570](https://blog.csdn.net/thanklife/article/details/84259570)
