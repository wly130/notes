## Pug模板

### 使用

- **安装**

```shell
npm install pug -g
npm install pug-cli -g
```

- **自动编译代码，生成`html`文件**

```shell
pug -P -w xxx.pug
```

### 语法

#### 注释

- **模板注释**

```jade
//-给模板注释
```

- **页面注释**

```jade
//页面
```

#### 标签

```jade
doctype html
html(lang='en')
    head  
        meta(charset="UTF-8")
        title Pug模板
    body 
        p 这是p标签
        //id和class
        div#id.className 这是div标签
        //属性
        img(src="", alt="")
        //行内样式
        div(style={color: 'red', background: 'green'})
```

#### 变量

- **定义变量**

```jade
- let key = 10
```

- **循环遍历**

```jade
- let index = 10
- for(let i = 0; i < index; i++)
  	span= i
```

- **条件语句**

```jade
- let flag = true
if flag
    p true
else 
    p false 
```

- **插值转义**

```jade
- let str = '<b>'
//转义
div= a    	//<div>&lt;b&gt;</div>
div #{a}	
//非转义
div !{a}	//div><b></div>
```

- **分支条件**

```jade
- let value = 0
case value
    when 1
        p 1
    when 2
        p 2
    default
        p null
```

