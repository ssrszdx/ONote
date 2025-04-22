# php調用dll

有时需要在php中利用到其他语言编写的dll类,PHP new COM方法

 1.　创建一个 C Class Library** ,命名为:HelloWorld **2.　打开项目的属性** ,在点选左边的 "Application"(就是第一个tab) , 然后点击Assembly Information 按钮 ,在弹出的Dialog中, 必须在底部勾上: Make assembly COM-visible !否则 , 这个dll将不能以COM方式访问 .(  也可以在代码中的类声明中写上[ComVisible(true)] , 效果一样,需要增加using System.Runtime.InteropServices;引用)

![PHP如何调用C#开发的dll类库](8414c589-fb7a-4d09-9e8f-dbf3b0074060-20220206184659-og4nqg9.jpg)

**3.　创建强命名签名文件并使用  
**　　使用vs.net的“Vsitual Studio .Net工具”-->Vistual Studio .Net命令提示符,输入 sn -k d:\HelloWorld.snk 回车即创建了强命名签名文件  
　　打开项目的属性,点选左边Signing 勾上Sign the assembly 在 Choose a strong name key file:处选择<Browse> 选择刚才创建的HelloWorld.snk文件

![PHP如何调用C#开发的dll类库](ed3b2065-9852-4bbb-9294-27fbe5ba3305-20220206184659-473p1vh.jpg)

**4.　创建类库并编译成dll  
**

代码如下:

namespace HelloWorld<br />{<br />    //[ComVisible(true)] //or check "Assembly COM-Visible" at Application-Assembly_Information dialog ;<br />    public class Hello<br />    {<br />        public string Write()<br />        {<br />            return "Hello World";<br />        }<br />    }<br />}

**5.　找到dll文件夹路径** ，然后使用vs.net的“Vsitual Studio .Net工具”-->Vistual Studio .Net命令提示符  
进入该dll文件夹下输入：  
代码如下:

regasm  HelloWorld.dll<回车>

这时这个.dll的.net程序集就变成一个标准的Com组件了,但是还不能用,必须让它变成全局Com组件.  
将程序集添加到全局程序集缓存中  
进入提示符窗口,输入：

代码如下:

gacutil /I HelloWorld.dll<回车>

这时这个dll就被复制到全局程序集缓存中了，无论在这个[电脑](http://www.jbxue.com/diannao/)的哪个硬盘上都可以使用此dll组件了.  
如果不进行强命名签名,这一步会提示加载失败

![PHP如何调用C#开发的dll类库](b97e08ee-5aac-4a23-9c91-0672e33a088a-20220206184659-yxz2ht3.jpg)

**PHP测试：  
**

代码如下:

<?php    
$r=new Com("HelloWorld.Hello");    
$s=$r->Write();    
echo $s;    
?>

‍
