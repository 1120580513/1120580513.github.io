# SQL

---

* [SQL](#sql)
* [SQL标准(<a href="https://blog\.csdn\.net/lengye7/article/details/80606489" rel="nofollow">源</a>)](#sql%E6%A0%87%E5%87%86%E6%BA%90)
* [三大范式(<a href="https://blog\.csdn\.net/dove\_knowledge/article/details/71434960" rel="nofollow">详细</a>)](#%E4%B8%89%E5%A4%A7%E8%8C%83%E5%BC%8F%E8%AF%A6%E7%BB%86)
* [事务(<a href="https://www\.cnblogs\.com/fjdingsd/p/5273008\.html" rel="nofollow">源</a>)](#%E4%BA%8B%E5%8A%A1%E6%BA%90)
  * [问题(如果没有隔离性)](#%E9%97%AE%E9%A2%98%E5%A6%82%E6%9E%9C%E6%B2%A1%E6%9C%89%E9%9A%94%E7%A6%BB%E6%80%A7)
  * [事务隔离级别](#%E4%BA%8B%E5%8A%A1%E9%9A%94%E7%A6%BB%E7%BA%A7%E5%88%AB)

# SQL标准([源](https://blog.csdn.net/lengye7/article/details/80606489))
 1. **1986年，ANSI X3.135-1986，ISO/IEC 9075:1986，`SQL-86`**
 2. **1989年，ANSI X3.135-1989，ISO/IEC 9075:1989，`SQL-89`**
 3. **1992年，ANSI X3.135-1992，ISO/IEC 9075:1992，`SQL-92（SQL2）`**
 4. **1999年，ISO/IEC 9075:1999，`SQL:1999（SQL3）`**
 5. **2003年，ISO/IEC 9075:2003，`SQL:2003`**
 6. **2008年，ISO/IEC 9075:2008，`SQL:2008`**
 7. **2011年，ISO/IEC 9075:2011，`SQL:2011`**
>绝大多数人提起SQL标准，涉及的内容其实是`SQL92`里头最基本或者说最核心的一部分。SQL92本身是分级的，包括`入门级`、`过度级`、`中间级`和`完全级`。为了验证具体的产品对标准的遵从程度，NIST还曾经专门发起了一个项目，来做标准符合程度的[测试集合](http://itl.nist.gov/div897/ctg/sql_form.htm)。不过，SQL标准包含的内容实在太多了，而且有很多特性对新的SQL产品而言也越来越不重要了。从SQL99之后，标准中符合程度的定义就不再分级，而是改成了核心兼容性和特性兼容性；也没有机构来推出权威的SQL标准符合程度的测试认证了。

# 三大范式([详细](https://blog.csdn.net/dove_knowledge/article/details/71434960))
 1. 确保每列保持原子性`1NF`
 2. 在`1NF`的前提下确保表中的每列都和主键相关`2NF`
 3. 在`2NF`的前提下确保每列都和主键列直接相关,而不是间接相关`3NF`

# 事务([源](https://www.cnblogs.com/fjdingsd/p/5273008.html))
## ACID
 - 原子性（Atomicity）
 - 一致性（Consistency）
 - 隔离性（Isolation）
 - 持久性（Durability）

## 问题(如果没有隔离性)
- 脏读
- 不可重复读
- 虚读(幻读)

## 事务隔离级别
- `未提交读（READ UNCOMMITTED）`：事务中的修改，即使没有提交，对其它事务也是可见的。最低级别，任何情况都无法保证。
- `提交读（READCOMMITTED）`：一个事务只能读取已经提交的事务所做的修改。句话说，一个事务所做的修改在提交之前对其它事务是不可见的。可避免脏读的发生。（Sql Server、Oracle默认级别）
- `可重复读（REPEATABLEREAD）`：保证在同一个事务中多次读取同样数据的结果是一样的。可避免脏读、不可重复读的发生。（MySql默认级别）
- `可串行化（SERIALIXABLE）`：强制事务串行执行。可避免脏读、不可重复读、幻读的发生。
