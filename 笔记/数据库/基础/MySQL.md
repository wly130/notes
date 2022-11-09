## MySQL

### 常用操作

- **创建数据库**

```sql
CREATE DATABASE 数据库名;
```

- **删除数据库**

```sql
DROP DATABASE 数据库名;
```

- **进入数据库**

```sql
USE 数据库名;
```

- **查看某个数据库**

```sql
SHOW CREATE DATABASE 数据库名;
```

- **查看所有数据库**

```sql
SHOW DATABASES;
```

- **修改数据库编码方式**  

```sql
ALTER DATABASE 数据库名 DEFAULT CHARACTER SET 编码方式;
```

- **创建数据表**

```sql
CREATE TABLE 表名 (
    字段1 数据类型(范围) <NULL>,
    字段2 数据类型(范围) <NULL>
    ...
);
```

- **删除数据表**

```sql
DROP TABLE 表名;
```

- **修改字段名**

```sql
ALTER TABLE 表名 CHANGE 旧字段名 新字段名 新数据类型;
```

- **修改表名**

```sql
ALTER TABLE 旧表名 RENAME 新表名;
```

- **修改数据类型**

```sql
ALTER TABLE 表名 MODIFY 字段名 数据类型();
```

- **添加字段名**

```sql
ALTER TABLE 表名 ADD 新字段名 数据类型();
```

- **删除字段名**

```sql
ALTER TABLE 表名 DROP 字段名;
```

- **查看表结构**

```sql
DESC 表名;
```

- **添加数据**

```sql
INSERT INTO 表名 (字段1，字段2) VALUES (值1，值2);
```

- **删除数据**

```sql
DELETE FROM 表名 WHERE 条件;
```

- **修改数据**

```sql
UPDATE 表名 SET 字段名1=值1，字段名2=值2 WHERE 条件;
```

- **查询所有数据**

```sql
SELECT * FROM 表名 WHERE 条件;
```

### 约束

- **NOT NULL 非空约束**

```sql
ALTER TABLE 表名 MODIFY 字段名 数据类型 NOT NULL;
ALTER TABLE 表名 MODIFY 字段名 数据类型; #删除约束
```

- **UNIQUE 唯一约束**

```sql
ALTER TABLE 表名 ADD UNIQUE (字段名);
ALTER TABLE 表名 DROP [INDEX/KEY] 字段名; #删除约束
```

- **PRIMARY KEY 唯一非空(主键)约束**

```sql
ALTER TABLE 表名 ADD PRIMARY KEY (字段名);
ALTER TABLE 表名 DROP PRIMARY KEY; #删除约束
```

- **FOREIGN KEY 外键约束**

```sql
ALTER TABLE 表名 ADD FOREIGN KEY (字段名) REFERENCES 外表名(外表字段名);
ALTER TABLE 表名 DROP FOREIGN KEY 字段名; #删除约束
```

- **DEFAULT 默认值约束**

```sql
ALTER TABLE 表名 ALTER 字段名 SET DEFAULT '默认值';
ALTER TABLE 表名 ALTER 字段名 DROP DEFAULT; #删除约束
```

- **自增长**

```sql
ALTER TABLE 表名 MODIFY 字段名 数据类型 AUTO_INCREMENT;
ALTER TABLE 表名 MODIFY 字段名 数据类型; #删除自增长
```

### 创建索引

```sql
ALTER TABLE 表名 ADD 索引类型 别名 (字段名);
CREATE 索引类型 INDEX 别名 ON 表名 (字段名);
```

| 单词        | 索引类型     |
| :---------- | ------------ |
| unique      | **唯一索引** |
| fulltext    | **全文索引** |
| spatial     | **空间索引** |
| index / key | **字段索引** |

| 数据语句 | 用途                                       |
| :------: | ------------------------------------------ |
| **DML**  | 数据操作语言语句,插入、修改、删除等        |
| **DDL**  | 数据定义语言语句,`create`、`alter`、`drop` |
| **DCL**  | 数据控制语言语句,`grant`、`revoke `语句    |

### 数据类型

<table>
        <tr>
            <th></th>
            <th>数据类型</th>
        </tr>
        <tr>
            <td>TINYINT</td>
            <td rowspan="5">整数类型</td>
        </tr>
        <tr>
            <td>SMALLINT</td>
        </tr>
        <tr>
            <td>MEDIUMINT</td>
        </tr>
        <tr>
            <td>INT</td>
        </tr>
        <tr>
            <td>BIGI</td>
        </tr>
        <tr>
            <td>FLOAT</td>
            <td rowspan="2">浮点数类型</td>
        </tr>
        <tr>
            <td>DOUBLE</td>
        </tr>
        <tr>
            <td>DECIMAL</td>
            <td>定点数类型</td>
        </tr>
        <tr>
            <td>YEAR</td>
            <td rowspan="5">日期/时间类型</td>
        </tr>
        <tr>
            <td>TIME</td>
        </tr>
        <tr>
            <td>DATE</td>
        </tr>
        <tr>
            <td>DATETIME</td>
        </tr>
        <tr>
            <td>TIMESTAMP</td>
        </tr>
