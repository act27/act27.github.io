# DQL

数据查询语言（Data Query Language,DQL)

用来查询表中的记录，主要包含 select 命令来查询++表中的数据。

SELECT 语法如下：

```mysql
SELECT
{* | <字段列名>}
[
FROM <表 1>, <表 2>…
[WHERE <表达式>
[GROUP BY <group by definition>
[HAVING <expression> [{<operator> <expression>}…]]
[ORDER BY <order by definition>]
[LIMIT[<offset>,] <row count>]
]
```

- {*|<字段列名>}表示所要查询字段的名称。
- <表 1>，<表 2>…，表示查询数据的来源，可以是单个或多个。
- WHERE <表达式>是可选项，如果选择该项，将限定查询数据必须满足该查询条件。
- GROUP BY< 字段 >，告诉 MySQL 如何显示查询出来的数据，并按照指定的字段分组。
- [ORDER BY< 字段 >]，该子句告诉 MySQL 按什么样的顺序显示查询出来的数据，可以进行的排序有升序（ASC）和降序（DESC），默认情况下是升序。
- [LIMIT<row count>]，[OFFSET<row count>]，该子句告诉 MySQL 每次显示查询出来的数据条数。



## 执行顺序！

```mysql
FROM, including JOINs
WHERE
GROUP BY
HAVING
-- WINDOW functions
-- SELECT
-- DISTINCT
-- UNION
ORDER BY
LIMIT and OFFSET
```



## 数据表查询

SELECT * FROM 表名；按顺序查询表中所有字段的数据。

```mysql
select * from student;
```



SELECT <列名> FROM <表名>;查找特定字段的数据。

```mysql
select name from studnet;
```



### DISTINCT

SELECT DISTINCT <字段名> FROM <表名>;

<字段名> 为需要消除重复记录的字段名称，多个字段用逗号隔开。

```mysql
select distinct name from student;
```

使用 distinct 关键字是需要注意以下几点：

- DISTINCT 关键字只能在 SELECT 语句中使用。
- 在对一个或多个字段去重时，DISTINCT 关键字必须在所有字段的最前面。
- 如果 DISTINCT 关键字后有多个字段，则会对多个字段进行组合去重。



### LIMIT

使用 LIMIT 属性来设定返回的记录数。

##### 指定初始位置

LIMIT 初始位置，记录数；

```mysql
select * from student limit 0,5;
-- 表示第一条记录的位置是 0 ，一共显示 5 条记录。
```

##### 不指定初始位置

LIMIT 记录数；

```mysql
select * from studnet limit 3;
-- 表示不指定初始位置，默认从第一条数据开始显示，一共显示 3 条记录。
```

##### LIMIT 和 OFFSET 组合使用

LIMIT 记录数 OFFSET 初始位置；

```mysql
select * from student limit 5 offset 3;
-- 该命令等效于：
select * from student limit 3,5;
```



### ORDER BY

ORDER BY <字段名> [ASC | DESC]

- <字段名> ，表示需要排序的字段名称，多字段用逗号隔开。
- ASC | DESC ， ASC 表示字段按 **升序** 排序；DESC 表示 **降序** 。默认为 ASC。

使用 ORDER BY 时注意：

- 当排序的字段中存在空值时，ORDER BY 会将空值作为最小值对待。
- 指定多个字段排序时，MySQL 会按照字段的顺序从左到右排序。
- 多字段排序时，排序的第一个字段 必须有相同的值，才会对第二字段进行排序。若没有，则不会对第二字段排序。
- 升序排序时，NULL会在第一条显示，NULL 会被认为是最小值。



### WHERE 条件查询

##### 单一条件查询

```mysql
SELECT *|{字段名1,字段名2,…} FROM 表名 WHERE 字段名 **[NOT] BETWEEN** 值1 AND 值2；
```



##### 多条件查询

- AND：满足所有查询条件时显示。
- OR：满足任意一个查询条件时显示。
- XOR：满足其中一个条件，且不满足零一条件时显示。

```mysql
select * from student where id = 1;
select * from student where age > 16;
select * from student where age > 16 and id = 1;
```



##### 模糊查询

[NOT] LIKE '字符串'；

