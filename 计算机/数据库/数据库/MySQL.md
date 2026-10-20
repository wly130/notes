## MySQL

### 常用操作

#### 创建数据库

```sql
CREATE DATABASE 数据库名;
```

#### 删除数据库

```sql
DROP DATABASE 数据库名;
```

#### 进入数据库

```sql
USE 数据库名;
```

#### 查看某个数据库

```sql
SHOW CREATE DATABASE 数据库名;
```

#### 查看所有数据库

```sql
SHOW DATABASES;
```

#### 修改数据库编码方式

```sql
ALTER DATABASE 数据库名 DEFAULT CHARACTER SET utf8mb4;
```

#### 创建数据表

```sql
CREATE TABLE 表名 (
    字段1 数据类型(范围) NULL,
    字段2 数据类型(范围) NULL
);
```

#### 删除数据表

```sql
DROP TABLE 表名;
```

#### 修改字段名

```sql
ALTER TABLE 表名 CHANGE 旧字段名 新字段名 新数据类型;
```

#### 修改表名

```sql
ALTER TABLE 旧表名 RENAME 新表名;
```

#### 修改数据类型

```sql
ALTER TABLE 表名 MODIFY 字段名 数据类型();
```

#### 添加字段

```sql
ALTER TABLE 表名 ADD 新字段名 数据类型();
```

#### 删除字段

```sql
ALTER TABLE 表名 DROP 字段名;
```

#### 查看表结构

```sql
DESC 表名;
SHOW CREATE TABLE 表名;
```

#### 添加数据

```sql
INSERT INTO 表名 (字段1, 字段2) VALUES (值1, 值2);
INSERT INTO 表名 (字段1) VALUES (1);              --添加数字
INSERT INTO 表名 (字段1) VALUES ('name');         --添加字符串
INSERT INTO 表名 (字段1) VALUES ('2000-01-01');   --添加时间
INSERT INTO 表名 (字段1) VALUES ('{"key":"val"}');--添加JSON
INSERT IGNORE INTO 表名 (字段1) VALUES (值1);     --忽略报错数据
```

#### 删除数据

```sql
DELETE FROM 表名 WHERE 条件;
TRUNCATE TABLE 表名;  --清空表(重置自增)
```

#### 修改数据

```sql
UPDATE 表名 SET 字段名1=值1, 字段名2=值2 WHERE 条件;
```

#### 查询数据

```sql
SELECT * FROM 表名 WHERE 条件;
```

### 约束

#### NOT NULL 非空约束

```sql
ALTER TABLE 表名 MODIFY 字段名 数据类型 NOT NULL;
ALTER TABLE 表名 MODIFY 字段名 数据类型;  --删除约束
```

#### UNIQUE 唯一约束

```sql
ALTER TABLE 表名 ADD UNIQUE (字段名);
ALTER TABLE 表名 DROP INDEX 索引名;  --删除约束
```

#### PRIMARY KEY 主键约束

```sql
ALTER TABLE 表名 ADD PRIMARY KEY (字段名);
ALTER TABLE 表名 DROP PRIMARY KEY;  --删除约束
```

#### FOREIGN KEY 外键约束

```sql
ALTER TABLE 表名 ADD FOREIGN KEY (字段名) REFERENCES 外表名(外表字段名);
ALTER TABLE 表名 DROP FOREIGN KEY 外键名;  --删除约束
```

#### DEFAULT 默认值约束

```sql
ALTER TABLE 表名 ALTER 字段名 SET DEFAULT '默认值';
ALTER TABLE 表名 ALTER 字段名 DROP DEFAULT;  --删除约束
```

#### 自增长

```sql
ALTER TABLE 表名 MODIFY 字段名 数据类型 AUTO_INCREMENT;
ALTER TABLE 表名 MODIFY 字段名 数据类型;  --删除自增长
```

### 索引

#### 创建索引

```sql
CREATE INDEX 索引名 ON 表名 (字段名);
CREATE UNIQUE INDEX 索引名 ON 表名 (字段名);  --唯一索引
ALTER TABLE 表名 ADD INDEX 索引名 (字段名);
```

