## Sequelize

- **`Node.js` 的 ORM 数据库框架,支持 MySQL、PostgreSQL、SQLite、MSSQL 等**

### 初始化

- **下载依赖包**

```shell
npm install mysql2 -S
npm install sequelize -S
```

- **数据库连接(config/db.js)**

```javascript
const { Sequelize, DataTypes, Op } = require("sequelize");

const sequelize = new Sequelize('my_project', 'root', '000000', {
    host: '127.0.0.1',       //数据库地址
    port: 3306,              //端口号
    dialect: 'mysql',        //数据库类型
    logging: false,          //关闭日志
    pool: {                  //连接池配置
        max: 5,              //最大连接数
        min: 0,              //最小连接数
        acquire: 30000,      //获取连接超时时间(ms)
        idle: 10000          //空闲连接释放时间(ms)
    }
});

//测试连接
sequelize.authenticate()
    .then(() => console.log('数据库连接成功'))
    .catch(err => console.log('数据库连接失败', err));

module.exports = { sequelize, DataTypes, Op };
```

- **定义模型(models/user.js)**

```javascript
const { sequelize, DataTypes } = require("../config/db");

const User = sequelize.define("User", {
    id: {
        type: DataTypes.INTEGER,
        primaryKey: true,      //主键
        autoIncrement: true,   //自增
        allowNull: false,      //不允许为空
        comment: '用户ID'      //备注
    },
    name: {
        type: DataTypes.STRING,
        allowNull: false,
        unique: true           //唯一约束
    },
    email: {
        type: DataTypes.STRING,
        validate: {
            isEmail: true      //验证器:必须为邮箱格式
        }
    }
}, {
    tableName: 'user',         //指定表名
    timestamps: false          //关闭createdAt/updatedAt
});

module.exports = User;
```

- **字段配置项**

| 配置项           | 说明                     |
| ---------------- | ------------------------ |
| type             | 数据类型                 |
| primaryKey       | 是否为主键               |
| autoIncrement    | 是否自增                 |
| allowNull        | 是否允许为空             |
| unique           | 是否唯一                 |
| defaultValue     | 默认值                   |
| comment          | 字段备注                 |
| validate         | 验证规则                 |

- **常用验证器**

| 验证器             | 说明                     |
| ------------------ | ------------------------ |
| isEmail            | 邮箱格式                 |
| isUrl              | URL格式                  |
| isInt              | 整数                     |
| isFloat            | 浮点数                   |
| len: [min, max]    | 字符串长度范围           |
| isIn: [[values]]   | 值在指定列表中           |
| notEmpty           | 不允许空字符串           |
| is: /regex/        | 匹配正则                 |

### 数据类型

| Sequelize 类型   | MySQL 类型  | 说明           |
| ---------------- | ----------- | -------------- |
| **STRING**       | VARCHAR     | 字符串(255)    |
| **STRING(64)**   | VARCHAR(64) | 指定长度字符串 |
| **TEXT**         | TEXT        | 长文本         |
| **INTEGER**      | INT         | 整数           |
| **BIGINT**       | BIGINT      | 长整数         |
| **FLOAT**        | FLOAT       | 单精度浮点     |
| **DOUBLE**       | DOUBLE      | 双精度浮点     |
| **DECIMAL**      | DECIMAL     | 精确小数       |
| **BOOLEAN**      | TINYINT(1)  | 布尔值         |
| **DATE**         | DATETIME    | 日期时间       |
| **DATEONLY**     | DATE        | 仅日期         |
| **JSON**         | JSON        | JSON数据       |
| **UUID**         | CHAR(36)    | UUID           |
| **ENUM**         | ENUM        | 枚举           |

### 添加数据

```javascript
//添加单条
// INSERT INTO user (id, name) VALUES (1, "Tom");
await User.create({ id: 1, name: "Tom" });

//添加多条
// INSERT INTO user (id, name) VALUES (1, "Tom"), (2, "Jerry");
await User.bulkCreate([
    { id: 1, name: "Tom" },
    { id: 2, name: "Jerry" }
]);
```

### 修改数据

```javascript
//按条件修改
// UPDATE user SET name="Jerry" WHERE id=1;
await User.update({ name: "Jerry" }, { where: { id: 1 } });

//批量修改
// UPDATE user SET status=0 WHERE age > 30;
await User.update({ status: 0 }, { where: { age: { [Op.gt]: 30 } } });

//查找后修改
const user = await User.findByPk(1);
user.name = "NewName";
await user.save();
```

### 删除数据

```javascript
//按条件删除
// DELETE FROM user WHERE id=1;
await User.destroy({ where: { id: 1 } });

//删除所有数据(清空表)
await User.destroy({ truncate: true });
```

### 单表查询

#### 查询方法

| 方法               | 说明                       |
| ------------------ | -------------------------- |
| findAll()          | 查询所有数据               |
| findOne()          | 查询第一条匹配数据         |
| findByPk(id)       | 按主键查询                 |
| findOrCreate()     | 查询,不存在则创建          |
| findAndCountAll()  | 查询数据+返回总数          |
| count()            | 返回总数                   |
| max(field)         | 返回最大值                 |
| min(field)         | 返回最小值                 |
| sum(field)         | 返回总和                   |

#### 查询特定字段

```javascript
// SELECT id, name FROM user;
await User.findAll({ attributes: ['id', 'name'] });

// SELECT id AS userId, name FROM user; (字段别名)
await User.findAll({ attributes: [['id', 'userId'], 'name'] });

//排除特定字段
await User.findAll({ attributes: { exclude: ['password'] } });
```

