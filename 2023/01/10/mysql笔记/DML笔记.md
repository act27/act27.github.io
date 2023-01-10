# DML

数据操作语言（Data Manipulation Language，DML）

**主要操作如下：**

- INSERT: 向表中插入新数据

- UPDATE: 更新表中的数据

- DELETE: 删除表中的数据



### 增 INSERT 

INSERT 语句有两种语法形式，分别是 INSERT…VALUES 语句和 INSERT…SET 语句。

##### INSERT ... VALUE

```mysql
INSERT INTO <表名> [(<列名1> [ , … <列名n>)] ] VALUES (值1) [… , (值n) ];
insert into student (id, name, sex, age) values (11,'刘强', '男',20);

-- INSERT…VALUES 后面可以不加字段名，如下：
INSERT INTO 表名 VALUES(值1，值2，……);   
-- 此时添加的值的顺序必须和字段在表中定义的顺序相同。
insert into student values (12,'张思奥','男',21);
```



##### INSERT ... SET

```mysql
INSERT INTO <表名> SET <列名1> = <值1>,<列名2> = <值2>, …;

insert into student set id = 13, name = '敖强';
```



### 改 UPDATE

UPDATE 语句用于修改、更新一个或多个表的数据。

使用 UPDATE 语句修改单个表，语法格式为：

```mysql
UPDATE <表名> SET 字段 1=值 1 [,字段 2=值 2… ] [WHERE 子句 ] [ORDER BY 子句] [LIMIT 子句];

update student set 
```

- <表名>：用于指定要更新的表名称。
- SET 子句：用于指定表中要修改的列名及其列值。其中，每个指定的列值可以是表达式，也可以是该列对应的默认值。如果指定的是默认值，可用关键字 DEFAULT 表示列值。
- WHERE 子句：可选项。用于限定表中要修改的行。若不指定，则修改表中所有的行。
- ORDER BY 子句：可选项。用于限定表中的行被修改的次序。
- LIMIT 子句：可选项。用于限定被修改的行数。
- 注意：修改一行数据的多个列值时，SET 子句的每个值用逗号分开即可。



### 删 DELETE

DELETE 用来删除MySQL 数据表中的数据。

```mysql
DELETE FROM <表名> [WHERE 子句] [ORDER BY 子句] [LIMIT 子句];
```

