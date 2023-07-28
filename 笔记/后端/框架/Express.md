## Express框架

- **基于 [Node.js](https://nodejs.org/en/) 平台，快速、开放、极简的 Web 开发框架**

### 创建项目

- **安装依赖包**

```shell
npm install -g express-generator  #全局安装
npm install --save express
```

- **创建项目**

```shell
express 项目名称
npm install  #安装依赖
```

- **目录结构**

<img src="../../img/express目录结构.png" style="zoom:100%;float:left;" />

| 文件名           | 功能                                                         |
| ---------------- | ------------------------------------------------------------ |
| **bin/www**      | 启动文件,包含引用要启动的应用、设置应用监听的端口和启动http服务 |
| **assets**       | 上传文件目录                                                 |
| **public**       | 静态资源文件目录                                             |
| **routes**       | 路由文件                                                     |
| **views**        | 视图文件                                                     |
| **app.js**       | 初始化文件                                                   |
| **package.json** | 配置文件                                                     |

- **启动项目**

```shell
npm run start
```

- **监控端口状态**

```js
app.listen(3000, 'localhost', () => {
	console.log('服务已开启,端口号:3000');
});
```

**默认访问地址为 `http://localhost:3000/`**

### 请求头

#### 设置请求头

```js
app.get('/url', (req, res) => {
    res.header("Content-Type", "application/json;");
});
```

### 网络请求

#### GET请求

```js
const express = require('express');
const app = express();

app.get('/url/:nam', (req, res) => {
    let query = req.query; //请求参数
    let params = req.params; //路径参数
	res.json(); //响应参数
});

module.exports = app;
```

#### POST请求

- **`www-form-urlencoded` 格式**

```js
const express = require('express');
const bodyParser = require('body-parser');
const app = express();

app.use(bodyParser.urlencoded({
	extended: true
}))

app.post("/url", (req, res, next) => {
    let data = req.body; //请求参数
	res.json(); //响应参数
});

module.exports = app;
```

- **`form-data` 格式**

```shell
npm install multer -S
```

```js
const express = require('express'); //导入框架
const multer = require('multer');
//获取图片地址,http开头
const path = require("path");
const app = express();
app.use(express.static(path.join(__dirname, "assets")));

const storage = multer.diskStorage({
	destination: (req, file, cb) => {
		cb(null, 'assets/'); //上传文件目录.根目录下
	},
	filename: (req, file, cb) => {
        //自定义 时间戳+文件名
		cb(null, Date.now() + '-' + file.originalname);
	}
});
const upload = multer({
	storage: storage
});

app.post('/uploadFile', upload.single("file"), (req, res) => {
    res.send({
		file: req.file //文件详情
	});
});

module.exports = app;
```

- **`application/json` 格式**

```js
const express = require('express');
const bodyParser = require('body-parser');
const app = express();

app.use(bodyParser.json()); //解析 JOSN 格式

app.post('/url', (req, res) => {});

module.exports = app;
```

- **请求参数**

| 参数名           | 功能                |
| ---------------- | ---------------------------|
|req.app|当callback为外部文件时，用req.app访问express的实例|
|req.baseUrl|获取路由当前安装的URL路径|
|req.body / req.cookies | 获得 请求主体 / Cookies |
|req.fresh / req.stale|判断请求是否还「新鲜」|
|req.hostname / req.ip|获取主机名和IP地址|
|req.originalUrl|获取原始请求URL|
|req.params|获取路由的params|
|req.path|获取请求路径|
|req.protocol|获取协议类型|
| req.query|获取URL参数|
| req.route|获取当前匹配的路由|
| req.subdomains|获取子域名|
| req.accepts()|检查可接受的请求的文档类型|
| req.acceptsCharsets |返回指定字符集的第一个可接受字符编码|
| req.get()|获取指定的HTTP请求头|
| req.is()|判断请求头Content-Type的MIME类型|

- **响应参数**

| 参数名           | 功能                |
| ---------------- | ---------------------------|
|res.app|同req.app一样|
|res.append()|追加指定HTTP头|
|res.set()|在res.append()后将重置之前设置的头|
|res.cookie()|设置Cookie|
|res.clearCookie()|清除Cookie|
|res.download()|传送指定路径的文件|
|res.get()|返回指定的HTTP头|
|res.json()|传送JSON响应|
| res.jsonp()|传送JSONP响应|
| res.location()|只设置响应的Location HTTP头，不设置状态码或者close response|
| res.redirect()|设置响应的Location HTTP头，并且设置状态码302|
| res.render() |渲染一个view，同时向callback传递渲染后的字符串|
| res.send()|传送HTTP响应|
| res.sendFile() |传送指定路径的文件|
| res.set()|设置HTTP头，传入object可以一次设置多个头|
| res.status()|设置HTTP状态码|
| res.type()|设置Content-Type的MIME类型|

### 数据库操作(Sequelize框架)

- **下载依赖包**

```shell
npm install mysql2 -S
npm install sequelize -S
```

- **数据库连接(config/mysql-config.js)**

```js
const {
	Sequelize,
	DataTypes,
	Op
} = require("sequelize");

const databaseName = 'my_project';
const hostName = 'root';
const password = '000000';
const host = '127.0.0.1';
const port = 3306;

const seq = new Sequelize(
	databaseName,
	hostName,
	password, {
	host: host,
	port: port,
	dialect: "mysql",
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
		type: db.DataTypes.INTEGER(10), //数据类型
		primaryKey: true //是否为主键
	},
	type: {
		type: db.DataTypes.STRING(255),
        allowNull: false //是否为空
	}
}, {
	tableName: 'm_type',
	timestamps: false
});

module.exports = {
	m_type
}
```

- **app.js**

```js
const express = require('express'); //导入框架
const { m_type } = require("./models/xxx");  //SQL操作函数
const { Op } = require("sequelize");
const app = express();
app.use(express.json());

//添加
app.post("/add", (req, res, next) => {
    let body = req.body; //请求参数
    
    (async () => {
		const data = await m_type.create({
			id: body.id,
			type: body.type,
		})
		res.json({
			data
		});
	})();
});

//删除
app.post("/del", (req, res, next) => {
    let body = req.body; //请求参数
    
    (async () => {
		const data = await m_type.destroy({
			where: {
                id: body.id
            }
		});
		res.json({
			data
		});
	})();
});

//修改
app.post("/updata", (req, res, next) => {
    let body = req.body; //请求参数
    
    (async () => {
		const data = await m_type.update({
			id: body.id
		}, {
			where: {
				type: body.type
			}
		});
		res.json({
			data
		});
	})();
});

//查询
app.get("/select", (req, res, next) => {
    let query = req.query; //请求参数
    
    (async () => {
		const data = await m_type.findAll({
			where: {
				type: body.type
			}
		});
		res.json({
			data
		});
	})();
});

module.exports = app;
```

### 路由

- **app.js**

```js
const express = require('express'); //导入框架
//导入路由表
const xxRouter = require('./routes/路由表');

const app = express();
app.use('/xxx', xxRouter);

module.exports = app;
```

- **/routes/路由表.js**

```js
const express = require('express'); //导入框架
const exec = require('../public/mysql/config'); //SQL操作函数
const router = express.Router();

//获取单词列表
router.get("/url", (req, res, next) => {
	res.send();
});

module.exports = router;
```

- **访问接口: `http:localhost:3000/xxx/url`**
