# sqlserver事务的两种用法

事务要尽可能简短和放在一个批处理中，这样可以避免死锁，提升时间。

学习事务后有些心得分享，以财务转账为例子：

一、用存储过程的方式实现事务

打开MSSQL,执行以下代码:

create database aaaa  ---创建数据库

---

use aaaa  
create table bb     ----创建表

( ID int not null primary key,  --帐号  
  moneys money    --转账金额  
 )

---

  insert into bb values ('1','2000') --插入两条数据  
  insert into bb values ('2','3000')

---

use aaaa  
go

alter procedure mon  --创建存储过程，定义几个变量  
@toID int,    --接收转账的账户  
@fromID int , --转出自己的账户  
@momeys money --转账的金额  
as

begin tran --开始执行事务

   update bb set [moneys=moneys-@momeys](mailto:moneys=moneys-@momeys) where [ID=@fromID](mailto:ID=@fromID) --执行的第一个操作，转账出钱，减去转出的金额

   update bb set [moneys=moneys+@momeys](mailto:moneys=moneys+@momeys) where [ID=@toID](mailto:ID=@toID) --执行第二个操作，接受转账的金额，增加

   if @@error<>0 --判断如果两条语句有任何一条出现错误

      begin rollback tran --–开始执行事务的回滚，恢复的转账开始之前状态  
      return 0  
      end

   else   --如何两条都执行成功

      begin commit tran ---执行这个事务的操作  
      return 1  
      end  
commit tran

---

至此sqlserver的准备工作结束，接下来来看下c#是如何调用这个存储过程的：

 protected void Button1_Click1(object sender, EventArgs e)  
    {

       SqlConnection con = new SqlConnection(@"Data Source=.;database=aaaa;uid=sa;pwd=123"); //连接字符串

       SqlCommand cmd = new SqlCommand("mon", con); //调用存储过程

       cmd.CommandType = CommandType.StoredProcedure;

       con.Open();

       SqlParameter prar = new SqlParameter();//传递参数

       cmd.Parameters.AddWithValue("@fromID", 1);

       cmd.Parameters.AddWithValue("@toID", 2);

       cmd.Parameters.AddWithValue("@momeys", Convert.ToInt32(TextBox1.Text));

       cmd.Parameters.Add("@return", "").Direction = ParameterDirection.ReturnValue;//获取存储过程的返回值

       cmd.ExecuteNonQuery();

       string value = cmd.Parameters["@return"].Value.ToString();//把返回值赋值给value

       if (value == "1")  
       {

           Label1.Text = "添加成功";

       }

       else  
       {

           Label1.Text = "添加失败";

       }  
    }

二、用Asp.net的方式实现事务（不用存储过程的方式）

    protected void Button2_Click(object sender, EventArgs e)  
    {

       SqlConnection con = new SqlConnection(@"Data Source=.;database=aaaa;uid=sa;pwd=123");

       con.Open();

       SqlTransaction tran = con.BeginTransaction();//先实例SqlTransaction类，使用这个事务使用的是con 这个连接，使用BeginTransaction这个方法来开始执行这个事务

       SqlCommand cmd = new SqlCommand();

       cmd.Connection = con;

       cmd.Transaction = tran;

       try  
       {

           //在try{} 块里执行sqlcommand命令，

           cmd.CommandText = "update bb set moneys=moneys-'" + Convert.ToInt32(TextBox1.Text) + "' where ID='1'";

           cmd.ExecuteNonQuery();

           cmd.CommandText = "update bb set moneys=moneys+'" + Convert.ToInt32(TextBox1.Text) + "' where ID='2'";

           cmd.ExecuteNonQuery();

           tran.Commit();//如果两个sql命令都执行成功，则执行commit这个方法，执行这些操作

           Label1.Text = "添加成功";

       }

       catch  
       {

           Label1.Text = "添加失败";

           tran.Rollback();//如何执行不成功，发生异常，则执行rollback方法，回滚到事务操作开始之前；

       }

    }
