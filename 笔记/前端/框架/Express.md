## Express框架

- **基于 [Node.js](https://nodejs.org/en/) 平台，快速、开放、极简的 Web 开发框架**

### 创建项目

- **安装依赖包**

```shell
npm install -g express-generator  //全局安装
npm install --save express
```

- **创建项目**

```shell
express 项目名称
npm install  //安装依赖
```

- **目录结构**

<img src="../../img/express目录结构.png" style="zoom:100%;float:left;" />

| 文件名           | 功能                                                         |
| ---------------- | ------------------------------------------------------------ |
| **bin/www**      | 应用的启动文件,包含引用要启动的应用、设置应用监听的端口和启动http服务等 |
| **public**       | 应用的静态资源文件目录                                       |
| **routes**       | 应用的路由文件                                               |
| **views**        | 应用的视图文件                                               |
| **app.js**       | 应用的初始化文件                                             |
| **package.json** | 应用的配置文件                                               |

- **启动项目**

```shell
npm run start
```

**默认访问地址为`http://localhost:3000/`**

### 网络请求

#### GET请求

```js
const express = require('express');
const app = express();

app.get('/url', (req, res) => {
    let data = req.query; //请求参数
	res.send(); //响应参数
});

module.exports = app;
```

#### POST请求

- **`www-form-urlencoded` 格式**

```js
const express = require('express');
const bodyParser = require('body-parser');
const app = express();

app.use(bodyParser.urlencoded({ //解析 www-form-urlencoded 格式
	txtended: false
}));

app.post("/url", (req, res, next) => {
    let data = req.body; //请求参数
	res.send(); //响应参数
});

module.exports = app;
```

- **`form-data` 格式**

```shell
npm install connect-multiparty
```

```js
const express = require('express');
const app = express();
const multipart = require('connect-multiparty');
const multipartMiddleware = multipart(); //文件上传

app.post('/url', multipartMiddleware, (req, res) => {});

module.exports = app;
```

- **`application/json` 格式**

```js
var express = require('express');
var bodyParser = require('body-parser');
var app = express();

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

### 数据库操作

- **数据库连接封装(config.js)**

```js
const mysql = require('mysql');

/**
 * @method 建立连接
 * @param {string} sql SQL语句
 * @param {Array} params 变量数组
 * @param {function} callback 成功回调函数
 */
function exec(sql, params, callback) {
    const conn = mysql.createConnection({
        host: 'localhost',
        user: 'root',
        password: 'password',
        port: '3306',
        database: '数据库名'
    })
    conn.connect((err) => {
        if (err) {
            throw err;
        }
        //数据操作
        conn.query(sql, params, (err, res) => {
            if (err) {
                throw err;
            }
            //查询数据返回给回调函数
            callback && callback(res);
            //g
            conn.end((err) => {
                if (err) {
                    throw err;
                }
            });
        });
    });
}

module.exports = exec;
```

- **app.js**

```js
const express = require('express'); //导入框架
const exec = require('./config.js');  //SQL操作函数
const app = express();
app.use(express.json());

//请求访问接口
app.get/post("/url", (req, res, next) => {
    let body = req.body; //请求参数
    let params = []; //sql参数
    const sql = 'sql语句';
    exec(sql, params, (data) => {
        res.json({ data });
    })
});

module.exports = app;
```

​	
