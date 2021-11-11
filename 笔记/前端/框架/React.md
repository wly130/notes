## 目录

[TOC]

## React 框架

### React CDN引用

- **react.min.js** **核心库**
- **react-dom.min.js** **DOM**相关的功能
- **babel.min.js** 将 **ES6** 代码转为 **ES5** 代码

> **开发环境**

```html
<script crossorigin src="https://unpkg.com/react@16/umd/react.development.js"></script>
<script crossorigin src="https://unpkg.com/react-dom@16/umd/react-dom.development.js"></script>
<script src="https://unpkg.com/babel-standalone@6/babel.min.js"></script>
<script type="text/babel"></script>
```

> **生产环境**

```html
<script crossorigin src="https://unpkg.com/react@16/umd/react.production.min.js"></script>
<script crossorigin src="https://unpkg.com/react-dom@16/umd/react-dom.production.min.js"></script>
<script src="https://unpkg.com/babel-standalone@6/babel.min.js"></script>
<script type="text/babel"></script>
```

### React项目搭建

- **安装脚手架**

```shell
npm install -g create-react-app
```

- **创建React项目**

```shell
create-react-app 项目名
```

- **启动项目**

```shell
npm start
```

- **项目打包**

```shell
npm run build
```

### 目录结构

<img src="../../img/React%E7%9B%AE%E5%BD%95%E7%BB%93%E6%9E%84.png" style="float:left;" />

| index.html        | 网页入口文件           |
| ----------------- | ---------------------- |
| **manifest.json** | **扩展配置文件**       |
| **robots.txt**    | **对特定网页进行屏蔽** |
| **index.js**      | **项目入口文件**       |

### JSX语法

- **注释**

    需要加**大括号**

```react
let name = <div>Text{/*注释*/}</div>;
```

- **多个标签**

    需要有一个**父元素**

```react
let num = (
  	<div>
  		<div>Text1</div>
    	<div>Text2</div>
  	</div>
);
```

- **插入变量**

```react
let text = "你好";
let num = 10;
let txt = (
  	<div>
  		<div>{text}</div>
  		<div>{num}</div>
  	</div>
);
```

- **引入图片**

```react
import imgUrl from '../assets/photo.png';
<img src={imgUrl} />
//或
<img src={require('../assets/photo.png')} />
```

- **插入样式**

> **CSS属性名采用==驼峰式==命名**
>
> **单位为 `px` 时可以忽略**

```react
import React from 'react';
import './css'; //外部引入CSS

class App extends React.Component {
    render() {
        let styles = { //CSS对象
            color: '#000000',
            backgroundColor: '#FFFFFF'
        }
        return (
            <div>
                <div style={styles}></div>
                <div style={{ color: '#FF0000', fontSize: 20 }}></div>
            </div>
        )
    }
}

export default App;
```

