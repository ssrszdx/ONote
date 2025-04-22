# SQL Server遍历表中记录的2种方法

SQL Server遍历表一般都要用到游标，SQL Server中可以很容易的用游标实现循环，实现SQL Server遍历表中记录。本文将介绍利用使用表变量和游标实现数据库中表的遍历。

## **表变量来实现表的遍历**

以下代码中，代码块之间的差异已经用灰色的背景标记。

![复制代码](copycode[1]-20220901154833-rimhvp2.gif)[&quot;复制代码&quot;]("复制代码")

```
 1 DECLARE @temp TABLE
 2 
 3     (
 4 
 5       [id] INT IDENTITY(1, 1) ,
 6 
 7       [Name] VARCHAR(10)
 8 
 9     )  
10 
11 DECLARE @tempId INT ,
12 
13     @tempName VARCHAR(10)  
14 
15 INSERT  INTO @temp
16 
17 VALUES  ( 'a' )  
18 
19 INSERT  INTO @temp
20 
21 VALUES  ( 'b' )  
22 
23 INSERT  INTO @temp
24 
25 VALUES  ( 'c' )  
26 
27 INSERT  INTO @temp
28 
29 VALUES  ( 'd' )  
30 
31 INSERT  INTO @temp
32 
33 VALUES  ( 'e' )  
34 
35 WHILE EXISTS ( SELECT   [id]
36 
37                FROM     @temp )
38 
39     BEGIN  
40 
41         SET ROWCOUNT 1   
42 
43         SELECT  @tempId = [id] ,
44 
45                 @tempName = [Name]
46 
47         FROM    @temp  
48 
49         SET ROWCOUNT 0  
50 
51 --delete from @temp where [id] = @tempId  
52 
53         PRINT 'Name:----' + @tempName  
54 
55     END 
```

![复制代码](copycode[1]-20220901154833-0ff2wyx.gif)[&quot;复制代码&quot;]("复制代码")

但是这种方法，必须借助[ROWCOUNT](http://msdn.microsoft.com/zh-cn/library/ms188774.aspx)。但是使用[ SET ROWCOUNT ](http://msdn.microsoft.com/zh-cn/library/ms188774.aspx)将可能会影响 DELETE、INSERT 和 UPDATE 语句。

所以修改上面WHILE循环，改用TOP来选出首条记录。

![复制代码](copycode[1]-20220901154833-t39wcgb.gif)[&quot;复制代码&quot;]("复制代码")

```
 1 WHILE EXISTS ( SELECT   [id]
 2 
 3                FROM     @temp )
 4 
 5     BEGIN  
 6 
 7         SELECT TOP 1
 8 
 9                 @tempId = [id] ,
10 
11                 @tempName = [Name]
12 
13         FROM    @temp  
14 
15         DELETE  FROM @temp
16 
17         WHERE   [id] = @tempId  
18 
19         SELECT  *
20 
21         FROM    @temp
22 
23   
24 
25         EXEC('drop table '+)
26 
27         PRINT 'Name:----' + @tempName  
28 
29     END 
```

![复制代码](copycode[1]-20220901154833-kj9iqnx.gif)[&quot;复制代码&quot;]("复制代码")

这种方法也存在一个问题，需要将遍历过的行删除，事实上，我们在实际应用中可能并不想要遍历完一行就删除一行。

## **利用游标来遍历表**

　　游标是非常邪恶的一种存在，使用游标经常会比使用面向集合的方法慢2-3倍，当游标定义在大数据量时，这个比例还会增加。如果可能，尽量使用while,子查询，临时表，函数，表变量等来替代游标，记住，游标永远只是你最后无奈之下的选择，而不是首选。

![复制代码](copycode[1]-20220901154833-mwuknda.gif)[&quot;复制代码&quot;]("复制代码")

```
 1 --定义表变量
 2 
 3 DECLARE @temp TABLE
 4 
 5     (
 6 
 7       [id] INT IDENTITY(1, 1) ,
 8 
 9       [Name] VARCHAR(10)
10 
11     )  
12 
13 DECLARE @tempId INT ,
14 
15     @tempName VARCHAR(10)  
16 
17 DECLARE test_Cursor CURSOR LOCAL FOR
18 
19 SELECT   [id],[name] FROM @temp
20 
21 --插入数据值
22 
23 INSERT  INTO @temp
24 
25 VALUES  ( 'a' )  
26 
27 INSERT  INTO @temp
28 
29 VALUES  ( 'b' )  
30 
31 INSERT  INTO @temp
32 
33 VALUES  ( 'c' )  
34 
35 INSERT  INTO @temp
36 
37 VALUES  ( 'd' )  
38 
39 INSERT  INTO @temp
40 
41 VALUES  ( 'e' )  
42 
43  
44 
45 --打开游标
46 
47 OPEN test_Cursor
48 
49 WHILE @@FETCH_STATUS = 0
50 
51     BEGIN  
52 
53         FETCH NEXT FROM test_Cursor INTO @tempId,@tempname
54 
55         PRINT 'Name:----' + @tempName  
56 
57     END 
58 
59 CLOSE test_Cursor
60 
61 DEALLOCATE test_Cursor
```
