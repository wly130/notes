## MongoDB

### 常用操作

- **创建数据库**

```sql
use 数据库名
```

- **删除数据库**

```sql
db.collection.drop()
```

- **进入数据库**

```sql
use 数据库名
```

- **创建集合**

```sql
db.createCollection("集合名", <options>)
```

**options: 指定有关内存大小及索引的选项**

- **删除集合**

```sql
db.集合名.drop()
```

- **添加文档**

```sql
db.集合名.insert({}) #添加一个对象
db.集合名.insert([{}]) #添加数组对象
db.集合名.insertOne({}) #只能添加一个对象
db.集合名.insertMany([{}]) #只能添加数组对象
```

- **删除文档**

```sql
db.集合名.remove(<条件>, { 
	justOne: <boolean>
})
```

**justOne**: `true` 时, 删除一条符合条件的文档, `false` 时, 删除所有符合条件的文档

- **修改文档**

```sql
db.集合名.update(
   <条件>,
   <update>,
   {
   	 upsert: <boolean>,
     multi: <boolean>
   }
)
```

**update** : 修改的对象和操作符

**upsert** : 如果不存在是否插入, true为插入，默认是false，不插入

**multi** : 默认是false,只第一条记录，为true,多条记录全部更新

- **查询所有文档**

```sql
db.集合名.find(<查询条件>)
db.集合名.find(<查询条件>).pretty() #格式化查询
```

### 查询操作

#### 逻辑操作符

- **`AND` 查询 (查询条件都满足的文档)**

```sql
db.集合名.find({
	key1: value1,
    key2: value2
})
```

- **`OR` 查询 (查询满足其中一个条件的文档)**

```sql
db.集合名.find({
	$or: [{
		key: value
    }] 
})
```

- **`NOT` 查询 (查询与表达式不匹配的文档)**

```sql
db.集合名.find({
	age: {
		$not: {
            key: value
        }
    }
})
```

- **`NOR` 查询 (查询与表达式都不匹配的文档)**

```sql
db.集合名.find({
	age: {
		$nor: [{
            key1: value1
        }, {
            key2: value2
        }]
    }
})
```

#### 条件操作符

| 操作符    | 意义     		| 操作符   | 意义     |
|--------|------------|--------|------------|
| **`$gt`**  | 大于(>) 		| **`$lt`**	| 小于(<)		|
| **`$gte`** | 大于等于(>=) | **`$lte`**	| 小于等于(<=) |
| **`$ne`** | 不等于(!=) 	| **`$eq`**	| 等于(=)		|
| **`$in`** 	| 等于数组其中之一 | **`$nin`**	| 不等于数组任何一个 |

- **查询 `age` 大于10的文档**

```sql
db.集合名.find({
	age: {
		$gt: 10
	} 
})
```

- **查询 `age` 匹配数组某一个的文档**

```sql
db.集合名.find({
	age: {
		$in: [10, 11, 12]
	} 
})
```

#### `$type` 操作符

| 操作符         | 数字 | 意义   |
| -------------- | ---- | ------ |
| **double**     | 1    | 小数   |
| **string**     | 2    | 字符串 |
| **object**     | 3    | 对象   |
| **array**      | 4    | 数组   |
| **bindata**    | 5    |        |
| **objectid**   | 7    |        |
| **boolean**    | 8    | 布尔   |
| **date**       | 9    | 时间   |
| **null**       | 10   | 空值   |
| **javascript** | 13   | js代码 |
| **int**        | 16   | 整数   |
| **long**       | 18   |        |

- **查询 `title` 的类型为 `“string”` 的文档**

```sql
db.集合名.find({
    title: {
        $type: "string"
    }
})
```

#### 分页

- **查询第 `n`~`m` 个的文档**

```sql
db.集合名.find().skip(n-1).limit(m)
```

#### 排序

- **`1` 为升序(1~10)**
- **`-1` 为降序(10~1)**

```sql
db.集合名.find().sort({<key>: 1/-1})
```

#### `$regex` 关键字

- **查询包涵某个关键字的文档**

```sql
db.集合名.find({
    title: {
        $regex: /关键字/
    }
})
```

- **查询某个关键字开头的文档**

```sql
db.集合名.find({
    title: {
        $regex: /^关键字/
    }
})
```

- **查询某个关键字结尾的文档**

```sql
db.集合名.find({
    title: {
        $regex: /关键字^/
    }
})
```

