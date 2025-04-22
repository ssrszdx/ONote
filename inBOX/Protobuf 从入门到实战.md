# Protobuf 从入门到实战

## 简介

      从第一次接触Protobuf到实际使用已经有半年多，刚开始可能被它的名字所唬住，其实就它是一种轻便高效的数据格式，平台无关、语言无关、可扩展，可用于**通讯协议**和**数据存储**等领域。

## 

## 优点

* **平台无关，语言无关，可扩展；**
* **提供了友好的动态库，使用简单；**
* **解析速度快，比对应的XML快约20-100倍；**
* **序列化数据非常简洁、紧凑，与XML相比，其序列化之后的数据量约为1/3到1/10。**

## 使用详解

### 1、服务器安装

安装依赖的库：     autoconf automake libtool curl make g++ unzip

安装：

```
  1 $ ./autogen.sh
  2 $ ./configure
  3 $ make
  4 $ make check
  5 $ sudo make install
```

### 2、 安卓客户端安卓

               下载相应版本jar包即可[。（csdn上上传了nano版本的jar包和exe文件）](http://download.csdn.net/download/lsfzlj/10238698)

### 3、 项目实战

       首先举一个服务端和客户端按照Protobuf协议进行数据数据传输的例子，工作流程如下图：（图下方深色部分为服务端部分，上方浅色部分为客户端部分）

![PB协议工作流程 (2)](0.15059435010872835-20220830222201-0vrceyr.png "PB协议工作流程 (2)")[http://images2017.cnblogs.com/blog/918077/201802/918077-20180203201730859-1335036836.png](http://images2017.cnblogs.com/blog/918077/201802/918077-20180203201730859-1335036836.png)

1、服务端和客户端约定他们使用PB协议作为数据传输和存储的工具，并约定传输信息的字段，如下：里面定义支付传输的字段。

![复制代码](0.09019562597717345-20220830222201-70nh64c.png)[&quot;复制代码&quot;]("复制代码")

```
  1 syntax = "proto2";          // PB协议版本
  2 import "Common.proto";      // 引入Common.proto，位于Protobuf sdk中
  3 
  4 option optimize_for = LITE_RUNTIME;
  5 
  6 option java_package = "com.xxxx.entity.pb";    // 生成类的包名
  7 option java_outer_classname = "PayInfo";       // 生成类的类名
  8 
  9 message PayInfo{
 10 	required string payid = 1;             // 支付相关的字段信息
 11 	optional string goodinfo = 2;          // optional 为可选参数
 12 	required string prepayid = 3;          // required为必填参数
 13 	optional string mode = 4;
 14 	optional int  userid = 5;
 15 	repeated string  extra = 6;           // repeated 为数组
 16 }
```

![复制代码](0.9130351469106361-20220830222201-629dqdi.png)[&quot;复制代码&quot;]("复制代码")

2、通过Protobuf源码编译得到可运行程序（也可以在网上查找下载，注意PB协议的版本）。得到exe程序中，在windows环境下通过命令行窗口命令生成上述文件中定义的PayInfo.java文件。（protoc 为可执行的exe文件）

```
  1 protoc --java_out ./ ./PayInfo.proto
```

3、将生成PayInfo文件集成到项目代码中，同时需要引入 ProtoBuf的sdk（因为生成的PayInfo.Java类中引入了sdk中的类），可以在github上下载：[https://github.com/google/protobuf](https://github.com/google/protobuf)。

4、服务端通过支付信息初始化PayInfo类，并调用ProtoBufSDK中 com.google.protobuf.nano 类的 toByteArray()方法将PayInfo转化为字节数组，通过网络传输给客户端（可以进行加密和压缩操作，注意顺序）。

```
  1 public static final byte[] toByteArray(MessageNano msg) {
  2         byte[] result = new byte[msg.getSerializedSize()];
  3         toByteArray(msg, result, 0, result.length);
  4         return result;
  5     }
```

5、客户端拿到字节数据后，通过集成的PayInfo.java文件对字节数据解析成PayInfo对象（通过程序生成的java文件都会自动生成这个函数）。

```
  1 public static PayInfo parseFrom(byte[] data)
```

自此：客户端就顺利拿到了服务端发送的支付信息，可以通过他们调起支付宝或者微信客户端发起支付了。

由此可以看出ProtoBuf只是一种协议，一种存储数据的格式,对应上面生成的字节数据的格式，任何语言的程序都可以通过本地类和jar包将字节数据解析成对象（语言/平台无关）。

使用建议：

1、通过编译程序生成.java文件有不同的版本，建议使用nano版本（3.0之后的PB协议才发布该版本），这种版本生成的java文件方法数较少（没有set，get等函数），对APK的体积影响更小(四五个文件大前后相差80~90Kb，项目中后续作了替换)。命令：

```
  1 protoc_3.1.0.exe  --javanano_out ./ ./GetConfig.proto
```

2、不管用2.0还是3.0还是nano版本还是非精简版最终生成的字节数据文件是相同的，不影响前后端的交互。

## 与其它数据协议比较

     Protobuf 有如 XML，不过它更小、更快、也更简单。你可以定义自己的数据结构，然后使用代码生成器生成的代码来读写这个数据结构。Protobuf 语义更清晰，无需类似 XML 解析器的东西（因为 Protobuf 编译器会将 .proto 文件编译生成对应的数据访问类以对 Protobuf 数据进行序列化、反序列化操作）。

     和其它数据协议的比较如下图：

![image](0.06104539967729661-20220830222201-pw3n14y.png "image")[http://images2017.cnblogs.com/blog/918077/201802/918077-20180203210241125-2020580326.png](http://images2017.cnblogs.com/blog/918077/201802/918077-20180203210241125-2020580326.png)

### 总结

        作为开发者使用protobuf简单高效，至于里面具体如何实现深层次的东西我们还不如花点时间学一下数据结构。

参考：

[https://developers.google.com/protocol-buffers/](https://developers.google.com/protocol-buffers/ "https://developers.google.com/protocol-buffers/")

[https://www.ibm.com/developerworks/cn/linux/l-cn-gpb/](https://www.ibm.com/developerworks/cn/linux/l-cn-gpb/)

https://tech.meituan.com/serialization_vs_deserialization.html

梦想不是浮躁,而是沉淀和积累

来源： [https://www.cnblogs.com/NeilZhang/p/8410589.html](https://www.cnblogs.com/NeilZhang/p/8410589.html)
