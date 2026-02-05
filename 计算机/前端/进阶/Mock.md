## Mock

### 模拟接口返回的信息

### 语法

#### 字符串

```json
'name|min-max': string  //重复次数取值区间为 [min,max]
'name|count':string  //重复次数为 count
```

- **示例**

```js
Mock.mock({
	"name|1-10": "N",
    "str|5": "S"
})
=>
{
	"name": "NNN",
	"str": "SSSSS"
}
```

#### 数字

```json
'name|+1': number //属性值自增1，初始值是number
'name|min-max': number  //生成一个整数，取值区间为[min,max]
//生成一个浮点数，整数部分取值区间为 [min-max]
//小数部分保留 dmin 到 dmax 位
//小数点后只有一个数 n 的话，就保留 n 位小数。
'name|min-max.dmin-dmax': number  
```

- **示例**

```js
Mock.mock({
  	"number|+1": 1,
    "number|1-100": 100,
    "number|1-100.1-10": 1,
    "number|123.1-10": 1,
    "number|123.3": 1,
    "number|123.10": 1.123
})
=>
{
    "number": 2,
    "number": 34,
    "number": 23.3423,
    "number": 123.2343242,
    "number": 123.256,
    "number": 123.123453
}
```

#### Boolean

```json
'name|1': boolean  //随机生成一个布尔值，概率 50%
'name|min-max': value  //随机生成一个布尔值，概率 min / (min + ma)
```

#### 对象

```json
'name|count': object  //从 object 中随机选取 count 个属性
'name|min-max': object  //从 object 中选取 min 到 max 个属性
```

- **示例**

```js
Mock.mock({
	"object|2": {
    	"310000": "上海市",
        "320000": "江苏省",
        "330000": "浙江省",
        "340000": "安徽省"
	},
    "object|1-3": {
    	"310000": "上海市",
        "320000": "江苏省",
        "330000": "浙江省",
        "340000": "安徽省"
	}
})
=>
{
	"object": {
        "320000": "江苏省",
		"330000": "浙江省"
  	},
    "object": {
    	"310000": "上海市",
        "320000": "江苏省",
        "330000": "浙江省"
	}
}
```

#### 数组

```json
'name|1': array   //从 array 中随机选取1个元素，作为最终值
'name|+1': array  //从 array 中顺序选取 1 个元素，作为最终值。
'name|min-max': array  //通过重复属性值 array 生成一个新数组，重复次数大于等于 min，小于等于 max
'name|count': array  //通过重复属性值 array 生成一个新数组，重复次数为 count
```

- **示例**

```js
Mock.mock({
	"array|1": ["AMD", "CMD", "UMD"],
    "array|+1": ["AMD", "CMD", "UMD"],
    "array|1-10": ["A"],
    "array|3": ["A"],
})
=>
{
    "array": ["CMD"],
    "array": ["AMD"],
    "array": ["A", "A", "A", "A"],
    "array": ["A", "A", "A"]
}
```

#### 函数

```json
'name': function  //执行函数 function，取其返回值作为最终的属性值，函数的上下文为属性 ‘name’ 所在的对象
```

#### 数据占位符

**生成随机的值**

| 占位符              | 作用                               |
| ------------------- | ---------------------------------- |
| @clast              | 中文姓                             |
| @cfirst             | 中文名                             |
| @cname              | 中文姓名                           |
| @last               | 英文姓                             |
| @first              | 英文名                             |
| @name               | 英文姓名                           |
| @id                 | 身份证号                           |
| @title              | 中文title                          |
| @province           | 省                                 |
| @city               | 市                                 |
| @county             | 县                                 |
| @zip                | 邮政编码                           |
| @ip                 | IP 地址                            |
| @domain             | 域名                               |
| @email              | Email 地址                         |
| @url                | URL 地址                           |
| @csentence(min,max) | min 到 max 个字的中文句子          |
| @cparagraph         | 中文段落                           |
| @paragraph          | 英文段落                           |
| @string(n)          | 输出 n 个字符长度的字符串          |
| @float(min,max)     | min 到 max 之间的浮点数            |
| @integer(min,max)   | min 到 max 之间的整数              |
| @boolean            | true 或 false                      |
| @date               | 日期(yyyy-mm-dd)                   |
| @time               | 时间(hh:mm:ss)                     |
| @datetime           | 日期+时间(yyyy-mm-dd hh:mm:ss)     |
| @now                | 当前日期+时间(yyyy-mm-dd hh:mm:ss) |
| @image              | 图片链接                           |
| @color              | 颜色(十六进制)                     |

### node 使用 Mock

#### 安装依赖包

```shell
npm install express --save--dev
npm install mockjs
```

#### server.js

```js
let express = require('express'); //引入express模块
let Mock = require('mockjs'); //引入mock模块
let app = express(); //实例化express

app.all('/json', (req, res) => {
    res.json(Mock.mock({
        code: 200,
        data: {}
    }));
});

//为app添加中间件处理跨域请求
app.all('*', function (req, res, next) {
    res.header('Access-Control-Allow-Origin', '*');
    res.header('Access-Control-Allow-Headers', 'X-Requested-With, accept, origin, content-type');
    res.header('Access-Control-Allow-Methods', 'PUT,POST,GET,DELETE,OPTIONS');
    res.header('Access-Control-Allow-Headers', 'Content-Type');
    next();
})

// 监听8090端口
app.listen('8090');
```

#### 启动

```shell
node server.js
```

**访问 `http://localhost:8090/json` 获取json数据**

