## MongoDB

- **基于文档的 NoSQL 数据库，使用 BSON(类JSON)格式存储数据**

### 常用操作

- **创建/进入数据库**

```javascript
use 数据库名
```

- **删除数据库**

```javascript
db.dropDatabase()
```

- **查看所有数据库**

```javascript
show dbs
```

- **创建集合**

```javascript
db.createCollection("集合名")
```

- **删除集合**

```javascript
db.集合名.drop()
```

- **添加文档**

```javascript
db.集合名.insert({})           //添加一个对象
db.集合名.insert([{}, {}])     //添加数组对象
db.集合名.insertOne({})        //只能添加一个对象
db.集合名.insertMany([{}, {}]) //只能添加数组对象
```

- **删除文档**

```javascript
db.集合名.deleteOne({条件})    //删除一条
db.集合名.deleteMany({条件})   //删除多条
db.集合名.remove({条件}, { justOne: true })  //旧语法
```

| 参数        | 说明                                   |
| ----------- | -------------------------------------- |
| justOne     | true删除一条，false删除所有(默认false) |

- **修改文档**

```javascript
db.集合名.update(
    {条件},
    {更新操作},
    {
        upsert: false,  //不存在时是否插入
        multi: false    //是否更新多条
    }
)
db.集合名.updateOne({条件}, {更新操作})   //更新一条
db.集合名.updateMany({条件}, {更新操作})  //更新多条
```

- **查询文档**

```javascript
db.集合名.find({条件})            //查询多条
db.集合名.findOne({条件})         //查询一条
db.集合名.find({条件}).pretty()   //格式化输出
```

### 查询操作符

#### 条件操作符

| 操作符     | 意义             | 示例                           |
| ---------- | ---------------- | ------------------------------ |
| **$eq**    | 等于(=)          | `{age: {$eq: 18}}`             |
| **$ne**    | 不等于(!=)       | `{age: {$ne: 18}}`             |
| **$gt**    | 大于(>)          | `{age: {$gt: 18}}`             |
| **$gte**   | 大于等于(>=)     | `{age: {$gte: 18}}`            |
| **$lt**    | 小于(<)          | `{age: {$lt: 18}}`             |
| **$lte**   | 小于等于(<=)     | `{age: {$lte: 18}}`            |
| **$in**    | 在数组中         | `{age: {$in: [18, 20, 22]}}`   |
| **$nin**   | 不在数组中       | `{age: {$nin: [18, 20]}}`      |
| **$between** | 范围查询       | `{age: {$gte: 18, $lte: 30}}`  |

```javascript
//查询age大于18的文档
db.user.find({ age: { $gt: 18 } })

//查询age在18-30之间的文档
db.user.find({ age: { $gte: 18, $lte: 30 } })
```

#### 逻辑操作符

| 操作符   | 意义             | 示例                                   |
| -------- | ---------------- | -------------------------------------- |
| **$and** | 与(AND)          | `{$and: [{a: 1}, {b: 2}]}`             |
| **$or**  | 或(OR)           | `{$or: [{a: 1}, {b: 2}]}`              |
| **$not** | 非(NOT)          | `{age: {$not: {$gt: 18}}}`             |
| **$nor** | 或非(NOR)        | `{$nor: [{a: 1}, {b: 2}]}`             |

```javascript
//AND查询(多条件默认是AND)
db.user.find({ name: "Tom", age: 18 })
db.user.find({ $and: [{ name: "Tom" }, { age: 18 }] })

//OR查询
db.user.find({ $or: [{ name: "Tom" }, { name: "Jerry" }] })
```

#### 元素操作符

| 操作符        | 意义               | 示例                     |
| ------------- | ------------------ | ------------------------ |
| **$exists**   | 字段是否存在       | `{age: {$exists: true}}` |
| **$type**     | 字段类型匹配       | `{age: {$type: "int"}}`  |
| **$mod**      | 取模运算           | `{age: {$mod: [2, 0]}}`  |

```javascript
//查询存在email字段的文档
db.user.find({ email: { $exists: true } })
```

#### $type 类型值

| 类型         | 值/别名        |
| ------------ | -------------- |
| double       | 1              |
| string       | 2              |
| object       | 3              |
| array        | 4              |
| objectId     | 7              |
| boolean      | 8              |
| date         | 9              |
| null         | 10             |
| int          | 16             |
| long         | 18             |

#### 数组操作符

| 操作符      | 意义                   | 示例                         |
| ----------- | ---------------------- | ---------------------------- |
| **$all**    | 包含所有指定元素       | `{tags: {$all: ["a", "b"]}}` |
| **$size**   | 数组长度匹配           | `{tags: {$size: 3}}`         |
| **$elemMatch** | 数组元素匹配多条件  | `{scores: {$elemMatch: {$gt: 80}}}` |

```javascript
//查询tags包含"a"和"b"的文档
db.user.find({ tags: { $all: ["a", "b"] } })
```

#### 正则表达式

```javascript
//包含关键字
db.user.find({ name: { $regex: /关键字/ } })
db.user.find({ name: /关键字/ })  //简写

//以关键字开头
db.user.find({ name: /^关键字/ })

//以关键字结尾
db.user.find({ name: /关键字$/ })

//忽略大小写
db.user.find({ name: { $regex: /关键字/, $options: "i" } })
```

### 更新操作符