</table>

| 字符串类型 | 二进制类型 |
| :--------- | :--------- |
|    CHAR    |    BIT     |
|  VARCHAR   |   BINARY   |
|   BINARY   | VARBINARY  |
| VARBINARY  |  TINYBLOB  |
|    BLOB    |    BLOB    |
|    TEXT    | MEDIUMBLOB |
|    ENUM    |  LONGBLOB  |
|    SET     ||

### 单表查询

- **查询指定字段**

```sql
SELECT 字段1,字段2 FROM 表名;
```

- **条件查询**

```sql
SELECT 字段1,字段2 FROM 表名 WHERE 条件;
```

- **查询指定范围数据**

```sql
SELECT 字段1,字段2 FROM 表名 WHERE 字段 BETWEEN 值1 AND 值2;
```

- **空值查询**

```sql
SELECT * FROM 表名 WHERE 字段名 IS NULL;
```

- **重复查询**

```sql
SELECT DISTINCT 字段1,字段2 FROM 表名;
```

- **字符查询**

```sql
SELECT * FROM 表名 WHERE 字段名 LIKE '%字符';
```

- **数据表设置别名**

```sql
SELECT 表名.字段名 FROM 表名 AS 别名;
```

- **字段设置别名**

```sql
SELECT 字段名 AS 别名 FROM 表名;
```

- **显示前 n 行数据**

```sql
SELECT * FROM 表名 LIMIT n;
```

- **显示第 n 行后面 m 个数据**

```sql
SELECT * FROM 表名 LIMIT n－1,m;
```

- **查询结果排序**[^1]
  
  [^1]:ASC: 升序&nbsp;&nbsp;DESC: 降序

```sql
SELECT * FROM 表名 ORDER BY 字段名 ASC / DESC;
```

- **查询多个结果**

```sql
SELECT * FROM 表名 WHERE 字段名 IN (value1, value2);
```

- **连接多条查询语句**

```sql
第一条 UNION 第二条
```

- **复制表内容**

```sql
INSERT INTO 表2 SELECT * FROM 表1;
```

### 多表查询

<img src="https://www.runoob.com/wp-content/uploads/2013/09/img_innerjoin.gif" alt="avatar" style="float:left" />

- **INNER JION 查询**

```sql
#如果表中有至少一个匹配，则返回行
SELECT 列名1,列名2 FROM 表名1 INNER JOIN 表名2;
```

<img src="https://www.runoob.com/wp-content/uploads/2013/09/img_leftjoin.gif" alt="avatar" style="float:left" />

- **LEFT JION 查询**

```sql
#即使右表中没有匹配的也从左表返回所有行
SELECT 列名1,列名2 FROM 左表 LEFT JOIN 右表;
```

<img src="https://www.runoob.com/wp-content/uploads/2013/09/img_rightjoin.gif" alt="avatar" style="float:left" />

- **RIGHT JION 查询**

```sql
#即使左表中没有匹配也从右表返回所有的行
SELECT 列名1,列名2 FROM 左表 RIGHT JION 右表;
```

<img src="https://www.runoob.com/wp-content/uploads/2013/09/img_fulljoin.gif" alt="avatar" style="float:left" />

- **FULL JION 查询**

```sql
#只要其中一个表中存在匹配，则返回行
SELECT 列名1,列名2 FROM 左表 FULL OUTER JOIN 右表;
```

- **分组查询**

```sql
SELECT 列名1,列名2 FROM 表名1 INNER JOIN 表名2 GROUP BY 字段名;
```

### 存储过程

- **创建存储过程**

```sql
DEIMITER //
    CREATE PROCEDURE 过程名(参数)
    BEGIN
         过程内容;
    END//
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

- **变量赋值**

```sql
SET @变量名=值;
```

- **存储函数**

```sql
CREATE functioFUNCTION 存储函数名(参数)
```

| 参数  | 作用         |
| :---: | ------------ |
|  IN   | 输入参数     |
|  OUT  | 输出参数     |
| INOUT | 输入输出参数 |

### 数据库备份

```sql
select * FROM 数据库名.表名 into outfilte '备份位置';
```

### 视图

- **创建视图**

```sql
create view 视图名 as <select语句>;
```

- **查看视图**

```sql
select * FROM 视图名;
```

- **修改视图**

```sql
create or replace view 视图名 as <新select语句>;
alter view 视图名 as <新select语句>;
```

- **删除视图**

```sql
drop view 视图名;
```

### SQL 函数

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
