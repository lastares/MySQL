```
mysql -u root -p password

mysql>use mysql;

mysql>update user set host ='%'where user ='root' and host ='localhost';

mysql>flush privileges;

```



