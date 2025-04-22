# Spring boot 两种热部署方式 springloader 和 devtools

什么是热部署？
热部署，就是在应用正在运行的时候升级软件，却不需要重新启动应用。在平时编写代码的时候，你会发现我们只是简单把打印信息改变了，就需要重新部署，如果要改变这样的方式，就需要用到热部署 springloaded。
使用方式，在项目中的 pom.xml 中 plugin 里添加依赖：

**[html]** [view plain](https://blog.csdn.net/a78270528/article/details/77584881# "view plain") [copy](https://blog.csdn.net/a78270528/article/details/77584881# "copy")

1. **&lt;**dependencies**&gt;**
2. **&lt;!--springloaded  hot deploy --&gt;**
3. **&lt;**dependency**&gt;**
4. **&lt;**groupId**&gt;**org.springframework**&lt;/**groupId**&gt;**
5. **&lt;**artifactId**&gt;**springloaded**&lt;/**artifactId**&gt;**
6. **&lt;**version**&gt;**1.2.4.RELEASE**&lt;/**version**&gt;**
7. **&lt;/**dependency**&gt;**
8. **&lt;/**dependencies**&gt;**
9. **&lt;**executions**&gt;**
10. **&lt;**execution**&gt;**
11. **&lt;**goals**&gt;**
12. **&lt;**goal**&gt;**repackage**&lt;/**goal**&gt;**
13. **&lt;/**goals**&gt;**
14. **&lt;**configuration**&gt;**
15. **&lt;**classifier**&gt;**exec**&lt;/**classifier**&gt;**
16. **&lt;/**configuration**&gt;**
17. **&lt;/**execution**&gt;**
18. **&lt;/**executions**&gt;**

如果使用的 run as，Java application 的话，那么需要做如下处理：
把 spring-loader-1.2.4.RELEASE.jar 下载下来，放到项目的 lib 目录中，[点击下载](http://download.csdn.net/download/a78270528/9950708)：

CSDN 下载地址：http://download.csdn.net/download/a78270528/9950708

![](0.19303724223987717-20220205164603-3w5vlyw.png)

然后设置 MyEclipse 的 run 参数里 VM 参数，点击 Java Application-->HelloController-->Arguments。

![](0.31641194311470744-20220205164603-nwrx38w.png)

填写 springloaded-1.2.4.RELEASE.jar 路径“-javaagent:.\lib\springloaded-1.2.4.RELEASE.jar -noverify”。
![](0.3754767706985016-20220205164603-q0hjbxq.png)

点击 run 即可运行项目，再次修改 java 文件即时生效，我们修改方法的返回值，这样在 run as 的时候，也能进行热部署了。直接访问就可以得到修改后的结果了。
![](0.2970836534327277-20220205164603-teyeeq1.png)
这里要注意的是，使用 springloader 这种方式，这并不是所有的代码都能够热部署的。使用 devtools 这种方式可以实现大部分代码的热部署。

spring-boot-devtools 是一个为开发者服务的一个模块，其中最重要的功能就是自动应用代码更改到最新的 App 上面去。原理是在发现代码有更改之后，重新启动应用，但是速度比手动停止后再启动还要更快，更快指的不是节省出来的手工操作的时间。

其深层原理是使用了两个 ClassLoader，一个 Classloader 加载那些不会改变的类（第三方 Jar 包），另一个 ClassLoader 加载会更改的类，称为 restart ClassLoader
，这样在有代码更改的时候，原来的 restart ClassLoader 被丢弃，重新创建一个 restart ClassLoader，由于需要加载的类相比较少，所以实现了较快的重启时间（5 秒以内）。

使用方法：

**[html]** [view plain](https://blog.csdn.net/a78270528/article/details/77584881# "view plain") [copy](https://blog.csdn.net/a78270528/article/details/77584881# "copy")

1. **&lt;!-- spring boot devtools 依赖包. --&gt;**
2. **&lt;**dependency**&gt;**
3. **&lt;**groupId**&gt;**org.springframework.boot**&lt;/**groupId**&gt;**
4. **&lt;**artifactId**&gt;**spring-boot-devtools**&lt;/**artifactId**&gt;**
5. **&lt;**optional**&gt;**true**&lt;/**optional**&gt;**
6. **&lt;**scope**&gt;**true**&lt;/**scope**&gt;**
7. **&lt;/**dependency**&gt;**

把上面的 pom.xml 中的 pulgin 替换成下面的代码：

**[html]** [view plain](https://blog.csdn.net/a78270528/article/details/77584881# "view plain") [copy](https://blog.csdn.net/a78270528/article/details/77584881# "copy")

1. **&lt;!-- 构建节点--&gt;**
2. **&lt;**build**&gt;**
3. **&lt;**plugins**&gt;**
4. **&lt;!-- 这是 spring boot devtool plugin --&gt;**
5. **&lt;**plugin**&gt;**
6. **&lt;**groupId**&gt;**org.springframework.boot**&lt;/**groupId**&gt;**
7. **&lt;**artifactId**&gt;**spring-boot-maven-plugin**&lt;/**artifactId**&gt;**
8. **&lt;**configuration**&gt;**
9. **&lt;!--fork :  如果没有该项配置，肯呢个 devtools 不会起作用，即应用不会 restart --&gt;**
10. **&lt;**fork**&gt;**true**&lt;/**fork**&gt;**
11. **&lt;/**configuration**&gt;**
12. **&lt;/**plugin**&gt;**
13. **&lt;/**plugins**&gt;**
14. **&lt;/**build**&gt;**

配置完成，重新启动项目可以试着修改代码，热部署成功，方便手动重启的时间，也加快了开发速度。

说明：
1、devtools 会监听 classpath 下的文件变动，并且会立即重启应用（发生在保存时机），注意：因为其采用的虚拟机机制，该项重启是很快的。
2、devtools 可以实现页面热部署（即页面修改后会立即生效，这个可以直接在 application.properties 文件中配置 spring.thymeleaf.cache=false 来实现(这里注意不同的模板配置不一样)。

在修改以下代码都不需要重启服务器：修改类、配置文件、页面文件（原理是将 spring.thymeleaf.cache 设为 false）之后 ctrl+s：应用会重启。

如果不能使用的话，以下就几种常见的问题：
1、对应的 spring-boot 版本是否正确，这里使用的是 1.5.3 版本；
2、是否加入 plugin 以及属性 <fork>true</fork>
3、Eclipse Project 是否开启了 Build Automatically（开启自动编译的功能）。
4、如果设置 SpringApplication.setRegisterShutdownHook(false)，则自动重启将不起作用。

最后，这两种方式 springloader、devtools 只需要配置一种即可，建议使用 devtools，可以支持更多的代码热部署。