| 操作符        | 意义                   | 示例                           |
| ------------- | ---------------------- | ------------------------------ |
| **$set**      | 设置字段值             | `{$set: {name: "Tom"}}`        |
| **$unset**    | 删除字段               | `{$unset: {name: ""}}`         |
| **$inc**      | 增加字段值             | `{$inc: {age: 1}}`             |
| **$mul**      | 乘以字段值             | `{$mul: {price: 1.1}}`         |
| **$rename**   | 重命名字段             | `{$rename: {old: "new"}}`      |
| **$min**      | 仅在小于时更新         | `{$min: {score: 60}}`          |
| **$max**      | 仅在大于时更新         | `{$max: {score: 100}}`         |
| **$currentDate** | 设置为当前日期      | `{$currentDate: {updated: true}}` |

#### 数组更新操作符

| 操作符        | 意义                   | 示例                           |
| ------------- | ---------------------- | ------------------------------ |
| **$push**     | 添加元素到数组         | `{$push: {tags: "new"}}`       |
| **$addToSet** | 添加不重复元素         | `{$addToSet: {tags: "new"}}`   |
| **$pop**      | 删除首/尾元素          | `{$pop: {tags: 1}}`            |
| **$pull**     | 删除匹配的元素         | `{$pull: {tags: "old"}}`       |
| **$pullAll**  | 删除所有匹配的元素     | `{$pullAll: {tags: ["a","b"]}}` |

```javascript
//更新字段值
db.user.updateOne({ _id: 1 }, { $set: { name: "Tom", age: 20 } })

//增加字段值
db.user.updateOne({ _id: 1 }, { $inc: { age: 1, score: 10 } })

//删除字段
db.user.updateOne({ _id: 1 }, { $unset: { temp: "" } })

//数组添加元素
db.user.updateOne({ _id: 1 }, { $push: { tags: "new" } })

//数组删除元素
db.user.updateOne({ _id: 1 }, { $pull: { tags: "old" } })
```

### 投影与分页

#### 投影(指定返回字段)

```javascript
//只返回指定字段
db.user.find({}, { name: 1, age: 1 })       //1表示包含
db.user.find({}, { password: 0 })           //0表示排除
db.user.find({}, { name: 1, _id: 0 })       //排除_id
```

#### 排序

```javascript
db.user.find().sort({ age: 1 })    //升序(1)
db.user.find().sort({ age: -1 })   //降序(-1)
db.user.find().sort({ age: -1, name: 1 })  //多字段排序
```

#### 分页

```javascript
db.user.find().skip(10).limit(5)   //跳过10条，取5条
//等价于: 第3页，每页5条 (page=3, pageSize=5)
```

| 方法      | 说明               |
| --------- | ------------------ |
| skip(n)   | 跳过n条记录        |
| limit(n)  | 限制返回n条记录    |

#### 计数

```javascript
db.user.countDocuments({})              //统计总数
db.user.countDocuments({ status: 1 })   //按条件统计
db.user.estimatedDocumentCount()        //估算总数(更快)
```

### 聚合(aggregate)

```javascript
db.集合名.aggregate([
    { $match: {条件} },      //过滤
    { $group: {_id: "$字段", 统计: {$sum: 1}} },  //分组
    { $sort: {字段: 1} },    //排序
    { $skip: 10 },           //跳过
    { $limit: 5 },           //限制
    { $project: {字段: 1} }  //投影
])
```

#### 常用管道操作符

| 操作符       | 说明                     |
| ------------ | ------------------------ |
| **$match**   | 过滤文档(类似WHERE)      |
| **$group**   | 分组聚合                 |
| **$sort**    | 排序                     |
| **$project** | 投影(选择字段)           |
| **$limit**   | 限制数量                 |
| **$skip**    | 跳过数量                 |
| **$unwind**  | 拆分数组                 |
| **$lookup**  | 关联查询(类似JOIN)       |

#### 分组统计操作符

| 操作符       | 说明         |
| ------------ | ------------ |
| **$sum**     | 求和         |
| **$avg**     | 平均值       |
| **$min**     | 最小值       |
| **$max**     | 最大值       |
| **$first**   | 第一个值     |
| **$last**    | 最后一个值   |
| **$push**    | 添加到数组   |

```javascript
//按status分组统计数量
db.user.aggregate([
    { $group: { _id: "$status", count: { $sum: 1 } } }
])

//按status分组求年龄平均值
db.user.aggregate([
    { $group: { _id: "$status", avgAge: { $avg: "$age" } } }
])

//先过滤再分组
db.order.aggregate([
    { $match: { status: 1 } },
    { $group: { _id: "$userId", total: { $sum: "$amount" } } }
])

//关联查询(类似LEFT JOIN)
db.order.aggregate([
    { $lookup: {
        from: "user",          //关联集合
        localField: "userId",  //当前集合字段
        foreignField: "_id",   //关联集合字段
        as: "userInfo"         //输出字段名
    }}
])
```

### 索引

```javascript
//创建索引
db.user.createIndex({ name: 1 })           //单字段索引(1升序，-1降序)
db.user.createIndex({ name: 1, age: -1 })  //复合索引
db.user.createIndex({ email: 1 }, { unique: true })  //唯一索引

//查看索引
db.user.getIndexes()

//删除索引
db.user.dropIndex("索引名")
db.user.dropIndexes()  //删除所有索引
```

| 索引选项     | 说明               |
| ------------ | ------------------ |
| unique       | 唯一索引           |
| sparse       | 稀疏索引(忽略null) |
| expireAfterSeconds | TTL过期时间  |
| name         | 索引名称           |

### 常用命令

| 命令                   | 说明               |
| ---------------------- | ------------------ |
| show dbs               | 显示所有数据库     |
| show collections       | 显示所有集合       |
| db.stats()             | 数据库状态         |
| db.集合名.stats()      | 集合状态           |
| db.集合名.totalSize()  | 集合总大小         |
| db.version()           | MongoDB版本        |
