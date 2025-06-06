# PHP 调用dll

![](spacer[1]-20220901155445-4bteohr.gif)​

# COM

(no version information, might be only in CVS)

COM -- COM 类

## 大纲

|

```
$obj = new COM("server.object")
```

||
| --|

## 描述

COM 类提供了一个将 (D)COM 组件整合到 PHP 脚本中的框架。

## 方法

string **COM::COM** ( string module_name [, string server_name [, int codepage]] )

COM 类构造函数。参数：

module_name被请求组件的名字或 class-id。

server_nameDCOM 服务器的名字，组件在此服务器上被取用。如果是 **NULL**，则假定是 localhost。想要允许 DCOM，必须将 php.ini 中的 **com.allow_dcom** 设为 **TRUE**。

codepage指定用于将 PHP 字符串（php-strings）转换成 UNICODE 字符串（unicode-strings）的代码页，反之亦然。可用的值为 **CP_ACP**、**CP_MACCP**、**CP_OEMCP**、**CP_SYMBOL**、**CP_THREAD_ACP**, **CP_UTF7** 和 **CP_UTF8**。

**例子 1. COM 示例 （1）**

|`<font color="#000000">// 启动 word$word = new COM("word.application") or die("Unable to instanciate Word");print "Loaded Word, version {$word->Version}\n";//将其置前$word->Visible = 1;//打开一个空文档$word->Documents->Add();//随便做些事情$word->Selection->TypeText("This is a test...");$word->Documents[1]->SaveAs("Useless test.doc");//关闭 word$word->Quit();//释放对象$word->Release();$word = null;</font>`|
| --|

**例子 2. COM 示例 （2）**

|`<font color="#000000">$conn = new COM("ADODB.Connection") or die("Cannot start ADO");$conn->Open("Provider=SQLOLEDB; Data Source=localhost;Initial Catalog=database; User ID=user; Password=password");$rs = $conn->Execute("SELECT * FROM sometable");    // 记录集$num_columns = $rs->Fields->Count();echo $num_columns . "\n";for ($i=0; $i < $num_columns; $i++){    $fld[$i] = $rs->Fields($i);}$rowcount = 0;while (!$rs->EOF){    for ($i=0; $i < $num_columns; $i++)    {        echo $fld[$i]->value . "\t";    }    echo "\n";    $rowcount++;            // rowcount 自增    $rs->MoveNext();}$rs->Close();$conn->Close();$rs->Release();$conn->Release();$rs = null;$conn = null;</font>`|
| --|
