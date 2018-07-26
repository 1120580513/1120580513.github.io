# SQL-DML

标签（空格分隔）： SQL DML

---
[参考](https://blog.jooq.org/2016/04/25/10-sql-tricks-that-you-didnt-think-were-possible/?utm_source=dbweekly&utm_medium=email)
##派生表
```sql
SELECT * FROM (
SELECT * FROM (VALUES(1,2),(3,4),(5,6)) AS t(a,b)
) x
```
##递归
###CTE
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
###变量
```sql
--TODO
```

##窗口函数
```sql
SELECT ROW_NUMBER() OVER(ORDER BY TABLE_NAME) row_id,* 
FROM INFORMATION_SCHEMA.TABLES
----------其他函数
--rank() 跳跃排序，如果有两个第一级时，接下来就是第三级
--dense_rank() 连续排序，如果有两个第一级时，接下来仍然是第二级
--count()、max()、min()、sum()、avg()、first_value()、last_value()
--lag(colName,offset,defval) 向前偏移offset个的值，如果超出取defval
--lead(colName,offset,defval) 向后偏移offset个的值，如果超出取defval
```