# extjs 4.2 目录结构

ExtJS 4.2-目录结构

一、文件结构

文件/文件夹名 作用

builds  压缩后的ExtJS代码,体积更小,更快

docs  开发文档

examples  官方演示示例

locale  多国语言资源文件

packages ExtJS各部分功能的打包文件

resource  ExtJS所需要的CSS与图片文件

src  未压缩的源代码目录

bootstarp.js  ExtJS库引导文件,可通过参数自动切换ext-all.js与ext-all-debug.js

ext-all.js  ExtJS核心库,需要引用

ext-all-debug.js  ExtJS核心库的调试版,调试时使用

注：EXTJS文件的区别：

ext-all.js：包含所有的EXTJS框架文件，已经混淆

ext-all-debug.js：包含所有的EXTJS框架文件，没有混淆

ext-all-dev.js：包含所有的EXTJS框架文件，没有混淆，且包含调试信息

ext.js：仅包含能让EXTJS运行的最小集合，已经混淆

ext-debug.js：仅包含能让EXTJS运行的最小集合，没有混淆

ext-dev.js：仅包含能让EXTJS运行的最小集合，没有混淆，且包含调试信息

在4.0的时候有bootstrap.js，4.2就没有了。但是sample中的include-ext.js应该是同样的功能

bootstrap.js：自动加载ext-all.js或者ext-all-debug.js，在以下情况下会加载ext-all-debug.js

1.当前主机名是本地

2.当前主机是使用IPV4地址

3.Current protocol is a file(当前的协议是一个文件)

4.其他情况下自动加载ext-all.js

    我们在进行Ext开发的时候只需将ext-all.js、ext-all-debug.js、bootstarp.js和locale文件夹里的ext-lang-zh_CN.js以及resource整个文件夹拷入项目的ExtJs4（名字随便起）文件夹里。

二、页面引用

1.引入Ext的ExtJs4/resources/css/ext-all.css 。

 2.引入ExtJS的核心库ExtJs4/ext-all.js或者ExtJs4/ext-all-debug.js 。

3.引入自己写的实现本页面功能的JS。