#### 删除索引

```sql
DROP INDEX 索引名 ON 表名;
ALTER TABLE 表名 DROP INDEX 索引名;
```

#### 查看索引

```sql
SHOW INDEX FROM 表名;
```

| 索引类型     | 说明                                     |
| ------------ | ---------------------------------------- |
| **PRIMARY**  | 主键索引，唯一且不为空                   |
| **UNIQUE**   | 唯一索引，索引列的值必须唯一，允许空值   |
| **INDEX**    | 普通索引，最基本的索引                   |
| **FULLTEXT** | 全文索引，用于文本搜索                   |
| **SPATIAL**  | 空间索引，用于地理数据类型               |

| SQL分类   | 说明                                         |
| --------- | -------------------------------------------- |
| **DML**   | 数据操作语言：INSERT、UPDATE、DELETE         |
| **DDL**   | 数据定义语言：CREATE、ALTER、DROP            |
| **DCL**   | 数据控制语言：GRANT、REVOKE                  |
| **DQL**   | 数据查询语言：SELECT                         |

### 数据类型

#### 数值

| 类型          | 大小       | 范围(有符号)                    | 用途           |
| ------------- | ---------- | ------------------------------- | -------------- |
| TINYINT       | **1Bytes** | (-128, 127)                     | 小整数值       |
| SMALLINT      | **2Bytes** | (-32768, 32767)                 | 整数值         |
| MEDIUMINT     | **3Bytes** | (-8388608, 8388607)             | 整数值         |
| INT / INTEGER | **4Bytes** | (-2^31^, 2^31^-1)               | 大整数值       |
| BIGINT        | **8Bytes** | (-2^63^, 2^63^-1)               | 极大整数值     |
| FLOAT         | **4Bytes** | 单精度浮点数                    | 浮点数值       |
| DOUBLE        | **8Bytes** | 双精度浮点数                    | 浮点数值       |
| DECIMAL(M,D)  | **变长**   | 精确小数                        | 金额等精确值   |

#### 日期时间

| 类型     | 大小       | 范围                                        | 格式                |
| -------- | ---------- | ------------------------------------------- | ------------------- |
| DATE     | **3Bytes** | 1000-01-01 ~ 9999-12-31                     | YYYY-MM-DD          |
| TIME     | **3Bytes** | -838:59:59 ~ 838:59:59                      | HH:MM:SS            |
| YEAR     | **1Bytes** | 1901 ~ 2155                                 | YYYY                |
| DATETIME | **8Bytes** | 1000-01-01 00:00:00 ~ 9999-12-31 23:59:59   | YYYY-MM-DD HH:MM:SS |
| TIMESTAMP| **4Bytes** | 1970-01-01 00:00:01 ~ 2038-01-19 03:14:07   | YYYY-MM-DD HH:MM:SS |

#### 字符串

| 类型       | 大小(Bytes)  | 用途                            |
| ---------- | ------------ | ------------------------------- |
| CHAR       | 0-255        | 定长字符串                      |
| VARCHAR    | 0-65535      | 变长字符串                      |
| TINYTEXT   | 0-255        | 短文本字符串                    |
| TEXT       | 0-65535      | 长文本数据                      |
| MEDIUMTEXT | 0-16777215   | 中等长度文本数据                |
| LONGTEXT   | 0-4294967295 | 极大文本数据                    |
| TINYBLOB   | 0-255        | 二进制数据                      |
| BLOB       | 0-65535      | 二进制长数据                    |
| JSON       | -            | JSON格式数据                    |

### 单表查询

#### 查询指定字段

```sql
SELECT 字段1, 字段2 FROM 表名;
```

#### 条件查询

```sql
SELECT * FROM 表名 WHERE 条件;
```

#### 范围查询

```sql
SELECT * FROM 表名 WHERE 字段 BETWEEN 值1 AND 值2;
SELECT * FROM 表名 WHERE 字段 IN (值1, 值2, 值3);
```