[返回顶部](#目录)

### state 状态

- **初始化state**

```react
constructor(props) {
	super(props);
	this.state={}
}
//或
state = {}
```

- **更新state**

```react
this.setState({ name: 'value' }, () => {
    //更新完成后执行的代码
});
```

- **更新对象属性**

```react
constructor(props) {
	super(props);
	this.state = {
        name: {
            value: '123'
        }
    }
}

let name = this.state,name;
name.value = '456';
this.setState({ name });
```

### 事件处理

#### 常用事件

| 事件名          | 作用                                   |
| --------------- | -------------------------------------- |
| **onClick**     | 鼠标点击事件                           |
| **onDblClick**  | 鼠标双击事件                           |
| **onMouseDown** | 鼠标上的按钮被按下了                   |
| **onMouseUp**   | 鼠标按下后，松开时                     |
| **onMouseOver** | 当鼠标移动到某对象范围的上方时         |
| **onMouseMove** | 鼠标移动时                             |
| **onMouseOut**  | 当鼠标离开某对象范围时                 |
| **onKeyPress**  | 当键盘上的某个键被按下并且释放时       |
| **onKeyDown**   | 当键盘上某个按键被按下时               |
| **onBlur**      | 当前元素失去焦点时                     |
| **onChange**    | 当前元素失去焦点并且元素的内容发生改变 |
| **onFocus**     | 当某个元素获得焦点时                   |
| **onReset**     | 重置表单时                             |
| **onSubmit**    | 提交表单时                             |

### 函数传参

- **使用 `biind` 传参**

```react
import React from 'react'
class App extends React.Component {
    constructor(props) {
        super(props)
        this.state = {
            index: 1
        }
    }
    
    render() {
        return (
            <div>
                <button onClick={this.getValue.bind(this, this.state.index)}>确定</button>
            </div>
        )
    }

    getValue(val) {
        console.log(val);
    }
}

export default App;
```

- **使用 `箭头函数` 传参**

```react
import React from 'react'
class App extends React.Component {
    constructor(props) {
        super(props)
        this.state = {
            index: 1
        }
    }
    render() {
        return (
            <div>
                <button onClick={() => this.getValue(this.state.index)}>确定</button>
            </div>
        )
    }

    getValue = (val) => {
        console.log(val);
    }
}

export default App;
```

### 条件渲染

#### `函数` 判断

```react
import React from 'react';
class App extends React.Component {
	constructor(props) {
        super(props);
        this.state = {
            flag: true
        }
    }

    IFFlag = () => {
        if (this.state.flag) {
            return <div>True</div>
        } else {
            return <div>False</div>
        }
    }

    render() {
        return (
            <div>
                {this.IFFlag()}
            </div>
        )
    }
}

export default App;
```

#### `元素变量` 判断

```react
import React from 'react';
class App extends React.Component {
	constructor(props) {
        super(props);
        this.state = {
            flag: true
        }
    }

    render() {
        let { flag } = this.state;
        let IFFlag;
        
        if (flag) {
            IFFlag = <div>True</div>
        } else {
            IFFlag = <div>False</div>
        }
        return (
            <div>
                {IFFlag }
            </div>
        )
    }
}

export default App;
```

#### `三元运算符 `判断

```react
import React from 'react';
class App extends React.Component {
	constructor(props) {
        super(props);
        this.state = {
            flag: true
        }
    }

    render() {
        let { flag } = this.state;
        return (
            <div>
                { flag ? <div>True</div> : <div>False</div> }
            </div>
        )
    }
}

export default App;
```

#### `逻辑运算符&&` 判断

```react
import React from 'react';
class App extends React.Component {
	constructor(props) {
        super(props);
        this.state = {
            flag: true
        }
    }

    render() {
        let { flag } = this.state;
        return (
            <div>
                { flag && <div>True</div> }
                { !flag && <div>False</div> }
            </div>
        )
    }
}

export default App;
```

#### `Switch` 判断

```react
import React from 'react';
class App extends React.Component {
	constructor(props) {
        super(props);
        this.state = {
            flag: true
        }
    }

    render() {
        let { flag } = this.state;
        switch (flag) {
        	case true:
      			return <div>True</div>;
    		case false:
      			return <div>False</div>;
    		default:
      			return null;
  		}
        return (
            <div>
                { flag }
            </div>
        )
    }
}

export default App;
```

[返回顶部](#目录)

### 列表渲染

#### `html`渲染

```react
import React from 'react';
class App extends React.Component {
    constructor(props) {
        super(props);
        this.state = {
            info: [{
                title: '第一个标题',
                key: '第一个key'
            }, {
                title: '第二个标题',
                key: '第二个key'
            }, {
                title: '第三个标题',
                key: '第三个key'
            }]
        }
    }

    render() {
        return (
			<ul>
			    {
			        this.state.info.map((item, key) => {
			            return (
			                <li key={key}>
			                    <p>{item.title}</p>
			                    <p>{item.key}</p>
			                </li>
			            )
			        })
			    }
			</ul>
        )
    }
}

export default App;
```

#### `参数`方式渲染

```react
import React from 'react'
class App extends React.Component {
    constructor(props) {
        super(props);
        this.state = {
            info: [{
                title: '第一个标题',
                key: '第一个key'
            }, {
                title: '第二个标题',
                key: '第二个key'
            }, {
                title: '第三个标题',
                key: '第三个key'
            }]
        }
    }

    render() {
        let ForList = this.state.info.map((item, key) => {
            return (
                <li key={key}>
                    <p>{item.title}</p>
                    <p>{item.key}</p>
                </li>
            )
        })
        return (
            <ul>
            	{ForList}
            </ul>
        )
    }
}

export default App;
```

#### `函数`方式渲染

```react
import React from 'react'
class App extends React.Component {
    constructor(props) {
        super(props);
        this.state = {
            info: [{
                title: '第一个标题',
                key: '第一个key'
            }, {
                title: '第二个标题',
                key: '第二个key'
            }, {
                title: '第三个标题',
                key: '第三个key'
            }]
        }
    }

    ForInfo(list) {
        let res = list.map((item, key) => {
            return (
                <li key={key}>
                    <p>{item.title}</p>
                    <p>{item.key}</p>
                </li>
            )
        })
        return res;
    }

    render() {
        return (
            <ul>{this.ForInfo(this.state.info)}</ul>
        )
    }
}

export default App;
```

#### `组件`方式渲染

```react
import React from 'react'
class ForList extends React.Component {
    constructor(props) {
        super(props);
    }

    render() {
        return (
            <li>
                <p>{this.props.info.title}</p>
                <p>{this.props.info.key}</p>
            </li>
        );
    }
}

class App extends React.Component {
    constructor(props) {
        super(props);
        this.state = {
            info: [{
                title: '第一个标题',
                key: '第一个key'
            }, {
                title: '第二个标题',
                key: '第二个key'
            }, {
                title: '第三个标题',
                key: '第三个key'
            }]
        }
    }

    render() {
        return (
            <ul>
            	{
            	    this.state.info.map((item, index) => {
            	        return <ForList key={index} info={item} />
            	    })
            	}
            </ul>
        )
    }
}

export default App;
```

####  `Array` 方式渲染

```react
import React from 'react';
class App extends React.Component {
    constructor(props) {
        super(props);
        this.state = {}
    }
    
    render() {
        return (
			<ul>
			    {
                    //从 0 开始,循环 n 次
			        [...Array(n)].map((item, key) => {
			            return <li key={key}>{key}</li>
			        })
			    }
			</ul>
        )
    }
}

export default App;
```

### 表单处理

- **`name` 属性值与 `state` 值相同**

```react
import React from 'react';
class App extends React.Component {
    state = {
        txt: '',
        checkeds: ''
    }

    //获取单个输入值
    getVal = (e) => {
        this.setState({
            txt: e.target.value
        })
    }

    //获取多个输入值(表单数量过多时)
    getAllVal = (e) => {
        const target = e.target;
        const value = target.type === 'checkbox' ? target.checked : target.value;
        const name = target.name;
        this.setState({
            [name]: value
        })
    }

    render() {
        return (
            <div>
                <input type="text" name="txt" value={this.state.txt} onChange={this.getVal} />
                <input type="checkbox" name="checkeds" checked={this.state.checkeds} onChange={this.getAllVal} />
            </div>
        )
    }
}

export default App;
```

[返回顶部](#目录)

### React 组件

- **组件名称必须以大写字母开头**

#### 函数组件

```react
let App = () => {
    return (
        <div>函数组件</div>
    )
}
```

#### 类组件

```react
import React from 'react';
class App extends React.Component {
   	constructor(props) {
        super(props)
        this.state = {}
    }

    render() {
        return (
            <div>类组件</div>
        )
    }
}

export default App;
```

#### 函数组件和类组件的区别

| 区别                   | 函数组件 | 类组件 |
| ---------------------- | -------- | ------ |
| **是否有 `this`**      | **没有** | **有** |
| **是否有生命周期**     | **没有** | **有** |
| **是否有状态 `state`** | **没有** | **有** |

#### 组件的生命周期方法

```mermaid
graph TD;
	getDefaultProps-->
	componentWillMount-->
	render-->
	componentDidMount-->
	props改变时-->
	componentWillReceiveProps-->
	shouldComponentUpdate
	componentDidMount-->
	state改变时-->
	shouldComponentUpdate-->
	componentWillUpdata-->
	render重新渲染-->
	componentDidUpdate-->
	componentWillUnmount;
	
	设置默认的props-->
	组件将要挂载-->
	创建虚拟dom-->
	组件挂载完成后-->
	更新props时-->
	更新props-->
	对比前后两个props和state是否相同
	组件挂载完成后-->
	更新state-->
	对比前后两个props和state是否相同-->
	组件将要更新时-->
	组件重新渲染-->
	组件更新完成后-->
	组件将要卸载时;
```

- **`componentWillMount` 和 `componentDidMount` 只执行一次**

```react
import React from 'react'
class App extends React.Component {
    constructor(props) {
        super(props)
        this.state = {
            msg: '一个msg数据'
        }
    }

    //组件将要挂载时
    componentWillMount() {
        console.log('组件将要挂载')
    }

    //组件挂载完成时
    componentDidMount() {
        console.log('组件挂载完成')
    }
	
    //更新数据函数
    setMsg = () => {
        this.setState({
            msg: '改变后的msg数据'
        })
    }

    //数据渲染
    render() {
        return (
            <div>
                {this.state.msg}
                <button onClick={this.setMsg}>更新数据</button>
            </div>
        )
    }

    //是否要更新数据 
    //nextProps: 父组件传给子组件的值, 没有显示空 
    //nextState: state 更新后的值
    shouldComponentUpdate(nextProps, nextState) {
        console.log('是否要更新数据')
        return true;
    }

    //将要更新数据时
    componentWillUpdate() {
        console.log('组件将要更新')
    }

    //数据更新完成时
    componentDidUpdate() {
        console.log('组件更新完成')
    }

    //改变props值时
    componentWillReceiveProps() {
        console.log('改变props值时')
    }

    //组件将要销毁时
    componentWillUnmount() {
        console.log('组件销毁')
    }
}

export default App;
```

[返回顶部](#目录)

#### 组件通信

- **子组件 调用 父组件中的方法**

```react
//父组件
fun() {
	console.log('父组件方法')；
}
<子组件 fun={this.fun} />

//子组件
this.props.fun();
```

- **子传父**

```react
//子组件
import React from 'react';
class Test extends React.Component {
    //点击向父组件传值
    setValue = (val) => {
        return () => {
            this.props.setVal(val)
        }
    }

    render() {
        return (
            <div>
                <button onClick={this.setValue('子组件信息')}>点击</button>
            </div>
        )
    }
}
export default Test;

//父组件
import React from 'react';
import Test from './test';
class App extends React.Component {
    //接收子组件的值
    setVal(val) {
        console.log(val);
    }

    render() {
        return (
            <div>
                <Test setVal={this.setVal.bind(this)} />
            </div>
        )
    }
}
export default App;
```

- **父组件 调用 子组件的方法**

```react
<子组件 ref="fun" />

this.$refs.fun.方法名();
```

- **父传子**

```react
//父组件
import React from 'react';
import Test from './test';
class App extends React.Component {
    render() {
        return (
            <div>
                {/* 向子组件传值 */}
                <Test name="你好" />
            </div>
        )
    }
}
export default App;

//子组件
import React from 'react';
class Test extends React.Component {
    render() {
        //接收父组件的值
        return (
            <div>{this.props.name}</div>
        )
    }
}
export default Test;
```

- **跨组件通信**

```react
import React from 'react';
//父组件 给 孙组件 传值
const Context = React.createContext();
//孙组件
class Leaf extends React.Component {
    render() {
        return (
            <Context.Consumer>
                {(name) => <h1>{name}</h1>}
            </Context.Consumer>
        )
    }
}

// 子组件
class Middle extends React.Component {
    render() {
        return <Leaf />
    }
}

//父组件
class App extends React.Component {
    state = {
        name: 'name'
    }

    render() {
        const { name } = this.state;
        return (
            <div>
                <Context.Provider value={name}>
                    <Middle />
                </Context.Provider>
            </div>
        )
    }
}
export default App;
```

[返回顶部](#目录)

### 封装API

> **和 Vue 封装一样**

- **index.js**

```react
import api from './api/api';

React.$api = api;  //全局引入

React.$api.函数名(params).then(res => {});  //调用API
```

### 跨域配置

- **安装依赖包**

```shell
npm install http-proxy-middleware
```

- **src/setupProxy.js**

```js
const proxy = require('http-proxy-middleware');
module.exports = (app) => {
    app.use(
        proxy('/api', {
        	target: 'http://xxx.xxx.xxx.xxx', //接口地址
            changeOrigin: true,
            pathRewrite: { 
                '^/api': '' 
            }
        })
    )
}
```

[返回顶部](#目录)

### React-router

#### 安装依赖包

```shell
npm install react-router-dom
```

#### Router配置

- **路由表**

```react
const routes = [{
    path: '/',
    component: require("../components/test"),
    exact: true
}, {
    path: '/Test2',
    component: require("../components/test2"),
    exact: true
}];

export default routes;
```

- **路由配置**

```react
import React from "react";
import { BrowserRouter as Router, Route, Link } from "react-router-dom"; //BrowserRouter
// import { HashRouter as Router, Route, Link } from "react-router-dom"; //HashRouter
import routes from "./routers"; //引入路由表

function RouterList() {
    return (
        <Router>
            {/*点击跳转*/}
            <Link to="/test2">test2</Link>
            {/*跳转接收*/}
            <Route path={routes[1].path} component={routes[1].component} />
        </Router>
    );
}

export default RouterList;
```

#### Router`跳转` 和 `传参`

- **路径传参  (参数显示在路径上, 刷新页面后参数不消失)**

```react
<Link to="/test2/${name}">test2</Link>
<Route path="/test2/:name" component={Test2} />

//js方式
this.props.history.push({
    pathname: '/test2/${name}'
})

//组件接收参数
{this.props.match.params.name}
```

- **`query`传参  (刷新页面后参数消失)**

```react
<Link to={{ pathname: '/test2', query: { name: 'name' } }}>test2</Link>
<Route path="/test2" component={Test2} />

//js方式
this.props.history.push({
    pathname: '/test2',
    query: {
        name: 'name'
    }
})

//组件接收参数
{this.props.location.query.name} 
```

- **`state`传参  (刷新页面后参数不消失)**

```react
<Link to={{ pathname: '/test2', state: { name: 'name' } }}>test2</Link>
<Route path="/test2" component={Test2} />

//js方式
this.props.history.push({
    pathname: '/test2',
    state: {
        name: 'name'
    }
})

//组件接收参数
{this.props.location.state.name}
```

[返回顶部](#目录)

### React Hooks

**在函数组件中可以使用 `生命周期函数` 和 `State状态`**

#### `useState` 

- **定义state状态**

```react
import React, { useState } from 'react'
function Example() {
    const [num, setNum] = useState(0);
    const [str, setStr] = useState('String');
    return (
        <div>
            <p>{num}</p>
            <p>{str}</p>
            {/* 修改变量值 */}
            <button onClick={() => { setNum(num + 1) }}>{num}+1</button>
            <button onClick={() => { setStr('str') }}>{str}</button>
        </div>
    );
}
```

#### `useEffect` 

- **类似于类组件的生命周期函数 (异步加载)**

```react
import React, { useState, useEffect } from 'react'
function Example() {
    const [num, setNum] = useState(0);
	
    useEffect(() => {
        console.log('组件渲染完成');
        return () => {
            console.log('组件卸载完成');
        }
    /**
     * 不传参数时，组件每次渲染时都执行
     * 为 [] 时，只调用一次
     * 有参数时，只在参数值变化时执行
     */
    }, [num]);

    return (
        <div>
            <p>{num}</p>
            <button onClick={() => { setNum(num + 1) }}>{num}+1</button>
        </div>
    );
}		
```

#### `useContext()`

- **共享状态**

```react
import React, { useContext } from 'react'
function Example1() {
    const AppContext = React.createContext({});
    const Example2 = () => {
        //获取参数
        const { name } = useContext(AppContext);
        return (
            <div>
                <p>Example2{name}</p>
            </div>
        );
    }

    const Example3 = () => {
        const { name } = useContext(AppContext);
        return (
            <div>
                <p>Example3{name}</p>
            </div>
        );
    }

    return (
        //父组件传参
        <AppContext.Provider value={{ name: 'value' }}>
            <Example2 />
            <Example3 />
        </AppContext.Provider>
    );
}
```

#### `userReducer()`

```react
import React, { useReducer } from 'react'
function Example() {
    /**
     * state 状态值
     * action dispatch传的参数
     */
    const [count, dispatch] = useReducer((state, action) => {
        switch (action) {
            case '+':
                return state + 1;
            case '-':
                return state - 1;
        }
    }, 0);

    return (
        <div>
            <span>{count}</span>
            <button onClick={() => { dispatch('+') }}>{count}+1</button>
            <button onClick={() => { dispatch('-') }}>{count}-1</button>
        </div>
    )
}
```

[返回顶部](#目录)

### Redux

- **安装依赖包**

```shell
npm install react-redux --save
```

- **index.js**

```react
import React from 'react';
import ReactDOM from 'react-dom';
import { Provider } from "react-redux";
import store from "./redux/store"
import './index.css';
import App from './App';

ReactDOM.render(
    <Provider store={store}>
        <App />
    </Provider>,
    document.getElementById('root')
);
```

- **redux/store.js**

```react
import { createStore } from "redux"

const State = {
    count: 0
}

const store = createStore((state = State, action) => {
    if (action.type == '+') {
        return {
            count: state.count + 1
        }
    }
});

export default store;
```

- **页面**

```react
import React, { Component } from 'react'
import { connect } from "react-redux"

class Index extends Component {
    constructor(props) {
        super(props);
        this.state = {}
    }

    add = () => {
        this.props.add();
    }

    render() {
        return (
            <div>
                <div>{this.props.count}</div>
                <button onClick={this.add}>+</button>
            </div>
        );
    }
}

//获取 state 状态
function getState(state) {
    return {
        count: state.count
    }
}

//获取 dispatch 的值
function getDispatch(dispatch) {
    return {
        add() {
            dispatch({
                type: "+"
            })
        }
    }
}

export default connect(getState, getDispatch)(Index);
```

[返回顶部](#目录)

