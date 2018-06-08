#### 1.管理员表

```sql
create table admin
(
    id mediumint unsigned not null auto_increment comment 'Id',
    username varchar(30) not null comment '用户名',
    password char(32) not null comment '密码',
    primary key (id)
)engine=InnoDB default charset=utf8 comment '管理员';
```

#### 2.角色表

```sql
create table role
(
    id mediumint unsigned not null auto_increment comment 'Id',
    role_name varchar(30) not null comment '角色名称',
    primary key (id)
)engine=InnoDB default charset=utf8 comment '角色';
```

#### 3.权限表

```sql
create table privilege
(
    id mediumint unsigned not null auto_increment comment 'Id',
    pri_name varchar(30) not null comment '权限名称',
    module_name varchar(30) not null default '' comment '模块名称',
    controller_name varchar(30) not null default '' comment '控制器名称',
    action_name varchar(30) not null default '' comment '方法名称',
    parent_id mediumint unsigned not null default '0' comment '上级权限Id',
    primary key (id)
)engine=InnoDB default charset=utf8 comment '权限';
```

#### 4.管理员角色表

```sql
create table admin_role
(
    admin_id mediumint unsigned not null comment '管理员id',
    role_id mediumint unsigned not null comment '角色id',
    key admin_id(admin_id),
    key role_id(role_id)
)engine=InnoDB default charset=utf8 comment '管理员角色';
```

#### 5.权限角色表

```sql
create table role_pri
(
    pri_id mediumint unsigned not null comment '权限id',
    role_id mediumint unsigned not null comment '角色id',
    key pri_id(pri_id),
    key role_id(role_id)
)engine=InnoDB default charset=utf8 comment '角色权限';
```



