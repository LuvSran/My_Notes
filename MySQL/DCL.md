```sql
#创建用户

CREATE USER '用户名'@'主机名' IDENTIFIED BY '密码';

  

#修改密码

ALTER USER '用户名'@'主机名' IDENTIFIED BY '新密码';

  

#删除用户

DROP USER '用户名'@'主机名';

  

#授权

GRANT 权限列表 ON 数据库.表 TO '用户名'@'主机名';

  

#撤销权限

REVOKE 权限列表 ON 数据库.表 FROM '用户名'@'主机名';

  

#查看权限

SHOW GRANTS FOR '用户名'@'主机名';
```