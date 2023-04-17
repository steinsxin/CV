

查看所有数据库：

```sql
show databases;
```

默认端口号：3306
查看服务器版本：select version(); 或者 cmd命令 mysql -verison

登录数据库：

```sql
mysql -uroot -p
```

退出数据库：

```sql
exit/quit
```

创建数据库：

```sql
create 库名;
```

使用数据库：

```sql
use 库名;
```

查看表：

```sql
show tables;
```

建表：

```sql
create table 表名 (字段名+空格+数据类型);
```

查看表结构：

```sql
desc 表名;
```

添值：

```sql
insert into 表名 (列名) values (值);
```

查看表中所有数据：

```sql
select * from 表名;
```

查询建表时的结构：

```sql
show create table 表名;
```

删除字段中的值：

```sql
delete from 表名 where 条件;
```

删除表中的字段：

```sql
delete from 表名 drop column 字段名;
```

删除表：

```sql
drop table 表名;
```

删除库：

```sql
drop database 库名;
```

主键约束：

```sql
primary key
```

唯一约束：

```sql
unique
```

非空约束：

```sql
not null
```

默认约束：

```sql
default
```

外键约束：

```sql
foreign key（外键）references主表（主键）
```

查看别的数据库的表格：

```sql
show tables from 表名;
```

查询数据：

```sql
select * from Username WHERE Username='steins_xin'
```

IP查询：

```sql
ipconfig/all
```

授权：

```c++
GRANT privileges ON databasename.tablename TO 'username'@'host'
说明:
privileges：用户的操作权限，如SELECT，INSERT，UPDATE等，如果要授予所的权限则使用ALL
databasename：数据库名
tablename：表名，如果要授予该用户对所有数据库和表的相应操作权限则可用*表示，如*.*
示例：
grant all privileges on mysql.* to 'steins_xin'@'%';
如果不想赋予这么多权限，可供选择的有select, insert, update, delete，逗号分隔就可以了
grant select,insert,update on mysql.* to 'steins_xin'@'%';
```

添加用户：

```c++
命令:
CREATE USER 'username'@'host' IDENTIFIED BY 'password';
说明：
username：你将创建的用户名
host：指定该用户在哪个主机上可以登陆，如果是本地用户可用localhost，如果想让该用户可以从任意远程主机登陆，可以使用通配符%
password：该用户的登陆密码，密码可以为空，如果为空则该用户可以不需要密码登陆服务器
```

设置与更改用户密码：

```c++
SET PASSWORD FOR 'username'@'host' = PASSWORD('newpassword');
如果是当前登陆用户用:
SET PASSWORD = PASSWORD("newpassword");
例子:
SET PASSWORD FOR 'pig'@'%' = PASSWORD("123456");
```

删除用户

```c++
命令:
DROP USER 'username'@'host';
```

QT打包命令：

```c++
windeployqt 程序.exe
```

查看用户：

```c++
select host, user from user;
```

mysql启动关闭：

```c++
net start mysql //启动数据库
net stop mysql // 有时候需要停止数据库服务，停止数据库
```

