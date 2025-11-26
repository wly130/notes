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
ALTER DATABASE 数据库名 DEFAULT CHARACTER SET 编码方式;
```

#### 创建数据表

```sql
CREATE TABLE 表名 (
    字段1 数据类型(范围) <NULL>,
    字段2 数据类型(范围) <NULL>
    ...
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

#### 添加字段名

```sql
ALTER TABLE 表名 ADD 新字段名 数据类型();
```

#### 删除字段名

```sql
ALTER TABLE 表名 DROP 字段名;
```

#### 查看表结构

```sql
DESC 表名;
```

#### 添加数据

```sql
INSERT INTO 表名 (字段1，字段2) VALUES (值1，值2);
INSERT INTO 表名 (字段1) VALUES (1); #添加数字
INSERT INTO 表名 (字段1) VALUES ('name'); #添加字符串
INSERT INTO 表名 (字段1) VALUES ('1000-01-01 00:00:00'); #添加时间
INSERT INTO 表名 (字段1) VALUES ('{"key":"value"}'); #添加JSON
INSERT IGNORE INTO 表名 (字段1，字段2) VALUES (值1，值2); #添加多条数据时,忽略报错数据,继续添加其余数据
```

#### 删除数据

```sql
DELETE FROM 表名 WHERE 条件;
```

#### 修改数据

```sql
UPDATE 表名 SET 字段名1=值1，字段名2=值2 WHERE 条件;
```

#### 查询所有数据

```sql
SELECT * FROM 表名 WHERE 条件;
```

### 约束

#### NOT NULL 非空约束

```sql
ALTER TABLE 表名 MODIFY 字段名 数据类型 NOT NULL;
ALTER TABLE 表名 MODIFY 字段名 数据类型; #删除约束
```

#### UNIQUE 唯一约束

```sql
ALTER TABLE 表名 ADD UNIQUE (字段名);
ALTER TABLE 表名 DROP [INDEX/KEY] 字段名; #删除约束
```

#### PRIMARY KEY 主键约束

```sql
ALTER TABLE 表名 ADD PRIMARY KEY (字段名);
ALTER TABLE 表名 DROP PRIMARY KEY; #删除约束
```

#### FOREIGN KEY 外键约束

```sql
ALTER TABLE 表名 ADD FOREIGN KEY (字段名) REFERENCES 外表名(外表字段名);
ALTER TABLE 表名 DROP FOREIGN KEY 字段名; #删除约束
```

#### DEFAULT 默认值约束

```sql
ALTER TABLE 表名 ALTER 字段名 SET DEFAULT '默认值';
ALTER TABLE 表名 ALTER 字段名 DROP DEFAULT; #删除约束
```

#### 自增长

```sql
ALTER TABLE 表名 MODIFY 字段名 数据类型 AUTO_INCREMENT;
ALTER TABLE 表名 MODIFY 字段名 数据类型; #删除自增长
```

### 索引

- **创建索引**

```sql
ALTER TABLE 表名 ADD 索引类型 别名 (字段名);
CREATE 索引类型 INDEX 别名 ON 表名 (字段名);
```

| 单词         | 索引类型     | 作用                                                         |
| :----------- | ------------ | ------------------------------------------------------------ |
| **UNIQUQE**  | **唯一索引** | **索引列的值必须唯一，但允许有空值**                         |
| **FULLTEXT** | **全文索引** | **在定义索引的列上支持值的全文查找，允许在这些索引列中插入重复值和空值** |
| **SPATIAL**  | **空间索引** |                                                              |
| **KEY**      | **字段索引** | **允许在定义索引的列中插入重复值和空值**                     |

| 数据语句 | 用途                                       |
| :------- | ------------------------------------------ |
| **DML**  | 数据操作语言语句,插入、修改、删除等        |
| **DDL**  | 数据定义语言语句 `create`、`alter`、`drop` |
| **DCL**  | 数据控制语言语句 `grant`、`revoke ` 语句   |

### 数据类型

#### 数值

| 类型          | 大小       | 范围（有符号）                  | 范围（无符号） | 用途           |
| ------------- | ---------- | ------------------------------- | -------------- | -------------- |
| TINYINT       | **1Bytes** | (-128，127)                     | (0，255)       | 小整数值       |
| SMALLINT      | **2Bytes** | (-2^15^，2^15^)                 | (0 , 2^16^)    | 大整数值       |
| MEDIUMINT     | **3Bytes** | (-2^23^，2^23^)                 | (0 , 2^24^)    | 大整数值       |
| INT / INTEGER | **4Bytes** | (-2^31^，2^31^)                 | (0，2^24^)     | 大整数值       |
| BIGINT        | **8Bytes** | (-2^63^，2^63^)                 | (0，2^64^)     | 极大整数值     |
| FLOAT         | **4Bytes** | (-3.4 * 10^38^，3.4 * 10^38^)   |                | 单精度浮点数值 |
| DOUBLE        | **8Bytes** | (-1.7 * 10^308^，1.7 * 10^308^) |                | 双精度浮点数值 |

#### 日期时间

| 类型     | 大小       | 范围                                       | 格式                | 用途             |
| -------- | ---------- | ------------------------------------------ | ------------------- | ---------------- |
| DATE     | **3Bytes** | 1000-01-01~9999-12-31                      | YYYY-MM-DD          | 日期值           |
| TIME     | **3Bytes** | -838:59:59~838:59:59                       | HH:MM:SS            | 时间值或持续时间 |
| YEAR     | **1Bytes** | 1901~2155                                  | YYYY                | 年份值           |
| DATETIME | **8Bytes** | 1000-01-01 00:00:00 ~ 9999-12-31 23:59:59' | YYYY-MM-DD hh:mm:ss | 混合日期和时间值 |

#### 字符串

| 类型       | 大小(Bytes)  | 用途                            |
| ---------- | ------------ | ------------------------------- |
| CHAR       | 0-255        | 定长字符串                      |
| VARCHAR    | 0-65535      | 变长字符串                      |
| TINYBLOB   | 0-255        | 不超过 255 个字符的二进制字符串 |
| TINYTEXT   | 0-255        | 短文本字符串                    |
| BLOB       | 0-65535      | 二进制形式的长文本数据          |
| TEXT       | 0-65535      | 长文本数据                      |
| MEDIUMBLOB | 0-16777215   | 二进制形式的中等长度文本数据    |
| MEDIUMTEXT | 0-16777215   | 中等长度文本数据                |
| LONGBLOB   | 0-4294967295 | 二进制形式的极大文本数据        |
| LONGTEXT   | 0-4294967295 | 极大文本数据                    |

#### JSON

```json
{
    "key": "value",
    "arr": [1, 2, 3]
}
```

### 单表查询

#### 查询指定字段

```sql
SELECT 字段1,字段2 FROM 表名;
```

#### 条件查询

```sql
SELECT 字段1,字段2 FROM 表名 WHERE 条件;
```

#### 查询指定范围数据

```sql
SELECT 字段1,字段2 FROM 表名 WHERE 字段 BETWEEN 值1 AND 值2;
```

#### 空值查询

```sql
SELECT * FROM 表名 WHERE 字段名 IS NULL;
```

#### 重复查询

```sql
SELECT DISTINCT 字段1,字段2 FROM 表名;
```

#### 字符查询

```sql
SELECT * FROM 表名 WHERE 字段名 LIKE '%x%'; #查询包含x的数据
SELECT * FROM 表名 WHERE 字段名 LIKE '%x'; #查询以x结尾的数据
SELECT * FROM 表名 WHERE 字段名 LIKE 'x%'; #查询以x开头的数据
SELECT * FROM 表名 WHERE 字段名 LIKE '_x'; #查询长度为2,以x结尾的数据
```

#### 正则表达式查询

```sql
SELECT * FROM 表名 WHERE 字段名 REGEXP '^x'; #查询以x开头的数据
SELECT * FROM 表名 WHERE 字段名 REGEXP 'x$'; #查询以x结尾的数据
SELECT * FROM 表名 WHERE 字段名 REGEXP '[1-9]'; #查询包含括号内的数据
```

#### 数据表设置别名

```sql
SELECT 表名.字段名 FROM 表名 AS 别名;
```

#### 字段设置别名

```sql
SELECT 字段名 AS 别名 FROM 表名;
```

#### 显示前 n 行数据

```sql
SELECT * FROM 表名 LIMIT n;
```

#### 显示第 n 行后面 m 个数据

```sql
SELECT * FROM 表名 LIMIT n－1,m;
```

#### 查询结果排序[^1]

[^1]:ASC: 升序&nbsp;&nbsp;DESC: 降序

```sql
SELECT * FROM 表名 ORDER BY 字段名 ASC / DESC;
```

#### 查询 `JOSN`

```sql
SELECT * FROM 表名 WHERE key->'$.key' = 'value';
```

#### 查询 `Array`

```sql
SELECT * FROM 表名 WHERE key ->'$[*].key' = 'value';
SELECT * FROM 表名 WHERE JSON_CONTAINS(key, JSON_OBJECT('key', "value"));
```

#### 查询多个结果

```sql
SELECT * FROM 表名 WHERE 字段名 IN (value1, value2);
```

#### 连接多条查询语句

```sql
第一条 UNION 第二条
```

#### 复制表内容

```sql
INSERT INTO 表2 SELECT * FROM 表1;
```

### 多表查询

<img src="https://www.runoob.com/wp-content/uploads/2013/09/img_innerjoin.gif" alt="avatar" style="float:left" />

#### INNER JION 查询(内连接)

```sql
#如果表中有至少一个匹配，则返回行
SELECT 列名1,列名2 FROM 表名1 INNER JOIN 表名2;
```

<img src="https://www.runoob.com/wp-content/uploads/2013/09/img_leftjoin.gif" alt="avatar" style="float:left" />

#### LEFT JION 查询(左连接)

```sql
#即使右表中没有匹配的也从左表返回所有行
SELECT 列名1,列名2 FROM 左表 LEFT JOIN 右表;
```

<img src="https://www.runoob.com/wp-content/uploads/2013/09/img_rightjoin.gif" alt="avatar" style="float:left" />

#### RIGHT JION 查询(右连接)

```sql
#即使左表中没有匹配也从右表返回所有的行
SELECT 列名1,列名2 FROM 左表 RIGHT JION 右表;
```

<img src="https://www.runoob.com/wp-content/uploads/2013/09/img_fulljoin.gif" alt="avatar" style="float:left" />

#### FULL OUTER JION 查询(全连接)

```sql
#只要其中一个表中存在匹配，则返回行
SELECT 列名1,列名2 FROM 左表 FULL OUTER JOIN 右表;
```

#### 分组查询

```sql
SELECT 列名1,列名2 FROM 表名1 INNER JOIN 表名2 GROUP BY 字段名;
```

### 存储过程

- **封装方法**

#### 实现

- **创建存储过程**

```sql
DEIMITER $
    CREATE PROCEDURE 过程名(参数)
    BEGIN
         过程内容;
    END$
DEIMITER;
```

- **调用存储过程**

```sql
CALL 过程名(参数);
```

- **删除存储过程**

```sql
DROP PROCEDURE 过程名;
```

- **变量**

```sql
DECLARE  变量名 数据类型 DEFAULT 默认值; #定义
SET @变量名=值; #赋值
SELECT @变量名:=值; #赋值
```

- **存储函数**

```sql
CREATE FUNCTION 存储函数名(参数)
```

| 参数  | 作用         |
| :---- | ------------ |
| IN    | 输入参数     |
| OUT   | 输出参数     |
| INOUT | 输入输出参数 |

#### 实例

- **`IN` 参数**

```sql
DEIMITER $
    CREATE PROCEDURE test(IN num INT)
    BEGIN
		SELECT num * 2;
    END$
DEIMITER;
```

- **`OUT` 参数**

```sql
DEIMITER $
    CREATE PROCEDURE test(OUT num INT)
    BEGIN
		SET num = 1; # 需要初始化
		SELECT num * 2;
		SELECT 字段名 INTO num FROM 表名; # INTO 赋值
    END$
DEIMITER;
```

- **`INOUT` 参数**

```sql
DEIMITER $
    CREATE PROCEDURE test(INOUT num INT)
    BEGIN
         SELECT num * 2;
    END$
DEIMITER;
```

### 事务

#### 特点

- **原子性：**一个事务中的所有操作，要么全部完成，要么全部不完成，不会结束在中间某个环节。事务在执行过程中发生错误，会被回滚到事务开始前的状态。
- **一致性：**在事务开始之前和事务结束以后，数据库的完整性没有被破坏。
- **隔离性：**数据库允许多个并发事务同时对其数据进行读写和修改的能力，隔离性可以防止多个事务并发执行时由于交叉执行而导致数据的不一致。
- **持久性：**事务处理结束后，对数据的修改就是永久的，即便系统故障也不会丢失。

#### 实现

- **开启事务**

```sql
SET autocommit = 0; #设置停止自动提交
START TRANSACTION;
```

- **操作**

```sql
INSERT INTO 表名 (字段1，字段2) VALUES (值1，值2);
UPDATE 表名 SET 字段名1=值1，字段名2=值2 WHERE 条件;
```

- **提交/回滚事务**

```sql
COMMIT; #提交事务
ROLLBACK; #回滚事务
```

#### 隔离级别

- **脏读：**事务中的修改，即使没有提交，对其他事务也都是可见的。事务可以读取未提交的数据。
- **不可重复读：**一个事务从开始直到提交之前，所做的任何修改对其他事务都是不可见的。但是两次执行同样的查询，可能会得到不一样的结果。
- **幻读：**当某个事务在读取某个范围内的记录时，另外一个事务又在该范围内插入了新的记录。

| 隔离级别             | 作用                                                         |
| -------------------- | ------------------------------------------------------------ |
| **READ UNCOMMITTED** | 可能读到其他会话未提交事务修改的数据                         |
| **READ COMMITTED**   | 只能查询到已提交的数据                                       |
| **REPEATABLE READ**  | 无论何时查到的数据都和第一次查到的数据一致                   |
| **SERIALIZABLE**     | 所有的事务依次逐个执行，事务之间互不干扰。但会导致读和写都会阻塞，性能极低 |

| 隔离级别                  | 脏读 | 不可重复读 | 幻读 |
| ------------------------- | ---- | ---------- | ---- |
| **READ UNCOMMITTED**      | √    | √          | √    |
| **READ COMMITTED**        | ×    | √          | √    |
| **REPEATABLE READ(默认)** | ×    | ×          | √    |
| **SERIALIZABLE**          | ×    | ×          | ×    |

- **查看隔离级别**

```sql
SELECT @@GLOBAL.transaction_isolation, @@transaction_isolation;
```

#### 实例

```sql
SET autocommit = 0;
START TRANSACTION;
INSERT INTO 表名 (字段1，字段2) VALUES (值1，值2);
UPDATE 表名 SET 字段名1=值1，字段名2=值2 WHERE 条件;
COMMIT; #提交事务
ROLLBACK; #回滚事务
```

### 视图

- **可以复用查询代码**

#### 实现

- **创建视图**

```sql
CREATE VIEW 视图名 AS <select语句>;
```

- **查看视图**

```sql
SELECT * FROM 视图名;
```

- **修改视图**

```sql
CREATE OR REPLACE VIEW 视图名 AS <新select语句>;
ALTER VIEW 视图名 AS <新select语句>;
```

- **删除视图**

```sql
DROP VIEW 视图名;
```

#### 实例

```sql
CREATE VIEW 视图名 AS 
SELECT * FROM 表名 WHERE 条件;
SELECT * FROM 视图名;
```

### SQL 函数

- [函数列表](https://www.runoob.com/mysql/mysql-functions.html)

| 函数名                    | 作用                                     |
| ------------------------- | ---------------------------------------- |
| **avg()**                 | 返回平均值                               |
| **abs()**                 | 返回绝对值                               |
| **count()**               | 返回行数                                 |
| **first()**               | 返回第一个记录的值                       |
| **last()**                | 返回最后一个记录的值                     |
| **max()**                 | 返回最大值                               |
| **min()**                 | 返回最小值                               |
| **mon()**                 | 返回余数                                 |
| **sqrt()**                | 返回 n²                                  |
| **pow()**                 | 返回 n^n^                                |
| **sum()**                 | 返回总和                                 |
| **rand()**                | 返回随机数                               |
| **ucase()**               | 将某个字段转换为大写                     |
| **lcase()**               | 将某个字段转换为小写                     |
| **mid()**                 | 从某个文本字段提取字符                   |
| **SubString(字段,1,end)** | 从某个文本字段提取字符                   |
| **len()**                 | 返回某个文本字段的长度                   |
| **round()**               | 对某个数值字段进行指定小数位数的四舍五入 |
| **now()**                 | 返回当前的系统日期和时间                 |
| **format()**              | 格式化某个字段的显示方式                 |

- **IF()判断**

```sql
SELECT IF(条件, true, false);
```

- **CASE()函数**

```sql
# 有字段
SELECT
	CASE 字段名
		WHEN 值1 THEN 1
		WHEN 值1 THEN 2
		ELSE 0 
	END
FROM
	表名;
# 无字段
SELECT
	CASE
		WHEN 条件1 THEN 1
		WHEN 条件2 THEN 2
		ELSE 0 
	END
FROM
	表名;
```

- **WHILE()循环**

```sql
DECLARE i INT DEFAULT 0;
WHILE i<=3 DO
	SET i = i + 1;
END WHILE;
```

### 数据库备份

```sql
SELECT * FROM 数据库名.表名 INTO outfilte '备份位置';
```