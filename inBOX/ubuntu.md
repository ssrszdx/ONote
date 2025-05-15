1. ubuntu 版本 ubuntu-18.04.6-desktop-amd64

sudo apt install papirus-icon-theme 安装系统主题

sudo apt install papirus-icon-theme 安装主题

sudo apt install tweaks (调整屏幕分辨率)---------和虚拟机的拉伸有关系

sudo sh -c 'echo "deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic main restricted universe multiverse" > /etc/apt/sources.list'  
sudo sh -c 'echo "deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse" >> /etc/apt/sources.list'  
sudo sh -c 'echo "deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse" >> /etc/apt/sources.list'  
sudo sh -c 'echo "deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-security main restricted universe multiverse" >> /etc/apt/sources.list'  
sudo sh -c 'echo "deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse" >> /etc/apt/sources.list'  

--- 
**sudo vi /etc/apt/sources.list 修改源

默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy main restricted universe multiverse
deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-updates main restricted universe multiverse
deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-backports main restricted universe multiverse
deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-backports main restricted universe multiverse

deb http://security.ubuntu.com/ubuntu/ jammy-security main restricted universe multiverse
deb-src http://security.ubuntu.com/ubuntu/ jammy-security main restricted universe multiverse
预发布软件源，不建议启用
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-proposed main restricted universe multiverse
deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-proposed main restricted universe multiverse

**sudo apt-get update

sudo apt-get dist-upgrade -y
--- 


cd / 或者 cd ~ 回到根目录

cd命令：切换当前目录至其它目录，比如进入/etc目录，则执行 cd /etc  
cd /: 在linux 系统中斜杠“/”表示的是根目录。 cd / ,即进入根目录.  
cd ~命令是，进入用户在该系统的home目录

方法一：

echo "deb http://nginx.org/packages/mainline/ubuntu lsb_release -cs​​ nginx"  
| sudo tee /etc/apt/sources.list.d/nginx.list  
curl -o /tmp/nginx_signing.key https://nginx.org/keys/nginx_signing.key  
sudo mv /tmp/nginx_signing.key /etc/apt/trusted.gpg.d/nginx_signing.asc

sudo apt update  
sudo apt install nginx

方法二：

​sudo wget https://***.tar.gz​​

tar -zxvf filename.tar.gz -C /path/to/destination

./configure

make

make install

方法三：如果有 .sh文件，直接执行就可以

​install.sh​​

2. 安装docker
    
    sudo apt install docker.io
    

sudo docker --version

sudo docker run hello-world

sudo apt install docker.io

---

sudo docker pull nginx

docker run --name nginx -p 80:80 -d nginx​​

宿主机访问http://localhost:8090

sudo docker images 列出docker里面镜像------------- sudo docker rmi nginx

sudo docker ps 列出docker里面服务-----------sudo docker stop nginx 停止 sudo docker rm nginx

3. 安装nginx
    
    sudo apt-get install nginx
    
    sudo apt-get remove nginx
    
    sudo apt-get purge nginx
    
    sudo apt autoremove nginx
    
    安装最新版本 nginx
    
    遇见问题：安装pcre和zlib模块 ，下载安装
    
    http://www.zlib.net/
    
    sudo apt install yum --------------yum是另外一种安装方式 不用装
    
    sudo apt-get install build-essential -----------(Invalid C++ compiler or C++ compiler flags centos 用gcc++)
    
    sudo apt-get install gcc 安装gcc.
    
    查看nginx状态
    
    ​sudo systemctl status nginx​​​
    
    所有的 Nginx 配置文件都在/etc/nginx/​​​目录下
    
    主要的 Nginx 配置文件是/etc/nginx/nginx.conf​​​
    
    Nginx 日志文件(access.log 和 error.log)定位在/var/log/nginx/​​​目录下
    
