# windows  curl 命令

> **阅读目录**
>
> * [一、windows 64 curl 命令的使用](#_label0)
>
>   * [1.https://blog.csdn.net/qq_27093465/article/details/53545693](#_label0_0)
>   * [2.Windows下elasticsearch插入数据报错！&quot;error&quot;:&quot;MapperParsingException[failed to parse]](#_label0_1)
>   * [3.https://blog.csdn.net/f7anty/article/details/49100409](#_label0_2)

[返回顶部](#_labelTop)

### windows 64 curl 命令的使用

#### https://blog.csdn.net/qq_27093465/article/details/53545693

curl命令可以通过命令行的方式，执行Http请求。在 Elasticsearch 中有使用的场景，因此这里研究下如何在windows下执行curl命令。

我提供我当时下载的，存放在某度云盘的压缩包。以防，官网不能用了呢，如下：

链接：http://pan.baidu.com/s/1bo7CyKJ 密码：jl8d

工具下载  
在官网处下载工具包：http://curl.haxx.se/download.html  
​![img-name1](20161209223210909-20220830215956-z3ofq6f.png)  
使用方式一：在curl.exe目录中使用  
解压下载后的压缩文件，通过cmd命令进入到curl.exe所在的目录。  
由于博主使用的是windows 64位 的系统，因此可以使用I386下的curl.exe工具。  
进入到该目录后，执行curl --help测试：  
​![img-name2](20161209223239692-20220830215956-rzuv3qw.png)  
测试命令，前提是你的es服务打开了：curl -XGET http://localhost:9200/_cluster/state/nodes?pretty  
​![img-name3](20161209223305755-20220830215956-09aqpcq.png)  
使用方式二：放置在system32中  
解压下载好的文件，拷贝I386/curl.exe文件到C:\Windows\System32  
然后就可以在DOS窗口中任意位置，使用curl命令了。  
​![img-name4](20161209223329646-20220830215956-vcp795x.png)  
使用方式三：配置环境变量  
在系统高级环境变量中，配置  
CURL_HOME ----- "你的curl目录位置\curl-7.43.0"  
path ---- 末尾添加 “;%CURL_HOME%\I386”  
这样与上面方式二的效果相同。  
​![img-name5](20161209223340318-20220830215956-cih15vy.png)  
这个我就没有测试了，因为上面的那个已经可以很方便使用这个命令了。这个就不麻烦了吧。

#### Windows下elasticsearch插入数据报错！"error":"MapperParsingException[failed to parse]

按照官方文档操作，但是windows下有些不同，它不认识单引号‘，因此如果这样操作，就会报错：

C:\Users\neusoft>curl localhost:9200/b1/b2/1 -d {"name":"fdafa"}  
{"error":"MapperParsingException[failed to parse]; nested: JsonParseException[Un  
recognized token ‘fdafa‘: was expecting ‘null‘, ‘true‘, ‘false‘ or NaN\n at [Sou  
rce: [B@1e6b986; line: 1, column: 13]]; ","status":400}  
　　此时，需要在{}周围添加双引号，json内部的双引号则转义

C:\Users\neusoft>curl localhost:9200/b1/b2/1 -d "{"name":"fdafa"}"  
{"_index":"b1","_type":"b2","_id":"1","_version":1,"created":true}  
　　这样操作就正常了！

在Linux下也会遇到同样的问题，有时候写的json也无法识别其中的参数，此时也需要经过转义才能使用。

#### https://blog.csdn.net/f7anty/article/details/49100409
