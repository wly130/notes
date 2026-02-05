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
const {Sequelize, DataTypes, Op} = require("sequelize");

const DIALECT = "mysql"; //数据库类型
const DATABASETABLE = 'my_project'; //数据库表
const HOSTNAME = 'root'; //用户名
const PASSWORD = '000000'; //密码
const HOST = '127.0.0.1'; //数据库地址
const PORT = 3306; //端口号

const seq = new Sequelize(
	DATABASETABLE,
	HOSTNAME,
	PASSWORD, {
	host: HOST,
	port: PORT,
	dialect: DIALECT,
	pool: {
		max: 5,
		min: 0,
		acquire: 3000
	}
});

const db = {
	DataTypes: DataTypes,
	sequelize: seq,
	Op: Op
};

module.exports = db;
```

- **封装数据库(models/index.js)**

```js
const db = require("../config/mysql-config");

const m_type = db.sequelize.define("m_type", { //数据表名
	id: { //字段名
		type: db.DataTypes.INTEGER, //数据类型
		primaryKey: true //是否为主键
        allowNull: false, //是否允许为空
        autoIncrement: true, //自动自增
        unique: true, //是否唯一
        comment: 'id',//备注
        validate: {
			is: /^[a-z]+$/i,          // 匹配这个 RegExp
			is: ["^[a-z]+$",'i'],     // 与上面相同,以字符串构造 RegExp
			not: /^[a-z]+$/i,         // 不匹配 RegExp
			not: ["^[a-z]+$",'i'],    // 与上面相同,但是以字符串构造 RegExp
			isEmail: true,            // 检查 email 格式
			isUrl: true,              // 检查 url 格式
			isIP: true,               // 检查 IPv4 或 IPv6 格式
			isIPv4: true,             // 检查 IPv4 格式
			isIPv6: true,             // 检查 IPv6 格式
			isAlpha: true,            // 只允许字母
			isAlphanumeric: true,     // 将仅允许使用字母数字
			isNumeric: true,          // 只允许数字
			isInt: true,              // 检查有效的整数
			isFloat: true,            // 检查有效的浮点数
			isDecimal: true,          // 检查任何数字
			isLowercase: true,        // 检查小写
			isUppercase: true,        // 检查大写
			notNull: true,            // 不允许为空
			isNull: true,             // 只允许为空
			notEmpty: true,           // 不允许空字符串
			equals: 'text',           // 字符串判断
			contains: 'text',         // 强制特定子字符串
			notIn: [['text', 'bar']], // 检查值不是这些之一
			isIn: [['text', 'bar']],  // 检查值是其中之一
			notContains: 'bar',       // 不允许特定的子字符串
			len: [2, 10],             // 仅允许长度在2到10之间的值
			isUUID: 4,                // 只允许 uuid
			isDate: true,             // 只允许日期字符串
			isAfter: "2011-11-05",    // 仅允许特定日期之后的日期字符串
			isBefore: "2011-11-05",   // 仅允许特定日期之前的日期字符串
			max: 23,                  // 仅允许值 <= 23
			min: 23,                  // 仅允许值 >= 23
			isCreditCard: true,       // 检查有效的信用卡号
			//自定义验证器
      		isEven(value) {
      			if (parseInt(value) % 2 !== 0) {
      		    	throw new Error('');
      		  	}
      		}
    	}
	},
	type: {
		type: db.DataTypes.STRING,
        allowNull: false
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

| 名称     | 数据类型 |
| -------- | -------- |
| STRING   | VARCHAR  |
| TEXT     | TEXT     |
| TINYTEXT | TINYTEXT |
| INTEGER  | INT      |
| BIGINT   | BIGINT   |
| FLOAT    | FLOAT    |
| DOUBLE   | DOUBLE   |
| DATE     | DATETIME |
| DATEONLY | DATE     |
| JSON     | JSON     |

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
 * 修改 id=1 的 name 为 "name2"
 * UPDATE 表名 SET name="name2" WHERE id=1;
 */
表名.update({ name: "name2" }, {
	where: {
		id: 1
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
const { 表名 } = require('../models/xxx');
const Op = db.Op;
```

#### 查询类型

```javascript
表名.count(); //返回总数数字
表名.max("字段名"); //返回最大值
表名.min("字段名"); //返回最小值
表名.sum("字段名"); //返回总和
表名.findAll(); //返回所有数据
表名.findByPk(id); //返回符合主键Id的所有数据
表名.findOne(); //返回第一条数据
表名.findOrCreate(); //先查询,如果没有就添加一条新数据
表名.findAndCountAll(); //返回总数和所有数据
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

#### `WHERE` 操作符

```javascript
表名.findAll({
	where: {
   		[Op.and]: { id: 5, name: "name" }, // id = 5 AND name = "name"
   		[Op.or]: { id: 5, name: "name" }, // id = 5 OR name = "name"
   		name: {
			[Op.eq]: 3, // name = 3
			[Op.ne]: 20, // name != 20
			[Op.is]: null, // name IS NULL
			[Op.not]: true, // name IS NOT TRUE
			[Op.or]: [5, 6], // name = 5 OR name = 6
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
			[Op.like]: '文字', // name LIKE '文字'
			[Op.notLike]: '文字', // name NOT LIKE '文字'
		}
	}
});
```

#### 排序

```javascript
/**
 * SELECT * FROM 表名 ORDER BY 字段名 ASC/DESC;
 */
表名.findAll({
	order: [['id', 'DESC/ASC']]
});
```

#### 分页

```javascript
/**
 * SELECT * FROM 表名 LIMIT startPage, pageSize;
 */
let startPage = (page - 1) * pageSize;
表名.findAll({
    offset: startPage,
    limit: pageSize
});
```

### 关联查询

```javascript
const A = db.sequelize.define("A", {
	id: {
		type: db.DataTypes.INTEGER,
		primaryKey: true,
        autoIncrement: true
	},
    type_id: {
		type: db.DataTypes.INTEGER,
		primaryKey: true
	},
	name: {
		type: db.DataTypes.STRING
	}
}, {
	tableName: 'A',
	timestamps: false
});
const B = db.sequelize.define("B", {
	type_id: {
		type: db.DataTypes.INTEGER,
		primaryKey: true,
        autoIncrement: true
	},
	type: {
		type: db.DataTypes.STRING
	}
}, {
	tableName: 'B',
	timestamps: false
});
const A_B = db.sequelize.define("A_B", {
    a_b_id: {
		type: db.DataTypes.INTEGER,
		primaryKey: true,
        autoIncrement: true
	},
	id: {
		type: db.DataTypes.INTEGER
	},
	type_id: {
		type: db.DataTypes.INTEGER
	}
}, {
	tableName: 'A_B',
	timestamps: false
});
//一对一关联,外键在B
A.hasOne(B, {
    as: "B", //别名
	foreignKey: 'type_id',
	targetKey: 'type_id',
    onDelete: 'RESTRICT', //删除时
  	onUpdate: 'RESTRICT' //更新时
});
//一对一关联,外键在A, A属于B
A.belongsTo(B, {
	foreignKey: 'type_id',
	targetKey: 'type_id'
});
//一对多关联,外键在B
A.hasMany(B, {
	foreignKey: 'type_id',
	targetKey: 'type_id'
});
//多对多关联
A.belongsToMany(B, {
	through: 'A_B' //关联表名
});
```

```javascript
A.findAll({
	include: {
        model: B,
        as: 'B'
    }
});
```

### 运行函数

```javascript
await 表名.findAll({
	attributes: [[db.sequelize.fn('函数名', db.sequelize.col('参数')), '别名']]
});
```

### 事务

```js
const t = await sequelize.transaction();
try {
	await 表名.create({ }, { transaction: t });
	await 表名.destroy({
	    where: {
			id: 1
	  	},
        transaction: t
	});
    await 表名.findAll({
	    where: {
			id: 1
	  	},
        transaction: t
	});
	//提交事务
	await t.commit();
} catch (err) {
  	//回滚事务
  	await t.rollback();
}
```

