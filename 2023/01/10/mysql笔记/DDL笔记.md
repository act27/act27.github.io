# Mysql：DDL

DDL 数据库

数据库定义语言（Data Definition Language，DDL）

**主要操作如下：**

- CREATE：创建数据库和表
- ALTER：修改数据库和表
- DROP：删除数据库和表

## 数据库

### CREATE

#### 	创建数据库：

```mysql
# 1.查看数据库
show databases;
# 2.创建数据库
create database test_1;
create database test_2;
create database test_3;
# 3.再次查看数据库
show databases;
# 4.使用like从句查看数据库
show database like "test%";
```

#### IF NOT EXISTS:

if not exists 作用是确保创建的数据库不存在。

```mysql
# 再次创建test_1数据库，会报错
create database test_1;
# 加上if not exist 则不会报错
create database if not exists test_1;
```

#### 创建指定字符集的数据库：

```mysql
create database if not exists school default character set utf8;
```



### ALTER

#### 修改数据库：

语法格式：

```mysql
alter database <数据库名> [default character set <字符集名>] [collate <校对规则名>];
```

 ↑ 待修改



### DROP

#### 删除数据库：

语法格式：

```mysql
drop database if exists <数据库名>;
```

例：

```mysql
drop database if exists test_3;
```









## 数据表

### CREATE

#### 	创建表：

语法：

```mysql
CREATE TABLE <表名>(<列名1 类型1，列名2 类型2>,...);
```

例：

```mysql
# 1.使用数据库
use test_1;
# 2.查看表
show tables;
# 3.创建表
create table student(id int,name varchar(25));

```

### ALTER

#### 修改表：

语法：

```mysql
ALTER TABLE <表名> [修改项];
# [修改项] 如下:
ADD COLUMN <列名> <类型>
CHANGE COLUMN <旧列名> <新列名> <类型>
MODIFY COLUMN <列名> <类型>
DROP COLUMN <列名>

RENAME TO <新表名>
CHARACTER SET  <字符集名>
COLLATE <校对规则名>
```

例：

```mysql
alter table student add column age int;
alter table change column name stname varchar(20);
alter table modify c
```











