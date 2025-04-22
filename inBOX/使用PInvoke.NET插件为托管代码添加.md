# 使用PInvoke.NET插件为托管代码添加

P/Invoke（Platform invoke）即在.NET Framework中调用Win32 API的过程。其中一个困难的地方就是确定要调用方法的签名（尤其是缺乏Win32编程经历的情况下），这经常会是一个反复尝试/出错的过程。向非托管Win32 API传递不正确的数据类型或值通常会导致内存泄漏或其它不希望的结果。    PInvoke.NET是一个wiki，用于建立正确的P/Invoke签名。wiki是一种任何人都可进行编辑的协作式网站，因此可以在那里找到数以千计的与使用P/Invoke相关的签名、示例和笔记。既然wiki可被任何人编辑，你也可以在使用这些信息的同时分享自己的经验。

    显然这个wiki及其包含的信息很有价值，而PInvoke.NET Visual Studio插件则使其更具价值。在下载、安装PInvoke.NET插件后，你就可以在Visual Studio内搜索需要的签名或向其添加新的内容。在VS内的代码文件内点击右键，会看到有两个新的菜单项：Insert PInvoke Signatures和Contribute PInvoke Signatures and Types。

    选择Insert PInvoke Signatures来插入一个新的PInvoke签名，这时会看到：  
 ![](https://www.cnblogs.com/images/cnblogs_com/anderslly/InsertPInvokeSignature.JPG)​

    使用这个简单的对话框，你可以搜索需要调用的函数。还有一个可选项，你可以包含该函数所在的模块。现在，应用程序的一个基本的能力是让计算机发出蜂鸣声。那么需要搜索一下Beep函数，看看它会出现什么。如果如下：  
 ![](https://www.cnblogs.com/images/cnblogs_com/anderslly/PInvokeSearchBeep.JPG)​

    在结果中会显示该函数用法的信息（Beep函数的信息是"Generates simple tones on the speaker"）。你可以看到供C#和VB.NET使用的签名。注意下面的文字Alternative Managed API，它给出了一个建议，告诉你.NET Framework 2.0中的System.Console.Beep方法具有相同功能（最好还是使用托管代码）。    对话框的底部还有一个链接，它会把你带向wiki中Beep方法的相应页面，这个页面包含了该方法的各个参数的相关文档以及一些代码示例。

    在选择了要插入的签名后，点击Insert按钮，该签名就被添加到代码中了。在Beep的示例中是这样的：

    [DllImport("kernel32.dll", SetLastError = true)]  
    [return: MarshalAs(UnmanagedType.Bool)]  
    static extern bool Beep(uint dwFreq, uint dwDuration);

    大功告成！现在就可以在代码中调用该方法了：  
    Beep(1223, 1000);

    PInvoke.NET wiki和Visual Studio插件为开发人员减少了很多痛苦，也节省了大量时间。wiki可以通过[www.pinvoke.net](http://www.pinvoke.net/)访问，在该页面的左下角的Helpful Tools链接中可以找到该插件的下载。

    PS：据说Beep函数还具有驱蚊之功效，不过需要知道相应的频率来设置第一个参数:)
