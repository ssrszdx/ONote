相信很多小伙伴在使用 Zotero 的时候都对它提供的云空间多有诟病，毕竟只有300MB，确实是捉襟见肘。幸好，Zotero 支持 WebDAV，所以我们还可以使用 WebDAV 来同步我们的附件。但是在国内支持 WebDAV 的网盘少之又少，很多小伙伴应该都是选择了坚果云。但是坚果云的免费用户的流量并不多，而付费的话也不便宜。今天我们就要使用 OneDrive 来实现 Webdav 同步，再也不用担心 Zotero 的空间不够用了。

首先需要说明的是，OneDrive只提供5G的免费空间，并不算多。我们可以申请开发者订阅，获得免费的空间，但我不建议这样做，因为一旦数据丢失，对我们来说损失巨大。所以我建议花几十块钱，拼车一个Office365的订阅，就能获得正版的Office365和1T的OneDrive。

在你获得正版的OneDrive以后，你就可以开始接下来的步骤。由于OneDrive本身不支持WebDAV，所以无法直接与Zotero进行同步，但是我们可以借助一个工具，让OneDrive也可以实现WebDAV。接下来开始我们的教程

1、首先我们进入Koofr官网Welcome back - Koofr，注册一个Koofr账户。

![[Pasted image 20231103160658.png]]
2、进入Koofr后，我们就可以登陆OneDrive账户，这样我们就把OneDrive挂载到Koofr上了。

​![[Pasted image 20231103160737.png]]​

3、挂载完成后，点击头像，进入Preferences

​![[Pasted image 20231103160747.png]]
4、然后点击Password，再在如图所示的地方，输入应用名（可以任意输入，主要是为了提醒你这是被哪个app在使用），点击生成密码，复制这个密码。
![[Pasted image 20231103160803.png]]
​![[Pasted image 20231103160813.png]]

5、后面和使用坚果云类似，进入Zotero，点击编辑-->首选项-->同步，登陆Zotero账户以后，在下面的文件同步选择WebDAV，URL填写app.koofr.net/dav/OneDrive，注意：这里的“OneDrive”一定要和你的Koofr里面的目录名称对应。用户名就是你的Koofr账户，密码是上一步复制的密码。然后验证服务器，提示成功。

![[Pasted image 20231103160822.png]]

![[Pasted image 20231103160835.png]]
6、经过上面5个步骤，我们就将Zotero和OneDrive进行了同步，后面我们在使用的时候，就会直接使用OneDrive的空间。注意：最好不要在OneDrive客户端对Zotero文件夹的文件进行修改，容易造成同步错误，如果发现同步错误，可以在Zotero中重新同步。（在使用Zotero for ipad客户端的时候，可能会出现附件下载失败，这就是因为云上的文件与Zotero数据不同，导致同步出错，建议重置同步后再试试）

​![[Pasted image 20231103160846.png]]​

好了，到这里我们就完成了WebDAV同步，开始享受自己的超大存储空间吧！！！

OneDrive for Business----------修改成OneDrive

密码： zhvc tthh pf7t 0tmw dxpkuer@gmail.com