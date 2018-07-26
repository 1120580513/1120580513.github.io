# MSSQL-TSQL

标签（空格分隔）： MSSQL TSQL

---
##BULK 行集提供程序([官方文档](https://docs.microsoft.com/zh-cn/sql/relational-databases/import-export/use-a-format-file-to-bulk-import-data-sql-server?view=sql-server-2014))
```sql
SELECT * FROM OPENROWSET(BULK 'C:\WeekSpace\a.txt',FORMATFILE = 'C:\WeekSpace\a.fmt') AS x
```
`a.txt：`
```txt
a,b,c,d
x,y,z,c
q,w,e,r
u,i,y,p
```
`a.fmt：`
```fmt
9.0
4
1 SQLCHAR 0 100 "," 1 Col1 ""
2 SQLCHAR 0 100 "," 2 Col2 SQL_Latin1_General_CP1_CI_AS
3 SQLCHAR 0 100 "," 3 Col3 SQL_Latin1_General_CP1_CI_AS
4 SQLCHAR 0 100 "\r\n" 4 Col4 SQL_Latin1_General_CP1_CI_AS
```

##FOR XML
```sql
SELECT * FROM (VALUES(1)) AS t(n) FOR XML PATH('')
```

##临时表
```sql
--局部临时表
SELECT * INTO #t FROM (VALUES(1,2,3),(4,5,6)) AS t(a,b,c)
SELECT * FROM #t
--全局临时表
SELECT * INTO ##t2 FROM (VALUES(1,2,3),(4,5,6)) AS t(a,b,c)
SELECT * FROM ##t2
```

##游标
```sql
DECLARE c CURSOR FAST_FORWARD FOR SELECT * FROM (VALUES(1),(2),(3),(4)) AS t(n)--创建并填充游标
OPEN c--打开游标
DECLARE @i INT
FETCH NEXT FROM c INTO @i--读取一个值给@i
WHILE(@@FETCH_STATUS = 0)--循环游标
BEGIN
	PRINT @i
	FETCH NEXT FROM c INTO @i
END
CLOSE c--关闭
DEALLOCATE c--删除游标
```

##用户定义函数
```sql
----------标量函数
CREATE FUNCTION dbo.fn_sum(
@nbr INT
) RETURNS INT
AS
BEGIN
    RETURN @nbr * 2
END
GO 
PRINT dbo.fn_sum(3)
----------表值函数
CREATE FUNCTION dbo.fn_rang(
@nbr INT
) RETURNS TABLE
AS
RETURN 
WITH a AS(
SELECT 1 n
UNION ALL
SELECT 1 n FROM a
),t AS(
SELECT TOP 100 ROW_NUMBER() OVER(ORDER BY n) AS n FROM a
)
SELECT * FROM t WHERE n <= @nbr
GO
SELECT * FROM dbo.fn_rang(10)
```

##存储过程
```sql
CREATE PROCEDURE dbo.proc_print(
@nbr INT 
)AS
PRINT @nbr
GO
CREATE PROCEDURE dbo.#proc_print(
@nbr INT 
)AS
PRINT @nbr
GO
CREATE PROCEDURE dbo.##proc_print(
@nbr INT 
)AS
PRINT @nbr
GO
CREATE PROCEDURE dbo.proc_select(
@nbr INT,
@outnbr INT OUTPUT 
)AS
BEGIN
    SET @outnbr = @nbr
	SELECT @nbr
END
GO
EXEC dbo.proc_print 1
EXEC dbo.#proc_print 1
EXEC dbo.##proc_print 1
GO
DECLARE @i INT 
EXEC dbo.proc_select 1,@i OUTPUT
SELECT @i
```

##触发器
##事务
##错误处理
##Service Broker