4. 安装mysql
    
    sudo apt-get install mysql-server
    
    mysql -v
    
    进入到mysql界面修改root密码
    
    ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '123456';
    
    FLUSH PRIVILEGES;
    
    打开带密码的mysql
    
    sudo mysql -u root -p 再输入密码
    
    ---
    
    创建一个用户名密码：
    
    ​CREATE USER 'sa'@'%' IDENTIFIED BY '123456';​​​ (%所有IP， localhost 只允许当前IP连主机)
    
    ​GRANT ALL PRIVILEGES ON sa.* TO 'sa'@'%';​​​
    
    ​FLUSH PRIVILEGES;​​​
    
    ALL PRIVILEGES：这意味着您为用户授予了所有权限。  
    _._：这意味着您为用户分配了对所有数据库和所有表的权限。  
    'myuser'@'localhost'：这是您的新用户名和主机名。  
    WITH GRANT OPTION：这意味着该用户可以将其权限授予其他用户。
    
    exit 退出mysql
    
    打开mysql 远程连接
    
    > 3306端口是否打开？（检查mysql赋值是否为%）
    > 
    > netstat -ano | grep 3306 查看3306端口
    > 
    > ​sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf​​
    > 
    > 找到 “bind-address = 127.0.0.1” , 这一行要注释掉，只需在前面加个#，即 # bind-address = 127.0.0.1，或者改成0.0.0.0
    > 
    > 然后，重新启动, 启动方式：
    > 
    > - 重启mysql服务 sudo systemctl restart mysql
    > - ​/etc/init.d/mysql restart​
    > - sudo service mysql restart
    > 
    > 防火墙打开3306端口（可以先看一下防火墙的状态）
    > 
    > - 打开3306端口，命令如下：
    >     
    >     ```
    >     sudo ufw allow 3306 
    >     ```
    >     
    > - 开启防火墙，命令如下：
    >     
    >     ```
    >     sudo ufw enable 
    >     
    >     sudo ufw default deny
    >     ```
    >     
    > - 查看防火墙的状态：
    >     
    >     ```
    >     sudo ufw status
    >     ```
    >     
    

5. 安装nodejs

sudo apt-get install nodejs

sudo apt install npm

npm install -g cnpm --registry=https://registry.npm.taobao.org；

