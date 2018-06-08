1.管理员表

```sql
CREATE TABLE `kqc_admin` (
  `id` smallint(5) unsigned NOT NULL AUTO_INCREMENT,
  `username` varchar(20) NOT NULL COMMENT '用户名',
  `password` char(32) NOT NULL COMMENT '密码',
  `salt` char(4) NOT NULL COMMENT '盐值',
  `role_id` tinyint(3) NOT NULL COMMENT '角色ID',
  `realname` varchar(16) NOT NULL COMMENT '姓名',
  `mobile` char(11) NOT NULL COMMENT '手机号',
  `last_login_ip` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '最后登录IP',
  `last_login_time` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '最后登录时间',
  `created_at` int(10) unsigned NOT NULL COMMENT '创建时间',
  `updated_at` int(10) unsigned NOT NULL COMMENT '更新时间',
  `status` tinyint(1) DEFAULT '1' COMMENT '状态 1 锁定  2禁用',
  `is_leader` tinyint(3) NOT NULL DEFAULT '0' COMMENT '0 不是leader  1 是',
  `subordinate` varchar(32) NOT NULL DEFAULT '' COMMENT '下属用户组的id(英文 , 号分割)    如：1,5,7',
  `dept_id` int(10) NOT NULL DEFAULT '0' COMMENT '部门id，关联erp_admin_group表',
  `position_id` smallint(5) NOT NULL DEFAULT '0' COMMENT '职位id',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

2.权限菜单表

```sql
CREATE TABLE `kqc_access` (
  `id` mediumint(8) unsigned NOT NULL AUTO_INCREMENT,
  `role_id` smallint(5) unsigned NOT NULL DEFAULT '0' COMMENT '角色(ID)',
  `menu_id` smallint(5) unsigned NOT NULL COMMENT '菜单ID',
  `type` tinyint(3) unsigned NOT NULL COMMENT '1为用户组2为用户,默认为用户组',
  `created_at` int(10) unsigned NOT NULL COMMENT '创建时间',
  `updated_at` int(10) unsigned NOT NULL COMMENT '更新时间',
  `deleted_at` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '删除时间',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```



