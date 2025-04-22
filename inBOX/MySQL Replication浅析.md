# MySQL Replication浅析

# MySQL Replication浅析

    MySQL Replication是MySQL非常出色的一个功能，该功能将一个MySQL实例中的数据复制到另一个MySQL实例中。整个过程是异步进行的，但由于其高效的性能设计，复制的延时非常小。MySQL复制功能在实际的应用场景中被广泛的应用于保证数据系统数据的安全性和可扩展设计中。

# 1、MySQL Replication功能的意义

   互联网应用系统中，一个设计恰当的WEB应用服务器在绝大多数情况下都是无状态的（Session除外，Session共享可通过WEB容器解决），故WEB应用服务器的扩展和集群相对简单。但数据库的集群和复制就不那么容易了。各个数据库厂商也一直在努力使自己的产品能够像WEB应用服务器一样能够方便的复制和集群。

   MySQLReplication的出现使我们能够非常方便将某一数据库中的数据复制到多台服务器中，从而实现数据备份、主从热备、数据库集群等功能。这样有效的提高了数据库的处理能力，提高了数据安全性等。

# 2、MySQLReplication实现原理

   MySQL的复制（replication）是一个异步的复制，从一个MySQLinstace（称之为Master）复制到另一个MySQLinstance（称之Slave）。整个复制操作主要由三个进程完成的，其中两个进程在Slave（Sql进程和IO进程），另外一个进程在Master（IO进程）上。

   要实施复制，首先必须打开Master端的binarylog（bin-log）功能，否则无法实现。因为整个复制过程实际上就是Slave从Master端获取该日志然后再在自己身上完全顺序的执行日志中所记录的各种操作。复制的基本过程如下：

   （1）Slave上面的IO进程连接上Master，并请求从指定日志文件的指定位置（或者从最开始的日志）之后的日志内容；

   （2）Master接收到来自Slave的IO进程的请求后，通过负责复制的IO进程根据请求信息读取指定日志指定位置之后的日志信息，返回给Slave的IO进程。返回信息中除了日志所包含的信息之外，还包括本次返回的信息已经到Master端的bin-log文件的名称以及bin-log的位置；

   （3）Slave的IO进程接收到信息后，将接收到的日志内容依次添加到Slave端的relay-log文件的最末端，并将读取到的Master端的bin-log的文件名和位置记录到master-info文件中，以便在下一次读取的时候能够清楚的告诉Master“我需要从某个bin-log的某个位置开始往后的日志内容，请发给我”；

   （4）Slave的Sql进程检测到relay-log中新增加了内容后，会马上解析relay-log的内容成为在Master端真实执行时候的那些可执行的内容，并在自身执行。

# 3、复制实现级别

   MySQL的复制有三种模式：Statement Level、Row Level、Mixed Level。复制级别的不同，会导致Master端二进制日志文件的生成形式的不同。

## 3.1 Statement Level复制

   该模式是最早的复制模式，主要的流程是Master端将每一条会修改数据的Query记录下来，Slave端在复制的时候会根据二进制文件重新执行相同的Query。这种模式的优点是Master端不需要记录每一行数据的变化，二进制日志文件量小，IO成本低，速度快。

   相应的，该模式存在的缺点如下：由于记录的是执行语句，就需要额外的知道每条语句执行的上下文信息，以保证该相同的操作在Slave端执行时能够得到和Master同样的结果。但由于MySQL功能的不断增多，这种复制模式需要考虑的情况也就越来越多，出现bug的几率也就也大。从MySQL 5.0开始，MySQL复制解决了大量的之前版本中出现的无法复制或复制错误的问题，但随着MySQL的发展，这种挑战将会日趋严峻。

## 3.2 Row Level复制

   MySQL开发人员意识到Statement Level存在的问题，于5.1.5开始提供Row Level模式。该模式的主要流程是，MySQL二级制日志文件会将每一行数据修改都记录下来，然后在Slave端进行同样的修改。这种模式的优点是：日志文件不会将SQL语句执行的上下文记录下来，只是记录哪一条数据修改了，修改成什么样子了；这样做可以避免如某些特定情况下存储过程、trigger的调用和触发没有被正确执行等复制问题。

   同样，该模式也存在缺点：日质量的成倍增加。例如：执行alter table之类的语句的时候，由于表结构修改，每条记录都发生改变，那么该表每一条记录都会记录到日志中。这样就大增加了复制过程的IO成本，导致速度下降、性能下降。

## 3.3 Mixed Level复制

   MySQL从5.1.8开始，提供Mixed Level。该模式结合了之前两种模式的优点，规避了二者的缺点。在该模式下，MySQL会根据执行的每一条语句来区分记录日志文件的格式。举例说明，当涉及到复杂的存储过程时，采用Row Level，规避Statement Level存在的某些场景无法复制的问题；当涉及到Alter table等操作时，采用Statement Level来规避Row Level带来的日志量巨大的问题。

# 4、MySQL Replication详细配置

## 4.1 环境介绍

### 4.1.1 Master环境介绍

   1）操作系统：Ubuntu12.04 32位

   2）Mysql版本：5.5.40-0ubuntu0.12.04.1-log (Ubuntu)

   3）IP：192.168.245.140

### 4.1.2 Slave环境介绍：

   1）操作系统：Ubuntu12.04 32位

   2）Mysql版本：5.5.40-0ubuntu0.12.04.1-log (Ubuntu)

   3）IP：192.168.245.139

## 4.2 配置

### 4.2.1 Master配置

   1）my.cnf配置

```
#vi /etc/mysql/my.cnf
[mysqld]
log-bin=mysql-bin   //[必须]启用二进制日志
server-id=140       //[必须]服务器唯一ID，默认是1，一般取IP最后一段
```

   2）重启mysql

