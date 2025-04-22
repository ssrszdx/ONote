# idea maven下载包太慢了如何解决

在pom.xml中添加maven 依赖包时，我就发现不管是否用了FQ，下载速度都好慢，就1M的东西能下半天，很是苦恼，于是到网上搜资料，然后让我查到了。说是使用阿里的maven镜像就可以了。我于是亲自试了下，速度快的飞起！！！

操作步骤：

1、右键项目选中maven选项，然后选择“open settings.xml”或者 “create settings.xml”

2、然后把如下代码粘贴进去就可以了。重启IDE，感受速度飞起来的感觉吧！！！

<?xml version="1.0" encoding="UTF-8"?>

<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"  
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
          xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd">  
    <mirrors>  
        <!-- mirror  
         | Specifies a repository mirror site to use instead of a given repository. The repository that  
         | this mirror serves has an ID that matches the mirrorOf element of this mirror. IDs are used  
         | for inheritance and direct lookup purposes, and must be unique across the set of mirrors.  
         |  
        <mirror>  
          <id>mirrorId</id>  
          <mirrorOf>repositoryId</mirrorOf>  
          <name>Human Readable Name for this Mirror.</name>  
          <url>http://my.repository.com/repo/path</url>  
        </mirror>  
         -->

        <mirror>  
            <id>alimaven</id>  
            <name>aliyun maven</name>  
            <url>http://maven.aliyun.com/nexus/content/groups/public/</url>  
            <mirrorOf>central</mirrorOf>  
        </mirror>

        <mirror>  
            <id>uk</id>  
            <mirrorOf>central</mirrorOf>  
            <name>Human Readable Name for this Mirror.</name>  
            <url>http://uk.maven.org/maven2/</url>  
        </mirror>

        <mirror>  
            <id>CN</id>  
            <name>OSChina Central</name>  
            <url>http://maven.oschina.net/content/groups/public/</url>  
            <mirrorOf>central</mirrorOf>  
        </mirror>

        <mirror>  
            <id>nexus</id>  
            <name>internal nexus repository</name>  
            <!-- <url>http://192.168.1.100:8081/nexus/content/groups/public/</url>-->  
            <url>http://repo.maven.apache.org/maven2</url>  
            <mirrorOf>central</mirrorOf>  
        </mirror>

    </mirrors>  
</settings>
