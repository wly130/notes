- **Typora快捷键**

| 快捷键                          | 作用                    | 快捷键                               | 作用                    |
| ------------------------------- | ----------------------- | ------------------------------------ | ----------------------- |
| **<kbd>Ctrl + F</kbd>**         | **搜索**                | **<kbd>Ctrl + H</kbd>**              | **替换**                |
| **<kbd>Ctrl + T</kbd>**         | **插入表格**            | **<kbd>Ctrl + Shift + K</kbd>**      | **插入代码块**          |
| **<kbd>Ctrl + B</kbd>**         | **加粗**                | **<kbd>Ctrl + I</kbd>**              | **倾斜**                |
| **<kbd>Ctrl + Shift + I</kbd>** | **插入图片**            | **<kbd>Ctrl + K</kbd>**              | **插入链接**            |
| **<kbd>Ctrl + /</kbd>**         | **源码 / 编辑模式切换** | **<kbd>Ctrl + Shift + L</kbd>**      | **侧边栏显示/隐藏切换** |
| **<kbd>Ctrl + Shift + ]</kbd>** | **插入无序列表**        | **<kbd>Ctrl + Shift + [</kbd>**      | **插入有序列表**        |
| **<kbd>Ctrl + F</kbd>**         | **插入标题**            | **<kbd>Ctrl + U</kbd>**              | **下划线**              |
| **<kbd>Ctrl + L</kbd>**         | **选中一整行**          | **<kbd>Ctrl + D</kbd>**              | **选中单词**            |
| **<kbd>Ctrl + Home</kbd>**      | **跳到头部**            | **<kbd>Ctrl + End</kbd>**            | **跳到底部**            |
| **<kbd>Ctrl + Enter</kbd>**     | **插入下面一行表格**    | **<kbd>Ctrl + Shift + Delete</kbd>** | **删除表格当前行**      |

# 标题 1

## 标题 2

### 标题 3

#### 标题 4

##### 标题 5

###### 标题 6

_斜体文本_
**粗体文斜本**
**_粗斜体文本_**
~~删除线~~
<u>带下划线文本</u>
==标记==
**H~2~O**
**2^n^**

- **支持的代码语言**

| APL        | asciiarmor     | ASP                 | assembly         | bash              |
| ---------- | -------------- | ------------------- | ---------------- | ----------------- |
| basic      | **C**          | **C++**             | **C#**           | **CSS**           |
| cassandra  | ceylon         | clike               | clojure          | cmake             |
| cobol      | coffeescript   | commonlisp          | **cpp**          | CQL               |
| crystal    | csharp         | cypher              | cython           | D                 |
| **dart**   | diff           | django              | dockerfile       | dtd               |
| dylan      | ejs            | elixir              | elm              | embeddedjs        |
| erb        | erlang         | F#                  | **flow(流程图)** | forth             |
| fortran    | fsharp         | gas                 | gfm              | glsl              |
| go         | gherkin        | groovy              | **html**         | **http**          |
| handlebars | haskell        | haxe                | htaccess         | **hxml**          |
| idl        | **ini**        | jade                | **java**         | **javascript/js** |
| jinja2     | **json**       | **jsp**             | **jsx**          | julia             |
| **kotlin** | latex          | **less**            | lisp             | livescipt         |
| lua        | makefile       | mariadb             | **markdown**     | mathematica       |
| matlab     | mbox           | **mermaid(流程图)** | mssql            | **mysql**         |
| **nginx**  | nim            | nsis                | objc             | **objective-c**   |
| ocaml      | octave         | oz                  | pascal           | perl              |
| perl6      | pgp            | **php**             | php+HTML         | plsql             |
| powershell | properties     | protobuf            | pseudocode       | pseudocode        |
| **python** | **react**      | reStructuredText    | rst              | ruby              |
| rust       | SAS            | **SQL**             | scala            | scheme            |
| **scss**   | sequence       | **shell/sh**        | smalltalk        | solidity          |
| SPARQL     | spreadsheet    | sqlite              | squirrel         | **stylus**        |
| swift      | tcl            | tex                 | tiddlywiki       | toml              |
| tsx        | **typescript** | turtle              | twig             | **vbscript/vb**   |
| velocity   | verilog        | vhdl                | **vue**          | visual basic      |
| web-idl    | wiki           | xaml                | **xml**          | xml-dtd           |
| xquery     | yacas          | **yaml**            |                  |                   |

- **流程图**

| graph TB     | 从上到下     |
| ------------ | ------------ |
| **graph BT** | **从下到上** |
| **graph LR** | **从左到右** |
| **graph RL** | **从右到左** |

```mermaid
graph TD;
    A{菱形};
    B(圆角);
    C((圆));
    D-->|a=1| E[结果1];
    F>箭头];
    
    箭头-->H;
    无箭头---N;
    粗箭头==>J;
    虚线-.-P;
    虚箭头-.->S
```

- **状态图**

```mermaid
stateDiagram
[*] --> First
First --> Second
First --> Third

state First {
    [*] --> fir
    fir --> [*]
}
state Second {
    [*] --> sec
    sec --> [*]
}
state Third {
    [*] --> thi
    thi --> [*]
}
```

- **类图**

```mermaid
classDiagram
      总结 <|-- Vue
      总结 <|-- React
      总结 <|-- JQuery
      总结: info
      总结: test()
      class Vue{
          vueRouter
          vuex
          vue()
      }
      class React{
          ReactRouter
          Reactx
          React()
      }
      class JQuery{
         JQuery
         JQuery
         JQuery()
      }
```

- **标准流程图**

```flow
st=>start: 开始框
op=>operation: 处理框
cond=>condition: 判断框(是或否?)
sub1=>subroutine: 子流程
io=>inputoutput: 输入输出框
e=>end: 结束框

st->op->cond
cond(yes)->io->e
cond(no)->sub1(right)->op
```

- **饼图**

```mermaid
pie
title 饼图
"语文": 100
"数学": 85
"英语": 150
```

- **甘特图**

    **任务状态**

    - **done**     已完成
    - **active**   正在进行
    - **crit**        关键任务
    - **默认为`active`(正在进行状态)**

    **任务描述**

    - **des**      项目名称
    - **after**   表示在该项目之后

```mermaid
gantt
	dateFormat  YYYY-MM-DD
	title 软件开发工作流程图

	section 设计
	分析需求:done,des1, 2022-01-06,1d
	设计原型:done,des2, after des1, 1d
	UI设计:done,des3, after des2, 2d

	section 开发
	理解需求:done,des4, after des3, 1d
	设计框架:done, des5, after des4, 1d
	开发:done, des6, after des5, 5d

	section 测试
	本地测试:done, des7,after des6, 2d
	x测试:done,des8,after des7, 2d
```

- **时序图**

| 类型 | 描述               |
| ---- | ------------------ |
| ->   | 没有箭头的实线     |
| -->  | 没有箭头的虚线     |
| ->>  | 有箭头的实线       |
| -->> | 有箭头的虚线       |
| -x   | 末端有十字叉的实线 |
| --x  | 末端有十字叉的虚线 |

```mermaid
sequenceDiagram
	%%  autonumber 生成序号
	autonumber
	%%  participant 声明
	Title: 我是标题
	participant A as 老板
	participant B as 下属
	rect red
    A->B: 请求
    A-->B: 请求
    A->>B: 请求
    A-->>B: 请求
    end
    Note right of B: 下属的描述
	Note left of A: 老板的描述
	Note over A,B: 关系
    loop 循环
        B->>B: 循环
    end
    activate B
		A->>B: 激活
	deactivate B
	B-xA: 响应
	B--xA: 响应
	alt 判断
		A ->> B: 是
	else 不是
        A ->> B: 不是
	end
```

创建脚注格式类似这样 [^runoob]

[^runoob]:菜鸟教程

- 列表 1
    - 子列表
        - 子列表

* 列表 2

- 列表 3

1. 列表
2. 列表
3. 列表

- [x] **任务列表**
- [ ] **任务列表**

> 区块引用 1
>
> > 区块引用 2
> > `代码` 函数

 ```javascript
 $(document).ready(function () {
   	alert("RUNOOB");
 });
 ```

`带行号代码` 函数

```javascript
$(document).ready(function () {
	alert("RUNOOB");
});
```

行内代码  `alert('RUNOOB');`

这是一个链接 [菜鸟教程](https://www.runoob.com)
<https://www.runoob.com>

![RUNOOB 图标]()
<img src="" width="50%">
这个链接用 [^1] 作为网址变量 [RUNOOB][1].
然后在文档的结尾为变量赋值

[^1]: http://static.runoob.com/images/runoob-logo.png

| 左对齐 | 右对齐 | 居中对齐 |
| :----- | -----: | :------: |
| 单元格 | 单元格 |  单元格  |
| 单元格 | 单元格 |  单元格  |

- **音频**

<audio controls>
  <source src="http://www.chongfanmitu.com/chat/Sounds/test.mp3" >
</audio>

- **视频**

<video src="https://media.w3.org/2010/05/sintel/trailer.mp4" style="margin:0;"></video>