```
sudo /etc/init.d/mysql restart
```

   3）在主服务器上建立帐户并授权slave

```
#mysql –u root –p 123456 
mysql>GRANT REPLICATION SLAVE ON *.* to 'mysync'@'%' identified by '123456';
 //一般不用root帐号，“%”表示所有客户端都可能连，只要帐号，密码正确，此处可用具体客户端IP代替，如192.168.245.139，加强安全。
```

4. 登录mysql，查询master的状态

![复制代码](copycode-20220830221037-7a3l3n3.gif)[&quot;复制代码&quot;]("复制代码")

```
mysql>show master status;
+------------------+----------+--------------+------------------+
| File             | Position | Binlog_Do_DB | Binlog_Ignore_DB |
+------------------+----------+--------------+------------------+
| mysql-bin.000004 |      308 |              |                  |
+------------------+----------+--------------+------------------+
1 row in set (0.00 sec)
```

![复制代码](copycode-20220830221037-hbth8sc.gif)[&quot;复制代码&quot;]("复制代码")

   注：执行完此步骤后不要再操作主服务器MYSQL，防止主服务器状态值变化。

### 4.2.2 Slave配置

   1）my.cnf配置

```
#vi /etc/mysql/my.cnf
[mysqld]
log-bin=mysql-bin   //[必须]启用二进制日志
server-id=139       //[必须]服务器唯一ID，默认是1，一般取IP最后一段
```

   2）重启mysql

```
sudo /etc/init.d/mysql restart
```

   3）配置从服务器Slave：

```
mysql>change master to master_host='192.168.245.140',master_user='mysync',master_password='123456',master_log_file='mysql-bin.000004',master_log_pos=308;   //注意不要断开，“308”无单引号。M
ysql>start slave;    //启动从服务器复制功能
```

4. 检查从服务器复制功能状态：

![复制代码](copycode-20220830221037-bmxsa43.gif)[&quot;复制代码&quot;]("复制代码")

```
mysql> show slave status\G
*************************** 1. row ***************************
               Slave_IO_State: Waiting for master to send event
                  Master_Host: 192.168.245.140
                  Master_User: root
                  Master_Port: 3306
                Connect_Retry: 60
              Master_Log_File: mysql-bin.000003
          Read_Master_Log_Pos: 5669
               Relay_Log_File: mysqld-relay-bin.000002
                Relay_Log_Pos: 5482
        Relay_Master_Log_File: mysql-bin.000003
             Slave_IO_Running: Yes
            Slave_SQL_Running: Yes
              Replicate_Do_DB: 
          Replicate_Ignore_DB: 
           Replicate_Do_Table: 
       Replicate_Ignore_Table: 
      Replicate_Wild_Do_Table: 
  Replicate_Wild_Ignore_Table: 
                   Last_Errno: 0
                   Last_Error: 
                 Skip_Counter: 0
          Exec_Master_Log_Pos: 5669
              Relay_Log_Space: 5639
              Until_Condition: None
               Until_Log_File: 
                Until_Log_Pos: 0
           Master_SSL_Allowed: No
           Master_SSL_CA_File: 
           Master_SSL_CA_Path: 
              Master_SSL_Cert: 
            Master_SSL_Cipher: 
               Master_SSL_Key: 
        Seconds_Behind_Master: 0
Master_SSL_Verify_Server_Cert: No
                Last_IO_Errno: 0
                Last_IO_Error: 
               Last_SQL_Errno: 0
               Last_SQL_Error: 
  Replicate_Ignore_Server_Ids: 
             Master_Server_Id: 140
1 row in set (0.00 sec)
```

![复制代码](copycode-20220830221037-c5ccoi9.gif)[&quot;复制代码&quot;]("复制代码")

   注：Slave_IO及Slave_SQL进程必须正常运行，即YES状态，否则都是错误的状态。

## 4.3 主从服务器测试

   主服务器Mysql，建立数据库，并在这个库中建表插入一条数据，观看从库是否也增加了相应的数据库、数据表、数据。

# 5、MySQL Replication常用架构总结

## 5.1 主从备份架构设计

![](101308093466015-20220830221037-ugwemax.png)​

   简述：两台mysql服务器如果其中有一台mysql服务器挂掉后，另外一台能立马接替其进行工作。因此我们就必须保证两台mysql数据库的数据完全一样，而且当挂掉的那一台重新启动的话，不再会被客户端继被访问，而是会充当备机跟现在工作的mysql进行数据同步，一直到提供服务的那台挂掉后再接替其工作。如此周而复始的实现了mysql的高可用。注意：Slave不对外提供服务；Slave和Master在同一个局域网内，以此保证主从复制的速度和连接的稳定性。

## 5.2 主主备份架构设计A

![](101309051582430-20220830221037-s2mlmc6.png)​

   简述：Mysql主主备份架构A——两台服务器互为主备，即A写的数据可以同步到B中去，B写的数据可以同步到A中去。代理服务器负责对读写进行负载均衡。

   缺点：自增主键的冲突问题无法解决；写操作频繁时，会导致并发问题。

   适用场景：写操作不多；无自增主键；主、备机同时承担读写任务，节省机器，适用于机器紧张的场景。

## 5.3 主主备份架构设计B

![](101310306748938-20220830221037-j0i3abp.png)​

   Mysql主主备份架构B——两台服务器互为主备，即A写的数据可以同步到B中去，B写的数据可以同步到A中去。但是，应用服务器通过keepalived只向Master（即其中之一）进行写入操作，代理服务器负责对读操作进行负载均衡。

   缺点：需要额外解决读写分离的问题

   优点：不需要额外的脚本控制主备角色转换；数据一致性保证
