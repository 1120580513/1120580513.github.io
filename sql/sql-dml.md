# SQL-DML

---
[参考](https://blog.jooq.org/2016/04/25/10-sql-tricks-that-you-didnt-think-were-possible/?utm_source=dbweekly&utm_medium=email)

* [SQL\-DML](#sql-dml)
  * [派生表](#%E6%B4%BE%E7%94%9F%E8%A1%A8)
  * [递归](#%E9%80%92%E5%BD%92)
    * [CTE](#cte)
    * [变量](#%E5%8F%98%E9%87%8F)
  * [窗口函数](#%E7%AA%97%E5%8F%A3%E5%87%BD%E6%95%B0)
  * [优化](#%E4%BC%98%E5%8C%96)
    * [索引失效的操作](#%E7%B4%A2%E5%BC%95%E5%A4%B1%E6%95%88%E7%9A%84%E6%93%8D%E4%BD%9C)

## 派生表
```sql
SELECT * FROM (
SELECT * FROM (VALUES(1,2),(3,4),(5,6)) AS t(a,b)
) x
```
## 递归

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

### 变量
```sql
--TODO
```

## 窗口函数
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

## 优化
### 索引失效的操作
 - 和NULL的判断操作
 - != <>操作
 - OR
 - LIKE
 - 使用表达式：WHERE num = 100 * 2
 - 使用函数
 - 在WHERE和ORDER BY中不要对索引字段进行计算