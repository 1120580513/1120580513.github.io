# MSSQL

标签（空格分隔）： MSSQL

---

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

## sys
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