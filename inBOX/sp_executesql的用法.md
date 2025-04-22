# sp_executesql的用法

需求：表名是动态的，必须用exec来执行，然后在exec里边还得给变量动态赋值  这时候exec 就搞不定了

```
exec（'select @count=count(empid) from '+@tableName+' where proid='+@proid+' and id<'+@id+' and state!=4'）
```

下边这个代码如果去掉where后边的东东就是可以的

一：正确

```
set @sql=N'select @count=count(empid) from '+@tableName
exec sp_executesql @sql,N'@count   int   output ',@count   output 
select @count
```

二：错误

```
set @sql=N'select @count=count(empid) from '+@tableName+' where proid='+@proid+' and id<'+@id+' and state!=4'
exec sp_executesql @sql,N'@count   int   output ',@count   output 
select @count
```

后来去查msdn才知道要这样：[传送门](http://msdn.microsoft.com/zh-cn/library/ms188001(v=sql.90).aspx)

三：正确（我的例子包含表名，表名不可以直接和proid1一样 表名必须要用 ‘+表名+’ ，不加的我试过了报错）

![复制代码](ad4a6761-011b-47b7-b149-dfa2aec12b58-20220206181450-ji7b7h6.gif)[&quot;复制代码&quot;]("复制代码")

```
declare @count int,@tableName nvarchar(50),@SQLString nvarchar(max),@proid int,@id int,@ParmDefinition nvarchar(max);
set @tableName='table27';
set @proid=433;
set @id=159;
--set @sql=N'select @count=count(empid) from table27'
set @SQLString=N'select @countOUT=count(empid) from '+@tableName+' where proid=@proid1 and id<@id1 and state!=4';
set @ParmDefinition=N'@proid1 int,@id1 int,@countOUT   int   output';
exec sp_executesql @SQLString,@ParmDefinition,@proid1=@proid,@id1=@id,@countOUT=@count   output;
select  @count;
```

![复制代码](ad4a6761-011b-47b7-b149-dfa2aec12b58-20220206181450-smshsm4.gif)[&quot;复制代码&quot;]("复制代码")

官方的例子：

![复制代码](ad4a6761-011b-47b7-b149-dfa2aec12b58-20220206181450-03da5k3.gif)[&quot;复制代码&quot;]("复制代码")

```
DECLARE @IntVariable int;
DECLARE @SQLString nvarchar(500);
DECLARE @ParmDefinition nvarchar(500);
DECLARE @max_title varchar(30);

SET @IntVariable = 197;
SET @SQLString = N'SELECT @max_titleOUT = max(Title) 
   FROM AdventureWorks.HumanResources.Employee
   WHERE ManagerID = @level';
SET @ParmDefinition = N'@level tinyint, @max_titleOUT varchar(30) OUTPUT';

EXECUTE sp_executesql @SQLString, @ParmDefinition, @level = @IntVariable, @max_titleOUT=@max_title OUTPUT;
SELECT @max_title;
```

![复制代码](ad4a6761-011b-47b7-b149-dfa2aec12b58-20220206181450-jiezbob.gif)[&quot;复制代码&quot;]("复制代码")

MSDN的解释：

```
执行可以多次重复使用或动态生成的 Transact-SQL 语句或批处理。Transact-SQL 语句或批处理可以包含嵌入参数。
```

MSDN的语法：

```
sp_executesql [ @stmt = ] stmt
[
    {, [@params=] N'@parameter_name data_type [ OUT | OUTPUT ][,...n]' } 
     {, [ @param1 = ] 'value1' [ ,...n ] }
]
```

语法我看不懂啊 看不懂

看实例代码才看懂的有木有

总结：

![复制代码](ad4a6761-011b-47b7-b149-dfa2aec12b58-20220206181450-3omxrto.gif)[&quot;复制代码&quot;]("复制代码")

```
@sqlstring ：就是你要执行的sql语句字符串
@ParmDefinition： @sqlstring里边用到的参数在这里声明 输出的参数要加output  
sp_executesql： 
第一个参数sqlstring 就是执行的sql字符串了
第二个参数@ParmDefinition是@sqlstring里边用到的参数在这里声明 输出的参数要加output  
最后的参数加output的参数是输出的参数（需要和外部的相对应的变量建立关联）
中间的参数就是@sqlstring 里边用到的参数（需要和外部的相对应的变量建立关联）
最后你可以 select 输出的参数 来查询(select @count)
```
