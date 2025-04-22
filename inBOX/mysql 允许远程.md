# mysql 允许远程

###### 打开CMD

###### 1.连接Mysql (连接方式：`mysql -u 你设置的用户名 -p你设置的密码<span> </span>`​)

###### 2.查看数据库：`show databases;`​我们会看到有一个叫做"mysql"的数据库，这里我们输入`use mysql<span> </span>`​进入’mysql’数据库中

​![在这里插入图片描述](https://img-blog.csdnimg.cn/20200313133642982.png)​

###### 3.执行`GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY '你设置的密码' WITH GRANT OPTION;`​

注意：mysql8.0及以上版本需要使用以下语句：

###### `create user root@'%' identified by '设置的密码';`​

###### `grant all privileges on *.* to root@'%' with grant option;`​

###### 4.刷新,`FLUSH PRIVILEGES；`​

###### 5.退出，此时就可以进行远程连接了。

注意：‘%’ 指所有ip都可远程连接此电脑。如果%变成ip 仅指定该ip可以远程连接至此电脑。

‍

select * from  user;  //验证
