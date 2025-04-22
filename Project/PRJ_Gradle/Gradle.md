# Gradle

‍
> **Gradle**是一个基于[Apache Ant](https://link.zhihu.com/?target=http%3A//zh.wikipedia.org/wiki/Apache_Ant)和[Apache Maven](https://link.zhihu.com/?target=http%3A//zh.wikipedia.org/wiki/Apache_Maven)概念的项目[自动化建构](https://link.zhihu.com/?target=http%3A//zh.wikipedia.org/wiki/%25E8%2587%25AA%25E5%258B%2595%25E5%258C%2596%25E5%25BB%25BA%25E6%25A7%258B)工具。它使用一种基于[Groovy](https://link.zhihu.com/?target=http%3A//zh.wikipedia.org/wiki/Groovy)的[特定领域语言](https://link.zhihu.com/?target=http%3A//zh.wikipedia.org/w/index.php%3Ftitle%3D%25E7%2589%25B9%25E5%25AE%259A%25E9%25A2%2586%25E5%259F%259F%25E8%25AF%25AD%25E8%25A8%2580%26action%3Dedit%26redlink%3D1)来声明项目设置，而不是传统的[XML](https://link.zhihu.com/?target=http%3A//zh.wikipedia.org/wiki/XML)。当前其支持的语言限于[Java](https://link.zhihu.com/?target=http%3A//zh.wikipedia.org/wiki/Java)、[Groovy](https://link.zhihu.com/?target=http%3A//zh.wikipedia.org/wiki/Groovy)和[Scala](https://link.zhihu.com/?target=http%3A//zh.wikipedia.org/wiki/Scala)，计划未来将支持更多的语言。

上面是维基上对Gradle的解释,相信一个没有接触过构建的人是不大能看明白的,当初我也是.下面是我对Gradle**通俗**的理解:

软件开发讲究代码复用,通过复用可以使工程更易维护,代码量更少..... 开发者可以通过继承,组合,函数模块等实现不同程度上的代码复用.但不知你有没有想过,软件开发也是一种工程作业,绝不仅仅是写代码,还涉及到工程的各种管理(依赖,打包,部署,发布,各种渠道的差异管理.....),你每天都在build,clean,签名,打包,发布,有没有想过这种过程,也可以像代码一样被描述出来, 也可以被复用.

**举个例子**

我是做Android开发的,你可知道国内有n个Android市场,n个手机品牌,n个手机尺寸......,一般公司都会针对不同的市场单独发包用来统计不同渠道的下载量等情况,可能需要针对不同(品牌,尺寸等各种硬件信息)的手机做一些特殊的处理,这个时候你可以针对不同的情况单独建一个工程,或者更好一点你可以通过一些变量来控制,像这样:

if(isMoto){do something}  
else if(isHuawei){do something}  
...

**差异管理**

但这两种解决方法都有自己的缺点,特别是前一种有极大的代码重复.后一种稍微好一点,但这种方式的差异是运行时的,不是静态的,对于moto手机上的处理逻辑对华为手机来说一点作用也没有,但这一段针对moto手机的处理逻辑也被装到了华为手机上了,通过gradle的productFlavor与buildtype可以实现静态级的差异控制可以参考[如何通过Gradle实现一套代码开发不同特性的APK · ByGhui](https://link.zhihu.com/?target=http%3A//ghui.me/post/2015/03/create-several-variants/)

说到前面的多渠道问题,不同的渠道一般会对应不同的渠道号,你当然可以通过修改一次打一个包这种纯手工的方式来生成你的多渠道包,但据听说国内某团购网站的Android App有100多个渠道.这里出现了什么?重复,反复的去打包而且这些包之前的差异很小(只是渠道号不同),和写代码一样我们应该复用,通过Gradle可以实现一个命令打出所有的渠道包,一个命令打出指定的渠道包.再复杂一点,你可能需要不同的渠道对应不同的签名文件,不同的icon,不同的服务器地址...这些都可以通过Gradle来方便的实现.

**依赖管理:**

做软件开发你可能需要依赖各种不同的jar,library.你当然可以通过将.jar/library工程下载到本地再copy到你的工程中,但不知你是否听说过国外有个叫**中央仓库**的东西,在这个仓库里你可以找到所有你能想到以及你从来没听说过的jar,aar...[The Central Repository Search Engine](https://link.zhihu.com/?target=http%3A//search.maven.org/) 这里可以找到所有你需要的依赖,而你需要的只是指定一个坐标,如下:

![](0.3447175028572458-20220206110639-iwx9pie.png)​

剩下的依赖的寻找,下载,添加到classpath等你都不需要去关心,通过这种方式来维护依赖的好处有以下几点:

剩下的依赖的寻找,下载,添加到classpath等你都不需要去关心,通过这种方式来维护依赖的好处有以下几点:

1. 依赖不会进入到你的版本控制仓库中(默认会缓存到~/.gradle/下)
2. 方便卸载装载依赖(只是一条坐标依赖,不需要删除即可)
3. 方便的版本管理,如上图中的2.3.3既是picasso的版本号,若改为+就表示从中央仓库中下载最新的版本
4. 不同工程的相同依赖不会存在重复副本(只在~/.gradle下存在一份)

**项目部署**

这方面我没怎么接触过,但据我所知通过一些插件,可以实现自动将你的输出(.jar,.apk,.war...)上传到指定仓库,自动部署...

罗哩罗嗦说了这么多,不知大家有没有理解

**总结一下:**

1. Gradle ***是一种构建工具*** ,它可以帮你管理项目中的差异,依赖,编译,打包,部署......,你可以定义满足自己需要的构建逻辑,写入到build.gradle中供日后复用.
2. Gradle ***不是一种编程语言*** ,它不能帮你实现软件中的任何实际功能

通俗的解释肯定是不严谨的解释,不妥之处欢迎讨论.
