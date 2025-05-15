# zotero使用

1. **中国知网下载全文**

我们在之前介绍**Zotero**导入文献时，提到**Zotero**支持直接从中国知网导入文献，但导入时只能导入题录而不能同时下载CAJ或PDF全文。如需在导入题录的同时下载全文，可通过更新CNKI.js来实现。

CNKI.js下载地址：

[https://](https://link.zhihu.com/?target=https%3A//github.com/zotero/translators/blob/master/CNKI.js)​[**github.com/zotero/trans**](https://link.zhihu.com/?target=https%3A//github.com/zotero/translators/blob/master/CNKI.js)​[lators/blob/master/CNKI.js](https://link.zhihu.com/?target=https%3A//github.com/zotero/translators/blob/master/CNKI.js)

​![](v2-35a47e45ffbd0acc18207406e77167e7_1440w.webp.png)​

将下载的CNKI.js复制粘贴至**Zotero**的translator目录中，根据自己设定的目录找到，如利刃君这里目录为E：/Zotero/translator，选择替换原来的CNKI.js即可。为保险起见，可将原来的CNKI.js备份，如有问题可以替换回来。

替换完成后，在浏览器**Zotero**插件上点击右键，选择“**选项-&gt;Advanced**”，然后在“**Translator**”面板点击“**Update Translators**”。

​![](v2-35462820b3f5459ef8cff10f499ec2ba_1440w.webp.png)​

随后在有中国知网权限的情况下，通过**Zotero**浏览器插件导入中国知网的文献时，就会同时下载全文PDF了。