- NOT ：可选参数
- 字符串：指定要匹配的字符串，可以是完整的字符串，也可以是包含通配符的字符串。

```mysql
select * from student where name like '张三';
select * from student where name like '张%';
select * from student where name like '%三%';
select * from student where name not like '张三';
select * from student where name like binary  '____u';
```



LIKE 关键字支持百分号“%”和下划线“_”通配符。

- 通配符是一种特殊语句，主要用来模糊查询。当不知道真正字符或者懒得输入完整名称时，可以使用通配符来代替一个或多个真正的字符。
- “%”是 MySQL 中最常用的通配符，它能代表任何长度的字符串，字符串的长度可以为 0。
- “_”只能代表单个字符，字符的长度不能为 0。
- 默认情况下，LIKE 关键字匹配字符的时候是不区分大小写的。如果需要区分大小写，可以用LIKE BINARY 关键字。
- 如果查询内容中包含通配符，可以使用“\”转义符。WHERE NAME LIKE '%\%'。



##### 空值查询

判断某些列是否是NULL值

```mysql
SELECT * | 字段名1，字段名2，... FROM 表名 WHERE 字段名 IS [NOT] NULL
```

- IS [NOT] NULL，不能换成 = NULL 或者 !=  NULL。sql默认对 where ** != NULL 返回0；不会提示语法错误。
- ' ' 空值和 NULL 的含义不一样；' ' 代表没有内容，不占空间；NULL 占空间，这个值的内容是NULL；NOT NULL 的意思是不饿能插入NULL，但可以插入 ' '。



### GROUP BY 分组查询

GROUP BY 语句根据一个或多个列对结果集进行分组。

```mysql
SELECT column_name, function(column_name)
FROM table_name
WHERE column_name operator value
GROUP BY column_name;
```

```mysql
select any_value(name) , any_value(sex) from tb_emp8 group by sex;
-- 查询结果会只显示sex每个分组的第一条记录。

select GROUP_CONCAT(name) , any_value(sex) from tb_emp8 group by sex;
-- GROUP_CONCAT() 函数会把每个分组的字段值都显示出来。

SELECT sex,COUNT(sex) FROM tb_students_info  GROUP BY sex;
SELECT COUNT(*) FROM 表名;
SELECT SUM(字段名) FROM 表名;
SELECT AVG(字段名) FROM student;
SELECT MAX(grade) FROM student;
SELECT MIN(grade) FROM student;
-- GROUP BY 关键字经常和聚合函数一起使用。聚合函数包括 COUNT()，SUM()，AVG()，MAX() 和 MIN()。其中，COUNT() 用来统计记录的条数；SUM() 用来计算字段值的总和。
```



## 函数

MySQL 函数会对传递进来的参数进行处理，并返回一个处理结果，也就是返回一个值。MySQL 常用函数进行简单的分类，大概包括数值型函数、字符串型函数、日期时间函数、聚合函数等。

具体函数参考：https://www.runoob.com/mysql/mysql-functions.html

### CONCAT 函数

CONCAT();

CONCAT_WS();

功能：将多个字符串连接成一个字符串。

```mysql
concat(str1,str2);
-- 连接参数产生的字符串，如果有任何一个参数为null，则返回值为null。
concat_ws('分隔符',str1,str2,...);
-- concat_ws() 函数可以设置分隔符，且结果中有 null 时只会将当前位置显示为空，不会将整个返回值都显示为空。

select concat (id, name, score) as info from tt2;
select concat_ws ('_', id, name, score) as info from tt2;
```



### IF 函数

功能：MySQL IF 语句允许您根据表达式的某个条件或值结果来执行一组 SQL 语句。表达式可以返回 TRUE,FALSE 或 NULL，这三个值之一。

```mysql
IF(expr,v1,v2);
-- 	如果表达式 expr 成立，返回结果 v1；否则，返回结果 v2。

SELECT IF(1 > 0,'正确','错误')    
->正确

SELECT IF(1<2,1,0) c1,IF(1>5,'√','×') c2,IF(STRCMP('abc','ab'),'yes','no') c3;
+----+----+-----+
| c1 | c2 | c3  |
+----+----+-----+
|  1 | × | yes |
+----+----+-----+

```



## [  待补充  ]

### HAVING 查询



### 合并查询 UNION