#### 空值查询

```sql
SELECT * FROM 表名 WHERE 字段 IS NULL;
SELECT * FROM 表名 WHERE 字段 IS NOT NULL;
```

#### 去重查询

```sql
SELECT DISTINCT 字段 FROM 表名;
```

#### 模糊查询

```sql
SELECT * FROM 表名 WHERE 字段 LIKE '%x%';  --包含x
SELECT * FROM 表名 WHERE 字段 LIKE 'x%';   --以x开头
SELECT * FROM 表名 WHERE 字段 LIKE '%x';   --以x结尾
SELECT * FROM 表名 WHERE 字段 LIKE '_x';   --长度为2，以x结尾
```

#### 正则查询

```sql
SELECT * FROM 表名 WHERE 字段 REGEXP '^x';      --以x开头
SELECT * FROM 表名 WHERE 字段 REGEXP 'x$';      --以x结尾
SELECT * FROM 表名 WHERE 字段 REGEXP '[0-9]';   --包含数字
```

#### 别名

```sql
SELECT 字段 AS 别名 FROM 表名 AS 别名;
```

#### 分页

```sql
SELECT * FROM 表名 LIMIT n;          --前n行
SELECT * FROM 表名 LIMIT offset, n;  --跳过offset行，取n行
SELECT * FROM 表名 LIMIT n OFFSET m; --同上
```

#### 排序

```sql
SELECT * FROM 表名 ORDER BY 字段 ASC;   --升序(默认)
SELECT * FROM 表名 ORDER BY 字段 DESC;  --降序
SELECT * FROM 表名 ORDER BY 字段1 ASC, 字段2 DESC;  --多字段排序
```

#### JSON查询

```sql
SELECT * FROM 表名 WHERE json字段->'$.key' = 'value';
SELECT * FROM 表名 WHERE JSON_CONTAINS(json字段, '"value"', '$.key');
```

#### 聚合函数

```sql
SELECT COUNT(*) FROM 表名;           --总数
SELECT SUM(字段) FROM 表名;          --求和
SELECT AVG(字段) FROM 表名;          --平均值
SELECT MAX(字段) FROM 表名;          --最大值
SELECT MIN(字段) FROM 表名;          --最小值
```

#### 分组查询

```sql
SELECT 字段, COUNT(*) FROM 表名 GROUP BY 字段;
SELECT 字段, COUNT(*) FROM 表名 GROUP BY 字段 HAVING COUNT(*) > 1;
```

### 多表查询

#### INNER JOIN(内连接)

```sql
--返回两表匹配的行
SELECT a.字段, b.字段 FROM 表a AS a INNER JOIN 表b AS b ON a.id = b.a_id;
```

#### LEFT JOIN(左连接)

```sql
--返回左表所有行，右表无匹配则为NULL
SELECT a.字段, b.字段 FROM 表a AS a LEFT JOIN 表b AS b ON a.id = b.a_id;
```

#### RIGHT JOIN(右连接)

```sql
--返回右表所有行，左表无匹配则为NULL
SELECT a.字段, b.字段 FROM 表a AS a RIGHT JOIN 表b AS b ON a.id = b.a_id;
```

#### FULL JOIN(全连接)

```sql
--MySQL不直接支持，使用UNION实现
SELECT * FROM 表a LEFT JOIN 表b ON a.id = b.a_id
UNION
SELECT * FROM 表a RIGHT JOIN 表b ON a.id = b.a_id;
```

#### 子查询

```sql
SELECT * FROM 表名 WHERE 字段 IN (SELECT 字段 FROM 表名2);
SELECT * FROM 表名 WHERE 字段 = (SELECT MAX(字段) FROM 表名);
```

### 存储过程

#### 创建存储过程

```sql
DELIMITER $
CREATE PROCEDURE 过程名(参数)
BEGIN
    --过程内容
END$
DELIMITER ;
```

#### 调用/删除存储过程

