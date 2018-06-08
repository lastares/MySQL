1.管理员角色表

```sql
create table admin_role
(
	admin_id mediumint unsigned not null comment '管理员id',
	role_id mediumint unsigned not null comment '角色id',
	key admin_id(admin_id),
	key role_id(role_id)
)engine=InnoDB default charset=utf8 comment '管理员角色';
```