UNION 操作符用于连接两个以上的 SELECT 语句的结果组合到一个结果集合中。多个 SELECT 语句会删除重复的数据。

```mysql
SELECT expression1, expression2, ... expression_n
FROM tables
[WHERE conditions]
UNION [ALL | DISTINCT]
SELECT expression1, expression2, ... expression_n
FROM tables
[WHERE conditions];
```

ALL: 可选，返回所有结果集，包含重复数据。默认DISTINCT去重。

UNION 语句：用于将不同表中相同列中查询的数据展示出来；（不包括重复数据）

UNION ALL 语句：用于将不同表中相同列中查询的数据展示出来；（包括重复数据）



### 笛卡尔积

笛卡尔积（Cartesian product）是指两个集合 X 和 Y 的乘积。

例如，有 A 和 B 两个集合，它们的值如下：

A = {1,2}

B = {3,4,5}

集合 A×B 和 B×A 的结果集分别表示为：

A×B={(1,3), (1,4), (1,5), (2,3), (2,4), (2,5) };

B×A={(3,1), (3,2), (4,1), (4,2), (5,1), (5,2) };









### 交叉链接 CROSS JOIN

交叉连接（CROSS JOIN）一般用来返回连接表的笛卡尔积。



交叉连接的语法格式如下：

```mysql
SELECT <字段名> FROM <表1> CROSS JOIN <表2> [WHERE子句]
或
SELECT <字段名> FROM <表1>, <表2> [WHERE子句] 
```



课程表（课程和学生表的乘积）：

```mssql
SELECT * FROM tb_course CROSS JOIN tb_students_info;
SELECT * FROM tb_course CROSS JOIN tb_students_info  WHERE tb_students_info.course_id = tb_course.id;
```

如果在交叉连接时使用 WHERE 子句，MySQL 会先生成两个表的笛卡尔积，然后再选择满足 WHERE 条件的记录。因此，表的数量较多时，交叉连接会非常非常慢。一般情况下不建议使用交叉连接。



### 内连接 INNER JOIN

**INNER JOIN（内连接,或等值连接）**：获取两个表中字段匹配关系的记录。

内连接的语法格式如下：

```mysql
SELECT <字段名> FROM <表1> [INNER] JOIN <表2> [ON子句];

SELECT s.name,c.course_name FROM tb_students_info s INNER JOIN tb_course c  ON s.course_id = c.id;
```











### 外连接

#### LEFT JOIN

​	**LEFT JOIN（左连接）：**获取左表所有记录，即使右表没有对应匹配的记录。

```mysql
SELECT <字段名> FROM <表1> LEFT [OUTER] JOIN <表2> <ON子句>;
SELECT s.name,c.course_name FROM tb_students_info left join tb_course;
```

“表1”为基表，“表2”为参考表。左连接查询时，可以查询出“表1”中的所有记录和“表2”中匹配连接条件的记录。



#### RIGHT JOIN

​	**RIGHT JOIN（右连接）：** 与 LEFT JOIN 相反，用于获取右表所有记录，即使左表没有对应匹配的记录。

```mysql
SELECT <字段名> FROM <表1> RIGHT [OUTER] JOIN <表2> <ON子句>;
```





### 子查询

子查询是 MySQL 中比较常用的查询方法，通过子查询可以实现**多表**查询。子查询指将一个查询语句**嵌套**在**另一个查询语句**中。子查询可以在 SELECT、UPDATE 和 DELETE 语句中使用，而且可以进行多层嵌套。

外层的 SELECT 查询称为父查询，圆括号中嵌入的查询称为子查询（子查询必须放在圆括号内）。

```mysql
WHERE <表达式> <操作符> (子查询)
-- 其中，操作符可以是比较运算符 和 IN、NOT IN、EXISTS、NOT EXISTS 等关键字。

-- 1）IN | NOT IN
-- 当表达式与子查询返回的结果集中的某个值相等时，返回 TRUE，否则返回 FALSE；若使用关键字 NOT，则返回值正好相反。

-- 2）EXISTS | NOT EXISTS
-- 用于判断子查询的结果集是否为空，若子查询的结果集不为空，返回 TRUE，否则返回 FALSE；若使用关键字 NOT，则返回的值正好相反。

```











