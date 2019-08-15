<!-- GFM-TOC -->
* [一、基础](#一基础)
* [二、创建表](#二创建表)
* [三、修改表](#三修改表)
* [四、插入](#四插入)
* [五、更新](#五更新)
* [六、删除](#六删除)
* [七、查询](#七查询)
* [八、排序](#八排序)
* [九、过滤](#九过滤)
* [十、通配符](#十通配符)
* [十一、计算字段](#十一计算字段)
* [十二、函数](#十二函数)
* [十三、分组](#十三分组)
* [十四、子查询](#十四子查询)
* [十五、连接](#十五连接)
* [十六、组合查询](#十六组合查询)
* [十七、视图](#十七视图)
* [十八、存储过程](#十八存储过程)
* [十九、游标](#十九游标)
* [二十、触发器](#二十触发器)
* [二十一、事务管理](#二十一事务管理)
* [二十二、字符集](#二十二字符集)
* [二十三、权限管理](#二十三权限管理)
* [参考资料](#参考资料)
<!-- GFM-TOC -->


# 一、基础

mysql支持三种注解，分别是`#`,`--`,`/* */`
```sql
# 注释
SELECT *
FROM mytable; -- 注释
/* 注释1
   注释2 */
```

# 二、创建表

```sql
CREATE TABLE mytable (
  # int 类型，不为空，自增
  id INT NOT NULL AUTO_INCREMENT,
  # int 类型，不可为空，默认值为 1，不为空
  col1 INT NOT NULL DEFAULT 1,
  # 变长字符串类型，最长为 45 个字符，可以为空
  col2 VARCHAR(45) NULL,
  # 日期类型，可为空
  col3 DATE NULL,
  # 设置主键为 id
  PRIMARY KEY (`id`));
```

# 三、修改表

添加列
```sql
ALTER TABLE mytable
ADD col CHAR(20);
```

删除列
```sql
ALTER TABLE mytable
DROP COLUMN col;
```

删除表
```sql
DROP TABLE mytable;
```

# 四、插入

普通插入
```sql
INSERT INTO mytable(col1, col2)
VALUES(val1, val2);
```
插入检索出来的数据

```sql
INSERT INTO mytable1(col1, col2)
SELECT col1, col2
FROM mytable2;
```

将一个表的内容插入到一个新表
```sql
CREATE TABLE newtable AS
SELECT * FROM mytable;
```
# 五、更新

```sql
UPDATE mytable
SET col = val
WHERE id = 1;
```

# 六、删除

```sql
DELETE FROM mytable
WHERE id = 1;
```
TRUNCATE TABLE 可以清空表，也就是删除所有行。
```sql
TRUNCATE TABLE mytable;
```

**使用更新和删除操作时一定要用 WHERE 子句，不然会把整张表的数据都破坏。可以先用 SELECT 语句进行测试，防止错误删除。**

# 七、查询

**DISTINCT**
相同值只会出现一次。它作用于所有列，也就是说所有列的值都相同才算相同。
```sql
SELECT DISTINCT col1, col2
FROM mytable;
```
**LIMIT**
```sql
SELECT *
FROM mytable
LIMIT 0, 5;
```

# 八、排序

* ASC ：升序（默认）
* DESC ：降序
```sql
SELECT *
FROM mytable
ORDER BY col1 DESC, col2 ASC;
```

# 九、过滤
```sql
SELECT *
FROM mytable
WHERE col IS NULL;
```
下表显示了 WHERE 子句可用的操作符

|  操作符 | 说明  |
| :---: | :---: |
| = | 等于 |
| &lt; | 小于 |
| &gt; | 大于 |
| &lt;&gt; != | 不等于 |
| &lt;= !&gt; | 小于等于 |
| &gt;= !&lt; | 大于等于 |
| BETWEEN | 在两个值之间 |
| IS NULL | 为 NULL 值 |


# 十、通配符

通配符也是用在过滤语句中，但它只能用于文本字段。

* % 匹配 >=0 个任意字符；

```sql
SELECT  prod_id  ，prod_name
FROM     products
WHERE prod_Name LIKE  'jet%'
```
* _ 匹配 ==1 个任意字符；

```sql
#下划线通配符的用途与%一样，但是下划线只匹配单个字符而不是多个字符。
SELECT prod_id , prod_name
FROM   products
WHERE  prod_name LIKE  ' _ ton anvil'
```

* 可以匹配集合内的字符，例如 [ab] 将匹配字符 a 或者 b。用脱字符 ^ 可以对其进行否定，也就是不匹配集合内的字符。

# 十一、计算字段

在数据库服务器上完成数据的转换和格式化的工作往往比客户端上快得多，并且转换和格式化后的数据量更少的话可以减少网络通信量。

计算字段通常需要使用 AS 来取别名，否则输出的时候字段名为计算表达式。
```sql
SELECT col1 * col2 AS alias
FROM mytable;
```

CONCAT() 用于连接两个字段。许多数据库会使用空格把一个值填充为列宽，因此连接的结果会出现一些不必要的空格，使用 TRIM() 可以去除首尾空格。

# 十二、函数

各个 DBMS 的函数都是不相同的，因此不可移植，以下主要是 MySQL 的函数。

**汇总函数**

|  **函 数** | **说明**  |
| :---: | :---: |
|  AVG() | 返回某列的平均值  |
|  COUNT() | 返回某列的行数  |
|  MAX() | 返回某列的最大值  |
|  MIN() | 返回某列的最小值  |
|  SUM() | 返回某列值之和  |

**文本处理**

|  **函 数** | **说明**  |
| :---: | :---: |
|  LEFT() | 左边的字符  |
|  RIGHT() | 右边的字符  |
|  LOWER() | 转换为小写字符  |
|  LTRIM() | 去除左边的空格  |
|  RTRIM() | 去除右边的空格  |
|  LENGTH() | 长度  |
|  SOUNDEX() | 转换为语音值  |

其中， SOUNDEX() 可以将一个字符串转换为描述其语音表示的字母数字模式。

```sql
SELECT *
FROM mytable
WHERE SOUNDEX(col1) = SOUNDEX('apple')
```

**日期和时间处理**


- 日期格式：YYYY-MM-DD
- 时间格式：HH:<zero-width space>MM:SS

|函 数 | 说 明|
| :---: | :---: |
| ADDDATE() | 增加一个日期（天、周等）|
| ADDTIME() | 增加一个时间（时、分等）|
| CURDATE() | 返回当前日期 |
| CURTIME() | 返回当前时间 |
| DATE() |返回日期时间的日期部分|
| DATEDIFF() |计算两个日期之差|
| DATE_ADD() |高度灵活的日期运算函数|
| DATE_FORMAT() |返回一个格式化的日期或时间串|
| DAY()| 返回一个日期的天数部分|
| DAYOFWEEK() |对于一个日期，返回对应的星期几|
| HOUR() |返回一个时间的小时部分|
| MINUTE() |返回一个时间的分钟部分|
| MONTH() |返回一个日期的月份部分|
| NOW() |返回当前日期和时间|
| SECOND() |返回一个时间的秒部分|
| TIME() |返回一个日期时间的时间部分|
| YEAR() |返回一个日期的年份部分|

**数值处理**

| 函数 | 说明 |
| :---: | :---: |
| SIN() | 正弦 |
| COS() | 余弦 |
| TAN() | 正切 |
| ABS() | 绝对值 |
| SQRT() | 平方根 |
| MOD() | 余数 |
| EXP() | 指数 |
| PI() | 圆周率 |
| RAND() | 随机数 |

# 十三、分组

把具有相同的数据值的行放在同一组中。

可以对同一分组数据使用汇总函数进行处理，例如求分组数据的平均值等。

指定的分组字段除了能按该字段进行分组，也会自动按该字段进行排序。

```sql
SELECT col, COUNT(*) AS num
FROM mytable
GROUP BY col;
```

GROUP BY 自动按分组字段进行排序，ORDER BY 也可以按汇总字段来进行排序。

```sql
SELECT col, COUNT(*) AS num
FROM mytable
GROUP BY col
ORDER BY num;
```

WHERE 过滤行，HAVING 过滤分组，行过滤应当先于分组过滤。

```sql
SELECT col, COUNT(*) AS num
FROM mytable
WHERE col > 2
GROUP BY col
HAVING num >= 2;
```

分组规定：

- GROUP BY 子句出现在 WHERE 子句之后，ORDER BY 子句之前；
- 除了汇总字段外，SELECT 语句中的每一字段都必须在 GROUP BY 子句中给出；
- NULL 的行会单独分为一组；
- 大多数 SQL 实现不支持 GROUP BY 列具有可变长度的数据类型。


# 十四、子查询


子查询中只能返回一个字段的数据。

可以将子查询的结果作为 WHRER 语句的过滤条件：

```sql
SELECT *
FROM mytable1
WHERE col1 IN (SELECT col2
               FROM mytable2);
```

下面的语句可以检索出客户的订单数量，子查询语句会对第一个查询检索出的每个客户执行一次：

```sql
SELECT cust_name, (SELECT COUNT(*)
                   FROM Orders
                   WHERE Orders.cust_id = Customers.cust_id)
                   AS orders_num
FROM Customers
ORDER BY cust_name;
```

# 十五、连接

**内连接**

内连接又称等值连接，使用 INNER JOIN 关键字。
```sql
SELECT * FROM dept INNER JOIN dict ON dept.id = dict.id
```
可以不明确使用 INNER JOIN，而使用普通查询并在 WHERE 中将两个表中要连接的列用等值方法连接起来。

```sql
SELECT A.value, B.value
FROM tablea AS A, tableb AS B
WHERE A.key = B.key;
```

**自连接**
自连接可以看成内连接的一种，只是连接的表是自身而已。

一张员工表，包含员工姓名和员工所属部门，要找出与 Jim 处在同一部门的所有员工姓名。

子查询版本
```sql
SELECT name
FROM employee
WHERE department = (
      SELECT department
      FROM employee
      WHERE name = "Jim");
```
自连接版本
```sql
SELECT e1.name
FROM employee AS e1 INNER JOIN employee AS e2
ON e1.department = e2.department
      AND e2.name = "Jim";
```
**自然连接**

自然连接是把同名列通过等值测试连接起来的，同名列可以有多个。

内连接和自然连接的区别：内连接提供连接的列，而自然连接自动连接所有同名列。
```sql
SELECT A.value, B.value
FROM tablea AS A NATURAL JOIN tableb AS B;
```

**外连接**
```sql
SELECT Customers.cust_id, Orders.order_num
FROM Customers LEFT OUTER JOIN Orders
ON Customers.cust_id = Orders.cust_id;
```

# 十六、组合查询

```sql
SELECT col
FROM mytable
WHERE col = 1
UNION
SELECT col
FROM mytable
WHERE col =2;
```

# 十七、视图

```sql
CREATE VIEW myview AS
SELECT Concat(col1, col2) AS concat_col, col3*col4 AS compute_col
FROM mytable
WHERE col5 = val;
```

# 十八、存储过程
```sql
delimiter //

create procedure myprocedure( out ret int )
    begin
        declare y int;
        select sum(col1)
        from mytable
        into y;
        select y*y into ret;
    end //

delimiter ;
```
调用

```sql
call myprocedure(@ret);
select @ret;
```
# 十九、游标
# 二十、触发器
# 二十一、事务管理
# 二十二、字符集
# 二十三、权限管理