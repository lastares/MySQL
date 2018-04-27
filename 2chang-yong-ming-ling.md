##### 

##### 1.显示数据库

```
show databases;
```

2.创建数据库

```
create database 数据库名
eg: create database blog
```

3.选择数据库

```
use 数据库名
eg： use blog
```

4.直接删除数据库不提醒

```
drop database name
```

5.修改root用户密码

方法1：

```
SET PASSWORD FOR 'root'@'localhost' = PASSWORD('newpass');
```

方法2

```
mysql -u root
mysql> use mysql;
mysql> UPDATE user SET Password = PASSWORD('newpass') WHERE user = 'root';
mysql> FLUSH PRIVILEGES;
```