```sql
CALL 过程名(参数);
DROP PROCEDURE 过程名;
```

#### 变量

```sql
DECLARE 变量名 数据类型 DEFAULT 默认值;  --定义
SET @变量名 = 值;                        --赋值
SELECT @变量名 := 值;                    --赋值
```

#### 参数类型

| 参数    | 说明         |
| ------- | ------------ |
| IN      | 输入参数     |
| OUT     | 输出参数     |
| INOUT   | 输入输出参数 |

#### 示例

```sql
DELIMITER $
CREATE PROCEDURE test(IN num INT)
BEGIN
    SELECT num * 2;
END$
DELIMITER ;

CALL test(10);
```

### 事务

#### 特点(ACID)

| 特性       | 说明                                         |
| ---------- | -------------------------------------------- |
| **原子性** | 事务中的操作要么全部完成，要么全部不完成     |
| **一致性** | 事务开始前后，数据库完整性没有被破坏         |
| **隔离性** | 多个并发事务之间互不干扰                     |
| **持久性** | 事务完成后，数据修改是永久的                 |

#### 事务操作

```sql
START TRANSACTION;  --开启事务
--或 SET autocommit = 0;

INSERT INTO 表名 (字段) VALUES (值);
UPDATE 表名 SET 字段=值 WHERE 条件;

COMMIT;    --提交事务
ROLLBACK;  --回滚事务
```

#### 隔离级别

| 隔离级别                | 脏读 | 不可重复读 | 幻读 | 说明             |
| ----------------------- | ---- | ---------- | ---- | ---------------- |
| READ UNCOMMITTED        | ✓    | ✓          | ✓    | 读未提交         |
| READ COMMITTED          | ✗    | ✓          | ✓    | 读已提交         |
| **REPEATABLE READ(默认)** | ✗  | ✗          | ✓    | 可重复读         |
| SERIALIZABLE            | ✗    | ✗          | ✗    | 串行化(性能最低) |

- **脏读**：读到其他事务未提交的数据
- **不可重复读**：同一事务内两次查询结果不同
- **幻读**：同一事务内两次查询行数不同

```sql
--查看隔离级别
SELECT @@transaction_isolation;

--设置隔离级别
SET SESSION TRANSACTION ISOLATION LEVEL REPEATABLE READ;
```

### 锁

| 锁类型         | 说明                                 |
| -------------- | ------------------------------------ |
| **共享锁(S锁)** | 读锁，多个事务可同时加              |
| **排他锁(X锁)** | 写锁，独占，阻塞其他读写            |
| **行锁**       | 锁定单行，InnoDB支持                 |
| **表锁**       | 锁定整表，MyISAM默认                 |
| **间隙锁**     | 锁定范围，防止幻读                   |

```sql
--加共享锁
SELECT * FROM 表名 WHERE 条件 LOCK IN SHARE MODE;

--加排他锁
SELECT * FROM 表名 WHERE 条件 FOR UPDATE;
```

### 视图

#### 创建视图

```sql
CREATE VIEW 视图名 AS SELECT 语句;
```

#### 查看/修改/删除视图

```sql
SELECT * FROM 视图名;
CREATE OR REPLACE VIEW 视图名 AS 新SELECT语句;
ALTER VIEW 视图名 AS 新SELECT语句;
DROP VIEW 视图名;
```

### SQL函数

| 函数              | 说明                     |
| ----------------- | ------------------------ |
| **COUNT()**       | 返回行数                 |
| **SUM()**         | 返回总和                 |
| **AVG()**         | 返回平均值               |
| **MAX()**         | 返回最大值               |
| **MIN()**         | 返回最小值               |
| **ABS()**         | 返回绝对值               |
| **MOD()**         | 返回余数                 |
| **SQRT()**        | 返回平方根               |
| **POW(x,y)**      | 返回x的y次方             |
| **ROUND()**       | 四舍五入                 |
| **RAND()**        | 返回随机数(0-1)          |
| **UPPER()/UCASE()** | 转换为大写             |
| **LOWER()/LCASE()** | 转换为小写             |
| **LENGTH()**      | 返回字节长度             |
| **CHAR_LENGTH()** | 返回字符长度             |
| **SUBSTRING()**   | 截取字符串               |
| **CONCAT()**      | 拼接字符串               |
| **REPLACE()**     | 替换字符串               |
| **NOW()**         | 当前日期时间             |
| **CURDATE()**     | 当前日期                 |
| **CURTIME()**     | 当前时间                 |
| **DATE_FORMAT()** | 格式化日期               |
| **IFNULL()**      | 空值替换                 |

