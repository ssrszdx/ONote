# PHP魔术函数、魔术常量、预定义常量

 **一、魔术函数** **（13个）**

1、__construct()  
实例化对象时被调用， 当__construct和以类名为函数名的函数同时存在时，__construct将被调用，另一个不被调用。

2、__destruct()  
当删除一个对象或对象操作终止时被调用。

3、__call()  
对象调用某个方法， 若方法存在，则直接调用；若不存在，则会去调用__call函数。

4、__get()  
读取一个对象的属性时，若属性存在，则直接返回属性值； 若不存在，则会调用__get函数。

5、__set()  
设置一个对象的属性时， 若属性存在，则直接赋值；  
若不存在，则会调用__set函数。

6、__toString()  
打印一个对象的时被调用。如echo $obj;或print$obj;

7、__clone()  
克隆对象时被调用。如：$t=new Test();$t1=clone $t;

8、__sleep()  
serialize之前被调用。若对象比较大，想删减一点东东再序列化，可考虑一下此函数。

9、__wakeup()  
unserialize时被调用，做些对象的初始化工作。

10、__isset()  
检测一个对象的属性是否存在时被调用。如：isset($c->name)。

11、__unset()  
unset一个对象的属性时被调用。如：unset($c->name)。

12、__set_state()  
调用var_export时，被调用。用__set_state的返回值做为var_export的返回值。

13、__autoload()  
实例化一个对象时，如果对应的类不存在，则该方法被调用。

**举例说明**

1、__get() 当试图读取一个并不存在的属性的时候被调用。  
如果试图读取一个对象并不存在的属性的时候，PHP就会给出错误信息。如果在类里添加__get方法，并且我们可以用这个函数实现类似java中反射的各种操作。

class Test  
{  
     public function __get($key)  
    {  
         echo $key . " 不存在";  
    }  
}

$t = new Test();  
echo $t->name;  
就会输出：name 不存在

2、__set() 当试图向一个并不存在的属性写入值的时候被调用。

class Test  
{  
    public function __set($key,$value)  
    {  
         echo '对' . $key . &quot;附值&quot; .$value;  
    }  
}

$t = new Test();  
$t->name = "aninggo";

就会输出：对 name 附值 aninggo

3、__call() 当试图调用一个对象并不存在的方法时，调用该方法。

class Test  
{  
    public function __call($Key,$Args)  
    {  
         echo "您要调用的 {$Key} 方法不存在。你传入的参数是：&quot; . print_r($Args, true);  
    }  
}

$t = new Test();  
$t->getName(aning, go);

程序将会输出：  
您要调用的 getName 方法不存在。参数是：Array  
(  
     [0] => aning  
     [1] => go  
)

4、__toString() 当打印一个对象的时候被调用，这个方法类似于java的toString方法，当我们直接打印对象的时候回调用这个函数。

class Test  
{  
     public function __toString()  
     {  
         return "打印 Test";  
     }  
}

$t = new Test();  
echo $t;

运行echo $t;的时候，就会调用$t->__toString();从而程序将会输出：打印 Test;

5、__clone() 当对象被克隆时，被调用。

class Test  
{  
     public function __clone()  
     {  
         echo "我被复制了！";  
     }  
}

$t = new Test();  
$t1 = clone$t;

程序输出：我被复制了！

 **二、魔术常量** （8个）

1、__LINE__  
返回文件中的当前行号。

2、__FILE__  
返回文件的完整路径和文件名。如果用在包含文件中，则返回包含文件名。自 PHP 4.0.2 起，__FILE__ 总是包含一个绝对路径，而在此之前的版本有时会包含一个相对路径。

3、__DIR__  
文件所在的目录。如果用在被包括文件中，则返回被包括的文件所在的目录。它等价于 dirname(__FILE__)。除非是根目录，否则目录中名不包括末尾的斜杠。（PHP 5.3.0中新增）