#### WHERE 操作符(Op)

```javascript
const { Op } = require("sequelize");

await User.findAll({
    where: {
        // 逻辑运算
        [Op.and]: [{ id: 5 }, { name: "Tom" }],   // AND
        [Op.or]: [{ id: 5 }, { name: "Tom" }],    // OR
        // 比较运算
        id: {
            [Op.eq]: 3,              // = 3
            [Op.ne]: 20,             // != 20
            [Op.gt]: 6,              // > 6
            [Op.gte]: 6,             // >= 6
            [Op.lt]: 10,             // < 10
            [Op.lte]: 10,            // <= 10
            [Op.between]: [6, 10],   // BETWEEN 6 AND 10
            [Op.notBetween]: [1, 5], // NOT BETWEEN 1 AND 5
            [Op.in]: [1, 2, 3],      // IN (1, 2, 3)
            [Op.notIn]: [1, 2],      // NOT IN (1, 2)
            [Op.is]: null,           // IS NULL
            [Op.not]: null           // IS NOT NULL
        },
        // 字符串匹配
        name: {
            [Op.like]: '%Tom%',      // LIKE '%Tom%'
            [Op.notLike]: '%Tom%',   // NOT LIKE '%Tom%'
            [Op.startsWith]: 'Tom',  // LIKE 'Tom%'
            [Op.endsWith]: 'Tom'     // LIKE '%Tom'
        }
    }
});
```

#### 排序

```javascript
// SELECT * FROM user ORDER BY id DESC, name ASC;
await User.findAll({
    order: [['id', 'DESC'], ['name', 'ASC']]
});
```

#### 分页

```javascript
// SELECT * FROM user LIMIT 10 OFFSET 20;
let page = 3, pageSize = 10;
await User.findAll({
    offset: (page - 1) * pageSize,
    limit: pageSize
});

//带总数的分页查询
const { count, rows } = await User.findAndCountAll({
    offset: (page - 1) * pageSize,
    limit: pageSize
});
```

### 关联查询

#### 关联类型

| 方法              | 说明                 | 外键位置   |
| ----------------- | -------------------- | ---------- |
| hasOne()          | 一对一               | 目标表     |
| belongsTo()       | 一对一(反向)         | 源表       |
| hasMany()         | 一对多               | 目标表     |
| belongsToMany()   | 多对多               | 中间表     |

#### 定义关联

```javascript
//一对一: User有一个Profile
User.hasOne(Profile, { foreignKey: 'user_id', as: 'profile' });
Profile.belongsTo(User, { foreignKey: 'user_id' });

//一对多: User有多篇Post
User.hasMany(Post, { foreignKey: 'user_id', as: 'posts' });
Post.belongsTo(User, { foreignKey: 'user_id' });

//多对多: User和Role通过中间表关联
User.belongsToMany(Role, { through: 'user_role', foreignKey: 'user_id' });
Role.belongsToMany(User, { through: 'user_role', foreignKey: 'role_id' });
```

#### 关联查询(include)

```javascript
//查询User及其Profile(一对一)
await User.findAll({
    include: [{ model: Profile, as: 'profile' }]
});

//查询User及其所有Post(一对多)
await User.findAll({
    include: [{ model: Post, as: 'posts', attributes: ['title'] }]
});

//嵌套关联
await User.findAll({
    include: [{
        model: Post,
        as: 'posts',
        include: [{ model: Comment, as: 'comments' }]
    }]
});
```

### 聚合与分组

```javascript
//分组统计
await User.findAll({
    attributes: ['status', [sequelize.fn('COUNT', sequelize.col('id')), 'count']],
    group: ['status']
});

//聚合函数
await User.count({ where: { status: 1 } });
await User.max('age');
await User.sum('score', { where: { status: 1 } });
```

### 钩子(Hooks)

```javascript
//定义钩子
User.beforeCreate(async (user, options) => {
    //创建前执行,如加密密码
});
User.afterCreate(async (user, options) => {
    //创建后执行
});

//常用钩子
```

| 钩子                 | 触发时机         |
| -------------------- | ---------------- |
| beforeCreate         | 创建前           |
| afterCreate          | 创建后           |
| beforeUpdate         | 更新前           |
| afterUpdate          | 更新后           |
| beforeDestroy        | 删除前           |
| afterDestroy         | 删除后           |
| beforeBulkCreate     | 批量创建前       |
| afterBulkCreate      | 批量创建后       |

### 事务

```javascript
const t = await sequelize.transaction();
try {
    await User.create({ name: "Tom" }, { transaction: t });
    await User.destroy({ where: { id: 1 } }, { transaction: t });
    await User.update({ status: 1 }, { where: { id: 2 } }, { transaction: t });
    //提交事务
    await t.commit();
} catch (err) {
    //回滚事务
    await t.rollback();
}
```

- 所有操作必须传入 `{ transaction: t }` 参数
- 任一步骤失败则整体回滚,保证数据一致性

### 原始SQL查询

```javascript
//查询
const [results] = await sequelize.query('SELECT * FROM user WHERE id = :id', {
    replacements: { id: 1 },
    type: sequelize.QueryTypes.SELECT
});

//增删改
await sequelize.query('UPDATE user SET name = :name WHERE id = :id', {
    replacements: { name: 'Tom', id: 1 },
    type: sequelize.QueryTypes.UPDATE
});
```
