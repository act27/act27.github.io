# MySql：DCL

数据控制语言（Data Control Language，DCL），用来管理数据库用户、控制数据库的访问权限。

**主要操作如下：**

- GRANT: 赋予用户操作权限
- REVOKE: 取消用户的操作权限
- COMMIT: 确认对数据库中数据进行的变更
- ROLLBACK: 取消对数据库中的数据进行变更



**创建用户，修改用户，修改密码，删除用户**

实例：

```sql
# 1.1.创建用户 user01，密码 123456，只可在当前主机localhost访问是。
create user 'user01'@'localhost' identified by '123456';
create user 's'@'%' identified by '123456';
# 1.2.创建用户 user02，密码 123456，可以访问任意主机
create user 'user02'@'%' identified by '123456';

# 2.修改用户
rename user 'user01' to 'user001'

# 3.修改user02的密码
alter user 'user02'@'%' identified with 'mysql_native_password' by '1234';

# 4.删除用户user02。
drop user 'user02'@'%';
```



### GRANT

```mysql
# 全局权限
grant select on "." to 'user001'@localhost;

# 查看用户权限
select * from mysql.user;
# 或者使用show grants for 'username'@'hostname' 例:
show grants for 'user001'@'localhost';



```

补充：PPT 66页



#### 用户授权

授权就是为某个用户赋予某些权限。

其语法格式如下：

```mysql
GRANT priv_type [(column_list)] [, priv_type [(column_list)]] ...
ON priv_level
TO user1 [, user2] ...
[WITH GRANT OPTION]
```

- priv_type 参数表示权限类型；
- columns_list 参数表示权限作用于哪些列上，省略该参数时，表示作用于整个表；
- Priv_level:用于指定权限的级别,例如（*，*.*, dbname.*…）
- user 参数表示用户账户，由用户名和主机名构成，格式是'username'@'hostname'”；
- WITH GRANT OPTION：表示拥有该权限后的用户可以给别的用户授予自身所拥有的权限。



#### 权限级别

- *：表示当前数据库中的所有表。
- \*.*：表示所有数据库中的所有表。
- db_name.*：表示某个数据库中的所有表，db_name 指定数据库名。
- db_name.tbl_name：表示某个数据库中的某个表或视图，db_name 指定数据库名，tbl_name 指定表名或视图名。
- db_name.routine_name：表示某个数据库中的某个存储过程或函数，routine_name 指定存储过程名或函数名。