npm 默认仓库 [http://registry.npmjs.org](https://link.zhihu.com/?target=http%3A//registry.npmjs.org/)

### 第一种：

直接安装cnpm 安装淘宝提供的cnpm，并更改服务器地址为淘宝的国内地址， 命令：npm install -g cnpm --registry=https://registry.npm.taobao.org​​，以后安装直接采用cpm​​替代npm​​， 例如原生npm命令为：npm install uniq --save​​，cnpm命令为：cnpm install uniq --save​​

### 第二种：

替换npm仓库地址为淘宝镜像地址（推荐） 命令：npm config set registry https://registry.npm.taobao.org​​， 查看是否更改成功：npm config get registry​​，以后安装时，依然用npm命令，但是实际是从淘宝国内服务器下载的

nodejs 安装升级

安装

​sudo apt-get install nodejs​​

​sudo apt-get install npm​​

升级

​sudo npm install -g n​​

​sudo n stable # latest​​ #(升级node.js到最新版) stable #（升级node.js到最新稳定版）

​node -v​​

​npm -v​​

n是一个Node工具包，它提供了几个升级命令参数：

n 显示已安装的Node版本

n latest 安装最新版本Node

n stable 安装最新稳定版Node

n lts 安装最新长期维护版(lts)Node

n version 根据提供的版本号安装Node

</code></pre>

输出

提示了要重启终端或者执行 PATH="$PATH"​​

reset命令后更新为最新版本

卸载

​sudo npm uninstall npm -g​​

​sudo apt-get remove nodejs​​

或：

从Ubuntu系统上卸载NodeJS，运行 sudo apt-get remove nodejs​​ 该命令将删除软件包，但保留配置文件。

要删除软件包和配置文件，运行 sudo apt-get purge nodejs​​

最后，您可以运行以下命令以删除所有未使用的文件并释放磁盘空间 sudo apt-get autoremove​​

6. 安装redis
    
    sudo apt update
    
    安装  
    sudo apt install redis-server
    
    配置  
    sudo vim /etc/redis/redis.conf  
    找到 supervised 设置为 systemd  
    sudo service redis restart # 重新加载Redis服务文件  
    sudo systemctl status redis # 查看 Redis 的运行状态
    
    一些操作  
    sudo service redis start # 启动  
    sudo service redis stop # 关闭  
    sudo service redis restart # 重启  
    redis-cli # 客户端连接
    
    远程连接  
    sudo vi /etc/redis/redis.conf  
    将 bind 127.0.0.1 ::1 改为 bind 0.0.0.0  
    sudo service redis restart # 重启
    
    设置密码  
    sudo vi /etc/redis/redis.conf  
    设置：requirepass 自己的密码
    
    ---
    
    sudo apt install redis-server
    
    sudo chmod 755 redis.conf ( 否则redis配置文件为空)
    
    sudo vim redis.conf
    

---

redis-server -v

redis-cli

auth 密码

help keys @<group> ----------------redis命令

7. 安装vim
    
    sudo apt install vim
    

ESC 在输入和命令模式之间切换

！强制 w 保存 q 退出 i输入 / 搜索 （enter +n 下一个）

dd ——删除当前行 3dd ——从当前行算起，向下删除3行。D ——删除当前字符至行尾

yy ——复制当前行 yyp ——复制当前行并粘贴下一行

p------粘贴

G ——跳到最后一行，且光标定位到最后一个字符上 gg ——跳到第一行，且光标定位到都一个字符上，=1G

:79 第79行

​![image](assets/image-20230819145840-49fx7bq.png)​

8. 安装网络工具，查看ip ifconfig
    
    sudo apt install net-tools
    
9. 安装openssl
    
    ​sudo apt-get install openssl​​
    
    openssl version
    
    openssl help
    
10. 文件常用命令
    
    ls cd
    

11. 查看帮助文件
    
    Linux 的内建命令是 shell 程序的一部分，Linux 系统加载运行时就被加载并驻留在系统内存里的，因此执行速度较快。
    
    Linux 的外部命令是通过额外安装获得的命令，不随系统一起被加载到内容中，运行速度慢但功能强大。
    
    使用 type 命令可以查看该命令是内建命令还是外部命令
    
    ```powershell
    type <command>
    ```
    

info openssl 或者 man openssl

help ​<command>​​ -------只有内建命令能够使用下述形式的 help 命令查询帮助文档。

​<command> --help​​ （命令支持--help参数就能查看）

​<command> help​​ -h -? ?

tldr(too long didn't read)是一个比man更易读的命令行工具，可以快速的查看一个命令的常用方式和示例，比较适合刚开始学习命令行工具的新手

链接：[https://github.com/tldr-pages/tldr](https://bbs.pku.edu.cn/v2/jump-to.php?url=https%3A%2F%2Fgithub.com%2Ftldr-pages%2Ftldr)

​sudo pm install -g tldr​​

Then you have direct access to simplified, easy-to-read help for commands, such as tar​​, accessible through typing tldr tar​​ instead of the standard man tar​​.

12. 安装php
    
    sudo apt-get install php
    
    php -m 查看有哪些模块
    
    PHP在 5.3.3 之后已经把php-fpm并入到php的核心代码中了。 所以php-fpm不需要单独的下载安装。
    
    php扩展安装
    
    sudo apt install php-mysql
    
    sudo apt install php-curl
    
    sudo apt install php-dom
    
    sudo apt install php-redis
    
    php 需要安装Composer 包管理器 sudo apt install composer
    
    php如何配置nginx?-----需要先安裝 php-fp
    
    PHP CLI（Command Line Interface）和FPM（FastCGI Process Manager）都是PHP的运行模式，但它们之间有着明显的区别和适用场景。
    
    CLI模式是通过命令行来执行PHP脚本的一种运行方式，可以在命令行中直接执行PHP脚本并输出结果，适用于一些命令行工具、定时任务、后台进程等应用场景。CLI模式下，PHP没有Web服务器的限制，可以完全自由地调用系统的接口、读写文件、操作网络等。
    
    FPM模式则是在Web服务器中运行PHP的一种方式，将PHP解释器作为FastCGI进程与Web服务器（如Nginx、Apache）通信，处理请求并返回结果。FPM模式可以根据配置参数动态调整进程池中的进程数，充分利用服务器资源，同时支持慢请求处理、限流、多进程通信等功能，适用于高并发的Web应用。
    
    ```
    vim /etc/nginx/nginx.conf
    ```
    
    修改默认的 location 块，使其支持 .php 文件：
    
    ```
    location / {
        root   html;
        index  index.php index.html index.htm;
    }
    ```
    
    下一步配置来保证对于 .php 文件的请求将被传送到后端的 PHP-FPM 模块， 取消默认的 PHP 配置块的注释，并修改为下面的内容：
    
    ```
    location ~* \.php$ {
        fastcgi_index   index.php;
        fastcgi_pass    127.0.0.1:9000;
        include         fastcgi_params;
        fastcgi_param   SCRIPT_FILENAME    $document_root$fastcgi_script_name;
        fastcgi_param   SCRIPT_NAME        $fastcgi_script_name;
    }
    ```
    
    重启 Nginx。
    
13. 安装python
    
    ​sudo apt install python3 python3-pip​​​
    
    ​sudo apt install python-is-python3​​​
    
14. 安装GO语言
    
    sudo apt install golang
    
15. 安装ElasticSearch
    
16. linux命令
    
    tail 命令可用于查看文件的内容，有一个常用的参数 -f 常用于查阅正在改变的日志文件。
    
    Linux grep (global regular expression) 命令用于查找文件里符合条件的字符串或正则表达式。
    
    grep 指令用于查找内容包含指定的范本样式的文件，如果发现某文件的内容符合所指定的范本样式，预设 grep 指令会把含有范本样式的那一列显示出来。若不指定任何文件名称，或是所给予的文件名为 -，则 grep 指令会从标准输入设备读取数据。