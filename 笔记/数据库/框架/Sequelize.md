## Sequelize

- **`Node.js` 的数据库框架**

### 初始化

- **下载依赖包**

```shell
npm install mysql2 -S
npm install sequelize -S
```

- **数据库连接(config/mysql-config.js)**

```js
const {
	Sequelize,
	DataTypes, //数据类型
    Op //操作符
} = require("sequelize");

const sequelize = new Sequelize(
	'my_project', //数据库名
	'root', //用户名
	'000000', { //密码
		host: '127.0.0.1', //数据库地址
		port: '3306',  //端口号
		dialect: "mysql", //数据库类型
		pool: {
			max: 5,
			min: 0,
			acquire: 3000
		}
	}
);
const db = {};
db.DataTypes = DataTypes; //字段类型
db.sequelize = sequelize;
db.Op;

module.exports = db;
```

- **封装数据库(models/index.js)**

```js
const db = require("../config/mysql-config");

const m_type = db.sequelize.define(
    "m_type", { //数据表名
	id: { //字段名
		type: db.DataTypes.INTEGER(10), //数据类型
		primaryKey: true //是否为主键
        allowNull: false, //是否允许为空
        primaryKey: true, //是否为主键
        autoIncrement: true, //自动自增
        unique: true, //是否唯一
        comment: 'id' //备注
	},
	type: {
		type: db.DataTypes.STRING(255)
	}
}, {
	tableName: 'm_type',
	timestamps: false
});

module.exports = {
	m_type
}
```

### 数据类型

| 名称    | 类型     |
| ------- | -------- |
| STRING  | VARCHAR  |
| TEXT    | TEXT     |
| INTEGER | INTEGER  |
| BIGINT  | BIGINT   |
| FLOAT   | FLOAT    |
| DATE    | DATETIME |

### 添加数据

```javascript
/**
 * 添加单条数据
 * INSERT INTO 表名 (id，name) VALUES (1，"name");
 */
表名.create({
    id: 1,
    name: "name"
});

/**
 * 添加多条数据
 * INSERT INTO 表名 (id，name) VALUES (1，"name1"), (2，"name2");
 */
表名.bulkCreate({
    id: 1,
    name: "name1"
}, {
    id: 2,
    name: "name2"
});
```

### 修改数据

```javascript
/**
 * 修改 name="name1" 为 "name2"
 * UPDATE 表名 SET name="name1" WHERE name="name1";
 */
表名.update({ name: "name2" }, {
	where: {
		name: "name1"
  	}
});
```

### 删除数据

```javascript
/**
 * 删除单条数据
 * DELETE FROM 表名 WHERE id=1;
 */
表名.destroy({
    where: {
		id: 1
  	}
});

/**
 * 删除所有数据
 */
表名.destroy({
    truncate: true
});
```

### 单表查询

#### 初始化

```javascript
const {
	表名
} = require('../models/xxx');
const Op = db.Op;
```

#### 查询所有数据

```javascript
/**
 * SELECT * FROM 表名;
 */
表名.findAll();
```

#### 查询特定字段的数据

```javascript
/**
 * SELECT id, name FROM 表名;
 */
表名.findAll({
	attributes: ['id', 'name']
});
```

#### 查询字段别名的数据

```javascript
/**
 * SELECT userid AS id FROM 表名;
 */
表名.findAll({
	attributes: [['userid', 'id']]
});
```

#### 查询数据总数

```javascript
/**
 * SELECT COUNT(id) AS count FROM 表名;
 */
表名.findAll({
	attributes: [[db.sequelize.fn('COUNT', db.sequelize.col('id')), 'count']]
});
```

#### `WHERE` 操作符

```javascript
表名.findAll({
	where: {
   		[Op.and]: [{ id: 5 }, { name: "name" }], // (id = 5) AND (name = "name")
   		[Op.or]: [{ id: 5 }, { name: "name" }], // (id = 5) OR (name = "name")
   		name: {
			[Op.eq]: 3, // name = 3
			[Op.ne]: 20, // name != 20
			[Op.is]: null, // name IS NULL
			[Op.not]: true, // name IS NOT TRUE
			[Op.or]: [5, 6], // name (name = 5) OR (name = 6)
			// 数字比较
			[Op.gt]: 6, // name > 6
			[Op.gte]: 6, // name >= 6
			[Op.lt]: 10, // name < 10
			[Op.lte]: 10, // name <= 10
			[Op.between]: [6, 10], // name BETWEEN 6 AND 10
			[Op.notBetween]: [11, 15], // name NOT BETWEEN 11 AND 15
			// 其它操作符
			[Op.all]: sequelize.literal('SELECT 1'), // name > ALL (SELECT 1)
			[Op.in]: [1, 2], // name IN [1, 2]
			[Op.notIn]: [1, 2], // name NOT IN [1, 2]
			[Op.like]: '%文字', // name LIKE '%hat'
			[Op.notLike]: '%文字', // name NOT LIKE '%hat'
		}
	}
});
```

#### 排序

```javascript
/**
 * SELECT name FROM 表名 ORDER BY 字段名 ASC/DESC;
 */
表名.findAll({
    ['name'],
	order: [['id', 'DESC/ASC']]
});
```

#### 显示第 `n` 行后面 `m` 个数据

```javascript
/**
 * SELECT * FROM 表名 LIMIT n,m;
 */
表名.findAll({
    offset: n,
    limit: m
});
```

#### 显示第 `n` 行后面 `m` 个数据和总数

```javascript
/**
 * SELECT * FROM 表名 LIMIT n,m;
 * SELECT COUNT(*) AS count FROM 表名;
 */
表名.findAndCountAll({
	offset: n,
	limit: m,
	where: {}
});
```

### 多表查询

#### 一对一关联

```javascript
const A = db.sequelize.define("A", {
	id: {
		type: db.DataTypes.INTEGER(10),
		primaryKey: true
	},
	type: {
		type: db.DataTypes.STRING(255)
	}
}, {
	tableName: 'A',
	timestamps: false
});

const B = db.sequelize.define("B", {
	typeId: {
		type: db.DataTypes.INTEGER(10),
		primaryKey: true
	},
	type: {
		type: db.DataTypes.STRING(255)
	}
}, {
	tableName: 'B',
	timestamps: false
});

/**
 * A表 关联 B表
 * A.id 对应 B,typeId
 */
A.belongsTo(B, {
	foreignKey: 'id',
	targetKey: 'typeId'
});

/**
 * SELECT * FROM A LEFT OUTER JOIN B ON A.typeId= B.id;
 */
A.findAll({
    include: [{
		model: B,
        attributes: ['id', 'name']
	}]
});
```