#### IF判断

```sql
SELECT IF(条件, 真值, 假值);
SELECT IFNULL(字段, 默认值) FROM 表名;
```

#### CASE函数

```sql
--有字段
SELECT CASE 字段名
    WHEN 值1 THEN 结果1
    WHEN 值2 THEN 结果2
    ELSE 默认结果
END FROM 表名;

--无字段
SELECT CASE
    WHEN 条件1 THEN 结果1
    WHEN 条件2 THEN 结果2
    ELSE 默认结果
END FROM 表名;
```

### EXPLAIN分析

```sql
EXPLAIN SELECT * FROM 表名 WHERE 条件;
```

| 字段       | 说明                                         |
| ---------- | -------------------------------------------- |
| **id**     | 查询序号                                     |
| **type**   | 访问类型(性能从好到差)                       |
| **key**    | 实际使用的索引                               |
| **rows**   | 预估扫描行数                                 |
| **Extra**  | 额外信息                                     |

| type值     | 说明               |
| ---------- | ------------------ |
| **const**  | 主键/唯一索引查询  |
| **ref**    | 普通索引查询       |
| **range**  | 范围查询           |
| **index**  | 全索引扫描         |
| **ALL**    | 全表扫描(最差)     |

### 用户与权限

```sql
--创建用户
CREATE USER '用户名'@'localhost' IDENTIFIED BY '密码';

--授权
GRANT ALL PRIVILEGES ON 数据库名.* TO '用户名'@'localhost';
GRANT SELECT, INSERT ON 数据库名.* TO '用户名'@'localhost';

--刷新权限
FLUSH PRIVILEGES;

--查看权限
SHOW GRANTS FOR '用户名'@'localhost';

--撤销权限
REVOKE SELECT ON 数据库名.* FROM '用户名'@'localhost';

--删除用户
DROP USER '用户名'@'localhost';
```

### 备份与恢复

#### 备份(mysqldump)

```shell
#备份单个数据库
mysqldump -u root -p 数据库名 > backup.sql

#备份多个数据库
mysqldump -u root -p --databases 库1 库2 > backup.sql

#备份所有数据库
mysqldump -u root -p --all-databases > backup.sql

#备份指定表
mysqldump -u root -p 数据库名 表名 > backup.sql
```

#### 恢复

```shell
#命令行恢复
mysql -u root -p 数据库名 < backup.sql
```

```sql
--SQL方式恢复
SOURCE backup.sql;
```

#### 导出查询结果

```sql
SELECT * FROM 表名 INTO OUTFILE '/path/to/file.csv'
FIELDS TERMINATED BY ',' ENCLOSED BY '"'
LINES TERMINATED BY '\n';
```

### SQL优化建议

| 优化点               | 说明                                   |
| -------------------- | -------------------------------------- |
| 避免 `SELECT *`      | 只查询需要的字段                       |
| 使用 `LIMIT`         | 大数据量查询必须分页                   |
| 索引字段避免函数     | `WHERE YEAR(date)=2024` 无法使用索引   |
| 避免 `!=`、`<>`      | 可能导致索引失效                       |
| `LIKE` 避免左模糊    | `LIKE '%x'` 无法使用索引               |
| 使用 `EXISTS` 替代 `IN` | 子查询数据量大时效率更高            |
| 小表驱动大表         | JOIN时小表作为驱动表                   |
| 避免 `OR`            | 改用 `UNION ALL`                       |
