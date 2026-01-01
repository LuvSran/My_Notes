```sql
#创建数据库
CREATE DATABASE 数据库名;

#删除数据库
DROP DATABASE 数据库名;

#创建表
CREATE TABLE 表名 (
    字段1 数据类型 [约束],
    字段2 数据类型 [约束]
);

#复制表结构（不含数据）
CREATE TABLE 新表名 LIKE 原表名;

#复制表结构和数据
CREATE TABLE 新表名 AS SELECT * FROM 原表名;

#修改表（添加字段）
ALTER TABLE 表名 ADD COLUMN 字段名 数据类型 [约束];

#修改表（修改字段定义）
ALTER TABLE 表名 MODIFY COLUMN 字段名 新数据类型 [新约束];

#修改表（重命名字段）
ALTER TABLE 表名 CHANGE COLUMN 旧字段名 新字段名 新数据类型;

#删除字段
ALTER TABLE 表名 DROP COLUMN 字段名;

#重命名表
ALTER TABLE 旧表名 RENAME TO 新表名;

#添加主键
ALTER TABLE 表名 ADD PRIMARY KEY (字段名);

#添加外键
ALTER TABLE 表名 ADD FOREIGN KEY (字段名) REFERENCES 主表名(主键字段);

#添加索引
ALTER TABLE 表名 ADD INDEX 索引名 (字段名);

#删除索引
DROP INDEX 索引名 ON 表名;

#删除表
DROP TABLE 表名;

#清空表（保留结构）
TRUNCATE TABLE 表名;
```