4、__FUNCTION__  
返回函数名称（PHP 4.3.0 新加）。自 PHP 5 起本常量返回该函数被定义时的名字（区分大小写）。在 PHP 4 中该值总是小写字母的。

5、__CLASS__  
返回类的名称（PHP 4.3.0 新加）。自 PHP 5 起本常量返回该类被定义时的名字（区分大小写）。在 PHP 4 中该值总是小写字母的。

6、__TRAIT__  
Trait 的名字（PHP 5.4.0 新加）。自 PHP 5.4 起此常量返回 trait 被定义时的名字（区分大小写）。Trait 名包括其被声明的作用区域（例如 Foo\Bar）。

7、__METHOD__  
返回类的方法名（PHP 5.0.0 新加）。返回该方法被定义时的名字（区分大小写）。 格式：类名::方法名

8、__NAMESPACE__  
当前命名空间的名称（区分大小写）。此常量是在编译时定义的（PHP 5.3.0 新增）

**三、预定义常量**

PHP_VERSION                    PHP 程序的版本，如4.0.2  
PHP_OS                             执行PHP解释器的操作系统名称，如Windows  
PHP_SAPI                          用来判断是使用命令行还是浏览器执行的，如果 PHP_SAPI=='cli' 表示是在命令行下执行

E_ERROR                          最近的错误处  
E_WARNING                      最近的警告处  
E_PARSE                           剖析语法有潜在问题处  
E_NOTICE                         发生不寻常但不一定是错误处

PHP_EOL                           系统换行符，Windows是（\r\n），Linux是（/n），MAC是（\r），自 PHP 4.3.10 和 PHP 5.0.2 起可用  
DIRECTORY_SEPARATOR   系统目录分隔符，Windows是反斜线（\），Linux是斜线（/）  
PATH_SEPARATOR             多路径间分隔符，Windows是反斜线（;），Linux是斜线（:）

PHP_INT_MAX                    INT最大值，32位平台时值为2147483647，自 PHP 4.4.0 和 PHP 5.0.5 起可用  
PHP_INT_SIZE                   INT字长，32位平台时值为4（4字节），自 PHP 4.4.0 和 PHP 5.0.5 起可用

 **四、PHP运行环境检测函数php_sapi_name()**

该函数返回一个描述PHP与WEB服务器接口的小写字符串。

返回描述 PHP 所使用的接口类型（the Server API, SAPI）的小写字符串。  
例如，CLI 的 PHP 下这个字符串会是 "cli"，Apache 下可能会有几个不同的值，取决于具体使用的 SAPI。  
以下列出了可能的值：  
aolserver、apache、 apache2filter、apache2handler、 caudium、cgi （直到 PHP 5.3）, cgi-fcgi、cli、 continuity、embed、 isapi、litespeed、 milter、nsapi、 phttpd、pi3web、roxen、 thttpd、tux 和 webjames。

SAPI: 服务器端API，貌似和CGI是一个东西。每个服务器提供的API可能不同，但是他们都提供了CGI。  
        所以可以理解CGI是每个服务器都应该有的SAPI。apache有自己的SAPI，IIS也有自己的。但是php能在这些不同的服务器端工作，因为php支持了它们各自的SAPI。  
PHP-CLI: php命令行接口，php可以工作在这种模式下也可以CGI模式。是SAPI的一种，它和CGI提供的功能差不多。

参考：http://php.net/manual/zh/reserved.constants.php  
         http://hi.baidu.com/sy_wj/item/08c20d0b63ba29833d42e2f4

来源： [[http://www.cnblogs.com/adforce/archive/2011/09/13/2174928.html](http://www.cnblogs.com/adforce/archive/2011/09/13/2174928.html)](%5Bhttp://www.cnblogs.com/adforce/archive/2011/09/13/2174928.html%5D(http://www.cnblogs.com/adforce/archive/2011/09/13/2174928.html))
