```sql
#插入单行数据
INSERT INTO 表名 (字段1, 字段2, ...) VALUES (值1, 值2, ...);

#插入多行数据
INSERT INTO 表名 (字段1, 字段2, ...) VALUES (值1, 值2, ...), (值3, 值4, ...);

#插入查询结果
INSERT INTO 表名 (字段1, 字段2, ...) SELECT 字段A, 字段B, ... FROM 另一表 WHERE 条件;

#更新数据
UPDATE 表名 SET 字段1 = 新值1, 字段2 = 新值2 WHERE 条件;

#删除数据
DELETE FROM 表名 WHERE 条件;

#清空表（保留结构，可回滚）
DELETE FROM 表名;

#清空表（保留结构，不可回滚，更快）
TRUNCATE TABLE 表名;
```