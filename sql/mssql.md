# MSSQL

---

* [MSSQL](#mssql)
  * [系统数据库](#%E7%B3%BB%E7%BB%9F%E6%95%B0%E6%8D%AE%E5%BA%93)
    * [master](#master)
    * [msdb](#msdb)
  * [DateTime](#datetime)
    * [存储格式](#%E5%AD%98%E5%82%A8%E6%A0%BC%E5%BC%8F)
  * [DML](#dml)
    * [查询所有表和列和说明](#%E6%9F%A5%E8%AF%A2%E6%89%80%E6%9C%89%E8%A1%A8%E5%92%8C%E5%88%97%E5%92%8C%E8%AF%B4%E6%98%8E)
    * [临时表](#%E4%B8%B4%E6%97%B6%E8%A1%A8)
    * [开窗函数](#%E5%BC%80%E7%AA%97%E5%87%BD%E6%95%B0)
    * [CTE](#cte)
    * [Select Remote DB](#select-remote-db)
    * [BULK 行集提供程序(<a href="https://docs\.microsoft\.com/zh\-cn/sql/relational\-databases/import\-export/use\-a\-format\-file\-to\-bulk\-import\-data\-sql\-server?view=sql\-server\-2014" rel="nofollow">官方文档</a>)](#bulk-%E8%A1%8C%E9%9B%86%E6%8F%90%E4%BE%9B%E7%A8%8B%E5%BA%8F%E5%AE%98%E6%96%B9%E6%96%87%E6%A1%A3)
    * [FOR XML](#for-xml)
    * [阻塞与被阻塞的SQL脚本](#%E9%98%BB%E5%A1%9E%E4%B8%8E%E8%A2%AB%E9%98%BB%E5%A1%9E%E7%9A%84sql%E8%84%9A%E6%9C%AC)
    * [查询死锁](#%E6%9F%A5%E8%AF%A2%E6%AD%BB%E9%94%81)
  * [DDL](#ddl)
    * [CREATE](#create)
    * [BACKUP | DETACH | ATTACH \.\.\. : DATABASE](#backup--detach--attach---database)
  * [TABLE | VIEW | INDEX | UDT](#table--view--index--udt)
    * [CREATE TABLE](#create-table)
    * [TABLE | VIEW | INDEX | 权限 | UDT](#table--view--index--%E6%9D%83%E9%99%90--udt)
  * [T\-SQL](#t-sql)
    * [游标](#%E6%B8%B8%E6%A0%87)
    * [用户定义函数](#%E7%94%A8%E6%88%B7%E5%AE%9A%E4%B9%89%E5%87%BD%E6%95%B0)
    * [存储过程](#%E5%AD%98%E5%82%A8%E8%BF%87%E7%A8%8B)
    * [错误处理](#%E9%94%99%E8%AF%AF%E5%A4%84%E7%90%86)

## 系统数据库
### master
> 由系统表组成，**保存关于磁盘空间、文件分配和使用、系统层次的配置信息、端点登陆帐号的信息，当前实例的数据库信息和系统上其他SQL Server的存在信息**。
###model
> 该数据库是一相模板数据库，每**创建新数据库时，SQL Server会复制model数据库作为数据库的基础**。
###tempdb
> tempdb用来作为一个工作区，SQL Server每次启动都会重建该数据库。用来**保存临时表、工作表，维护快照隔离级别和某些其他操作的行版本，填充静态游标和键集游标也会用到该库**。
###mssqlsystemresource(resource)
> 该库是一个**隐藏的数据库**。可执行的系统对象(如系统存储过程和函数)。存放在数据目录，可手动复制mdf和ldf文件来查看其中的内容。
```sql
--复制resource库
CREATE DATABASE resource_copy ON (
NAME = data,
FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL12.MSSQLSERVER\MSSQL\Binn\mssqlsystemresource.mdf'
),(
NAME = log,
FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL12.MSSQLSERVER\MSSQL\Binn\mssqlsystemresource.ldf'
)
FOR ATTACH
```
### msdb
> **SQL Server代理服务、Service Broker会使用该库**。

---

## DateTime
### 存储格式
> **DateTime类型的毫秒格式匹配[0-9][0-9][039],994->993、996->997、999->000。**

---

## DML
```sql
SELECT * FROM sys.objects WHERE type = 'u'
SELECT * FROM sys.tables
SELECT * FROM sys.columns
SELECT * FROM sys.views
SELECT * FROM sys.procedures
SELECT * FROM sys.dm_exec_background_job_queue
-------------
SELECT * FROM sys.all_objects
SELECT * FROM sys.all_parameters
SELECT * FROM sys.all_columns
SELECT * FROM sys.all_views
SELECT * FROM sys.all_sql_modules
-------------
SELECT * FROM sys.sysprocesses--进程相关
SELECT * FROM sys.sysusers--用户相关
SELECT * FROM sys.dm_exec_connections--连接情况
SELECT session_id,status,login_name,login_time,* FROM sys.dm_exec_sessions--会话相关
SELECT sql_handle,* FROM sys.dm_exec_requests--请求相关
-------------
SELECT * FROM INFORMATION_SCHEMA.TABLES
SELECT * FROM INFORMATION_SCHEMA.VIEWS
SELECT * FROM INFORMATION_SCHEMA.COLUMNS
-------------
EXEC sys.sp_help
EXEC sys.sp_helpdb
EXEC sys.sp_helplogins
EXEC sys.sp_helpfile
```
### 查询所有表和列和说明
```sql
SELECT
表名 = case when a.colorder=1 then d.name else '' end,
字段序号 = a.colorder,
字段名 = a.name,
标识 = case when COLUMNPROPERTY( a.id,a.name,'IsIdentity')=1 then '√'else '' end,
主键 = case when exists(SELECT 1 FROM sysobjects where xtype='PK' and parent_obj=a.id and name in (
SELECT name FROM sysindexes WHERE indid in( SELECT indid FROM sysindexkeys WHERE id = a.id AND colid=a.colid))) then '√' else '' end,
类型 = b.name,
占用字节数 = a.length,
长度 = COLUMNPROPERTY(a.id,a.name,'PRECISION'),
小数位数 = isnull(COLUMNPROPERTY(a.id,a.name,'Scale'),0),
允许空 = case when a.isnullable=1 then '√'else '' end,
默认值 = isnull(e.text,''),
字段说明 = isnull(g.[value],'')
FROM
syscolumns a
left join
systypes b
on
a.xusertype=b.xusertype
inner join
sysobjects d
on
a.id=d.id and d.xtype='U' and d.name<>'dtproperties'
left join
syscomments e
on
a.cdefault=e.id
left join
sys.extended_properties g
on
a.id=G.major_id and a.colid=g.minor_id
left join
sys.extended_properties f
on
d.id=f.major_id and f.minor_id=0
where
d.name='FYQ_EstateCommissionRule' --如果只查询指定表,加上此红色where条件，tablename是要查询的表名；去除红色where条件查询说有的表信息
order by
a.id,a.colorder
```
### 临时表
```sql
--局部临时表
SELECT * INTO #t FROM (VALUES(1,2,3),(4,5,6)) AS t(a,b,c)
SELECT * FROM #t
--全局临时表
SELECT * INTO ##t2 FROM (VALUES(1,2,3),(4,5,6)) AS t(a,b,c)
SELECT * FROM ##t2
```
### 开窗函数
* **row_number()**：*增加一列,类似与增加伪列*
* **rank()**：*跳跃排序，如果有两个第一级时，接下来就是第三级。*
* **dense_rank()**：*连续排序，如果有两个第一级时，接下来仍然是第二级*
* **lag(colName,offset,defValue)**：*获取结果集中向前offset个的colName，如果超出则取defValue*
* **lead()**：*获取结果集中向后offset个的colName，如果超出则取defValue*
* `first_value()` `last_value()` `count()` `max()` `min()` `sum()` `avg()`

### CTE
```sql
WITH
  t1(v1, v2) AS (SELECT 1, 2),
  t2(w1, w2) AS (
    SELECT v1 * 2, v2 * 2
    FROM t1
  )
SELECT *
FROM t1, t2
```
---
```sql
WITH a AS(
SELECT * FROM (VALUES(1)) b(n)
UNION ALL
SELECT * FROM a
), t1 AS(--递归CTE(MSSQL最大为100)
SELECT TOP 100 * FROM a
),t2 AS(
SELECT 1 n FROM t1
CROSS JOIN t1 c
)
SELECT COUNT(1) FROM t2
```
### Select Remote DB
```sql
exec sp_addlinkedserver 'ITSV', ' ', 'SQLOLEDB', '远程服务器名或ip地址'
exec sp_addlinkedsrvlogin 'ITSV', 'false ',null, '用户名', '密码'
select * from ITSV.数据库名.dbo.表名
exec sp_dropserver 'ITSV ', 'droplogins'
```
### BULK 行集提供程序([官方文档](https://docs.microsoft.com/zh-cn/sql/relational-databases/import-export/use-a-format-file-to-bulk-import-data-sql-server?view=sql-server-2014))
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
### FOR XML
```sql
SELECT * FROM (VALUES(1)) AS t(n) FOR XML PATH('')
```
### 阻塞与被阻塞的SQL脚本
```sql
SELECT 
bl.spid blocking_session,bl.blocked blocked_session,st.text blockedtext,sb.text blockingtext
FROM(
	SELECT spid ,blocked
	FROM (SELECT * FROM sys.sysprocesses WHERE blocked>0 ) a
	WHERE NOT EXISTS(
		SELECT * FROM (SELECT * FROM sys.sysprocesses WHERE blocked>0 ) b WHERE a.blocked=spid)
	UNION
	SELECT spid,blocked FROM sys.sysprocesses WHERE blocked>0
) bl,(
    SELECT t.text ,c.session_id
    FROM sys.dm_exec_connections c
    CROSS APPLY sys.dm_exec_sql_text (c.most_recent_sql_handle) t
) st,(
    SELECT t.text ,c.session_id
    FROM sys.dm_exec_connections c
    CROSS APPLY sys.dm_exec_sql_text (c.most_recent_sql_handle) t
) sb
WHERE bl.blocked = st.session_id and bl.spid = sb.session_id
```
### 查询死锁
```sql
SELECT * FROM master..SysProcesses
WHERE db_Name(dbID) = '数据库名'
AND spId <> @@SpId
AND dbID <> 0
AND blocked >0;
```
[源](http://blog.itpub.net/29371470/viewspace-2128282)

## DDL
### CREATE
```sql
IF DB_ID('TEST') IS NOT NULL DROP DATABASE TEST
GO
CREATE DATABASE TEST
ON PRIMARY (**指定主文件组
NAME=TEST_DAT_MAIN1
,FILENAME='D:/SQL2008DATAS/TEST_DAT_MAIN1.MDF'
,SIZE=3MB
,MAXSIZE=UNLIMITED
,FILEGROWTH=5MB
),(
NAME=TEST_DAT_MAIN2
,FILENAME='D:/SQL2008DATAS/TEST_DAT_MAIN2.MDF'
,SIZE=3MB
,MAXSIZE=UNLIMITED
,FILEGROWTH=5MB
)
,FILEGROUP TEST_DAT_SUB1 DEFAULT (**指定用户文件组,DEFAULT：如果创建用户数据没有明确指定就放到这个文件
NAME=TEST_DAT_SUB1
,FILENAME='D:/SQL2008DATAS/TEST_DAT_SUB1.MDF'
,SIZE=2MB
,MAXSIZE=UNLIMITED
,FILEGROWTH=5MB
),(
NAME=TEST_DAT_SUB2
,FILENAME='D:/SQL2008DATAS/TEST_DAT_SUB2.MDF'
,SIZE=2MB
,MAXSIZE=UNLIMITED
,FILEGROWTH=5MB
)
,FILEGROUP TEST_DAT_SUB2 (
NAME=TEST_DAT_SUB3
,FILENAME='D:/SQL2008DATAS/TEST_DAT_SUB3.MDF'
,SIZE=2MB
,MAXSIZE=UNLIMITED
,FILEGROWTH=5MB
)
,FILEGROUP TEST_DAT_FILESTREAM1 CONTAINS FILESTREAM (
**CONTAINS FILESTREAM：指定文件组在文件系统中存储 FILESTREAM 二进制大型对象 (BLOB).FILESTREAM里面只能有一个文件
NAME=TEST_DAT_FILESTREAM1
,FILENAME='E:/SQL2008DATAS/TEST_DAT_FILESTREAM1.MDF'
)
,FILEGROUP TEST_DAT_FILESTREAM2 CONTAINS FILESTREAM (
NAME=TEST_DAT_FILESTREAM2
,FILENAME='E:/SQL2008DATAS/TEST_DAT_FILESTREAM2.MDF'
)
LOG ON (**日志
NAME=TEST_LOG1
,FILENAME='D:/SQL2008DATAS/TEST_LOG1.LDF'
,SIZE=10MB
,MAXSIZE=UNLIMITED
,FILEGROWTH=10MB
),(
NAME=TEST_LOG2
,FILENAME='D:/SQL2008DATAS/TEST_LOG2.LDF'
,SIZE=10MB
,MAXSIZE=UNLIMITED
,FILEGROWTH=10MB
)
COLLATE CHINESE_PRC_CI_AS
WITH
DB_CHAINING OFF**DB_CHAINING指定数据库可不可以为跨数据库所有权链的源或目标
,TRUSTWORTHY OFF**TRUSTWORTHY指定模拟上下文中的数据库模块能不能访问数据库以外的资源
```

### BACKUP | DETACH | ATTACH ... : DATABASE
```sql
*** 创建 备份数据的 device
USE master
EXEC sp_addumpdevice 'disk', 'testBack', 'c:\ \ mssql7backup\ \ MyNwind_1.
dat'
*** 开始 备份
BACKUP DATABASE pubs TO testBack
**分离
EXECUTE sp_detach_db ElmahLog
**附加
EXECUTE sp_attach_db 'C:/...'
**重命名
EXECUTE sp_renamedb 'old_name', 'new_name'
**删除
DROP DATABASE TestDB
```

## TABLE | VIEW | INDEX | UDT

### CREATE TABLE
```sql
USE [TripOrderDB]
GO
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
SET ANSI_PADDING ON
GO
IF EXISTS(SELECT 1 FROM sys.tables WHERE object_id=OBJECT_ID('Order'))
DROP TABLE Order
GO
CREATE TABLE [dbo].[Order](
[OrderID] [bigint] IDENTITY(1,1) NOT NULL,
[OrderStateID] [int] NOT NULL DEFAULT (0),
[Amount] [money] NULL DEFAULT (0) CHECK(Amount>=0),
[ProductID] [int] NOT NULL DEFAULT (0),
[OrderCreatedTime] [datetime] NOT NULL DEFAULT(GETDATE()),
CONSTRAINT [PK_Order] PRIMARY KEY CLUSTERED
(
[OrderID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
,CONSTRAINT [FK_Outer] FOREIGN KEY (OrderStatuId) REFERENCES OrderStatus(Id)
) ON [PRIMARY]
GO
SET ANSI_PADDING OFF
GO
**添加注释
EXECUTE sys.sp_addextendedproperty @name=N'MS_Description', @value=N'订单ID号' , @level0type=N'SCHEMA',@level0name=N'dbo', @level1type=N'TABLE',@level1name=N'Order', @level2type=N'COLUMN',@level2name=N'OrderID'
GO
EXECUTE sys.sp_addextendedproperty @name=N'MS_Description', @value=N'订单表' , @level0type=N'SCHEMA',@level0name=N'dbo', @level1type=N'TABLE',@level1name=N'Order'
GO
```

### TABLE | VIEW | INDEX | 权限 | UDT
```sql
**表
TRUNCATE TABLE tb
**视图
CREATE VIEW viewname AS select statement
DROP VIEW viewname
**索引
CREATE [UNIQUE] INDEX idxname on tabname(col„.)
DROP INDEX idxnam
**权限
**授权
GRANT SELECT,UPDATE,DELETE,INSERT,DELETE ON tb TO user1
**删除权限
REVOKE SELECT,UPDATE ON tb FROM user1
**用户定义数据类型
CREATE TYPE phone_number FROM NVARCHAR(20) NOT NULL
```

---

## T-SQL
### 游标
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
### 用户定义函数
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
### 存储过程
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
### 错误处理
```sql
BEGIN TRY
SELECT 3/0
END TRY
BEGIN CATCH
SELECT ERROR_NUMBER(),ERROR_SEVERITY(),ERROR_STATE(),ERROR_PROCEDURE(),ERROR_LINE(),ERROR_MESSAGE()
END CATCH
```