```sql
#基本查询
SELECT 字段列表 FROM 表名;

#条件查询
SELECT 字段列表 FROM 表名 WHERE 条件;

#排序查询
SELECT 字段列表 FROM 表名 ORDER BY 字段 [ASC|DESC];

#分组查询
SELECT 字段, 聚合函数 FROM 表名 GROUP BY 字段;

#分组后过滤
SELECT 字段, 聚合函数 FROM 表名 GROUP BY 字段 HAVING 条件;

#限制结果数量
SELECT 字段列表 FROM 表名 LIMIT 起始位置, 行数;

#去重查询
SELECT DISTINCT 字段列表 FROM 表名;

#连接查询（内连接）
SELECT 字段列表 FROM 表1 INNER JOIN 表2 ON 连接条件;

#连接查询（左外连接）
SELECT 字段列表 FROM 表1 LEFT JOIN 表2 ON 连接条件;

#子查询（WHERE 中）
SELECT 字段列表 FROM 表名 WHERE 字段 IN (SELECT 字段 FROM 表2 WHERE 条件);

#子查询（EXISTS）
SELECT 字段列表 FROM 表名 WHERE EXISTS (SELECT 1 FROM 表2 WHERE 关联条件);

#联合查询
SELECT 字段列表 FROM 表1 UNION [ALL] SELECT 字段列表 FROM 表2;
```