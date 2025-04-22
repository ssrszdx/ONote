# IIS服务无法启动的小实验之MachineKeys文件夹

[Windows Troubleshooting: could not start the IIS Admin Service - error code -2146893818 - TechNet Articles - United States (English) - TechNet Wiki (microsoft.com)](https://social.technet.microsoft.com/wiki/contents/articles/23797.windows-troubleshooting-could-not-start-the-iis-admin-service-error-code-2146893818.aspx)

windows server 2003 sp2 + IIS6.0 除了2003自动的1.1框架，没有安装任何.net框架。系统很纯净，因为是在虚拟机中做的实验。

# 实验步骤：

1. 观察安装完IIS之后的 ”C:\Documents and Settings\All Users\Application Data\Microsoft\Crypto\RSA\MachineKeys“ 文件夹：

​![](https://pic002.cnblogs.com/images/2012/344683/2012030121261218.png)​

iis安装之前是什么样忘了看了，失误：)

　　2. 删除以c2319开头的文件，记得要先备份一下，因为下面会发生让网管们抓狂的事情。

　　3. 停止服务IIS Admin Service ，再启动该服务，会发现IIS启动不了了。

​![](https://pic002.cnblogs.com/images/2012/344683/2012030121320298.png)​

错误代码：-2146893818。如果google一下，根本找不到解决方法，无非是让你恢复"C:\WINDOWS\system32\inetsrv\History"文件夹下的MetaBase文件。

　　4. 再去查看文件夹MachineKeys时，你会发现系统又重新创建了一个相同文件名的“c2319c42033a5ca7f44e731bfd3fa2b5_33858066-43b1-4245-82d9-bd2e4ee05dcd” 文件。

　　5. 恢复我们备份的“c2319c42033a5ca7f44e731bfd3fa2b5_33858066-43b1-4245-82d9-bd2e4ee05dcd” 文件，再次启动IIS Admin Service，**启动成功！**

# 分析与结论：

　　MachineKeys文件夹作为微软加密 [CryptoAPI](http://www.microsoft.com/mind/0697/crypto.asp) 接口的“Key Container”，扮演着重要的角色。关于加密解密的理论知识可以参考[此文](http://www.codeproject.com/Articles/10154/NET-Encryption-Simplified)。

至于那个神秘的“c2319c42033a5ca7f44e731bfd3fa2b5_33858066-43b1-4245-82d9-bd2e4ee05dcd” 文件，到底是不是所有的windows server 2003系统都相同呢？（估计是不同的，要不怎么叫MachineKey呢，肯定是与机器有关）在此做个备份：[文件下载](https://files.cnblogs.com/slmk/c2319c42033a5ca7f44e731bfd3fa2b5_33858066-43b1-4245-82d9-bd2e4ee05dcd.rar)

‍

‍

主要错误原因是因为'C:\Documents and Settings\All Users\Application Data\Microsoft\Crypto\RSA\MachineKeys' 文件夹下keys被破坏了，修改一下名称就可以了。  
该文件夹默认为隐藏，你可以直接在我的电脑里输入该路径。  
在文件夹下至少有两个文件，如下格式：  
c23***********************_MachineGUID7a4***********************_MachineGUID

当出现此IIS错误的时候，可能你能看见奇数个文件3个或5个或7个；按理说应该为复数2，4，6；所以问题就在这里了。文章来源地址https://www.yii666.com/blog/94676.html

解决方法：

1. 先从注册表中"regedit" 中找到该MachineGUID，注册表路径：HKEYLOCAL_MACHINE\SOFTWARE\Microsoft\Cryptography\MachineGUID
2. 再将此"C:\Documents and Settings\All Users\Application Data\Microsoft\Crypto\RSA\MachineKeys' 文件夹下的所有keys做个备份，以防不测，还可恢复。*文章地址https://www.yii666.com/blog/94676.html*
3. 然后成双成对地将“c23****_MachineGUID”和“7a4******_MachineGUID“文件名中"MachineGUID"替换成注册表中获取的key值.网址:yii666.com
4. 然后在服务里尝试启动IISAdmin, 这时你看见能启动了。恭喜你，修改成功了。网址:yii666.com<**文章来源地址:https://www.yii666.com/blog/94676.html**
5. 最后一步，启动IISAdmin后，请使用命令"iisreset"重置一下，否则网站不会启动，"iisreset"命令不会删除你的任何网站。
