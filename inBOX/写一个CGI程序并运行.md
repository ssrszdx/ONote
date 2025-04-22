# 写一个CGI程序并运行

```
准备Linux和Apache
我在/var/www/cgi-bin/下建一个文件get.c
```

![复制代码](0.1157430048312238-20220901121924-0yzuu4q.png)[&quot;复制代码&quot;]("复制代码")

```
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
    char *data;
    long m,n;
    printf("Content-type: text/html\n\n");
    printf("<TITLE>Mult Result</TITLE>");
    printf("<H3>Mult Result</H3>");

    data = getenv("QUERY_STRING");
    if(data == NULL)
        printf("<P>Don't transfer data or transfer error");
    else if(sscanf(data,"m=%ld&n=%ld",&m,&n)!=2)
        printf("<P>Error, invalid format, data have to number");
    else
        printf("<P>%ld and %ld result: %ld", m, n, m * n);
        printf("<br><h>Thank you to use the zongshuai webserver</h1>");

    return 0;
}
```

![复制代码](0.5629395447409147-20220901121924-yi3d9sm.png)[&quot;复制代码&quot;]("复制代码")

然后编译

gcc -o get.cgi get.c

编译完后会生成一个get.cgi文件

然后我配置Apache，我是这么配置的(我的Apache是2.4.23版本)

将**LoadModule** **cgid_module modules/mod_cgid.so前面的#去掉**

**添加**addHandler **cgi-**script **.cgi .pl**

**然后配置虚拟空间**

![复制代码](0.6346243471396962-20220901121924-b9pqo7s.png)[&quot;复制代码&quot;]("复制代码")

```
<VirtualHost *:80>
        ServerName cgi.xxx.com
        ServerAdmin admin@xxx.com
        DocumentRoot /var/www/cgi-bin
        <Directory "/var/www/cgi-bin">
                 AllowOverride None
                 Options +ExecCGI -MultiViews +SymLinksIfOwnerMatch
                 Require all granted
         </Directory>
        ErrorLog "logs/error.cgi.log"
        CustomLog "/var/www/cgi.log" combined
</VirtualHost>
```

![复制代码](0.1626685747967438-20220901121924-rlikgv4.png)[&quot;复制代码&quot;]("复制代码")

然后重启Apache

在浏览器中输入cig.xxx.com/get.cgi?m=3&n=7

输出如下内容:

### Mult Result

3 and 7 result: 21  
Thank you to use the zongshuai webserver

---
