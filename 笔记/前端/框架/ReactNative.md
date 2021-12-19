## React Native

### [React Native](https://reactnative.dev/) 是Facebook开发的跨平台移动应用开发框架

### 初始化

```shell
npm install -g create-react-native-app    #安装脚手架
create-react-native-app 项目名    #创建项目
npm start   #运行项目
```

### 目录结构

<img src="../../img/ReactNative%E7%9B%AE%E5%BD%95.png" style="zoom:100%;float:left;" />

| 名称         | 描述                                                         |
| :----------- | :----------------------------------------------------------- |
| **android**  | Android项目目录，包含了使用AndroidStudio开发项目的环境配置文件 |
| **ios**      | iOS项目目录，包含了XCode的环境                               |
| **index.js** | 入口文件                                                     |
| **app.json** | app的配置文件                                                |

### 初始化

**App.js**

```react
import React from 'react';
import { StyleSheet, Text, View } from 'react-native';

export default function App() {
    return (
        <View>
        	<Text></Text>
        </View>
    );
}

//CSS样式表
const styles = StyleSheet.create({

});
```

### 组件

#### 基础组件

```react
<View></View>  //视图组件
<Text>你好</Text>  //文字组件
```

#### 图片组件

```react
<Image source={ require('./img/img.jpg') } />
<Image source={{ uri:'https://img.jpg' }} />
```

#### 表单组件

```react
<TextInput
    value={text} //输入值
    multiline={true} //是否多行输入
    password={true} //是否为密码
    editable={false} //是否可编辑
    enablesReturnKeyAutomatically={true} //没有文字时禁用确认按钮
    onChangeText={text => setText(text)} //文本变化回调函数
    onFocus={() => {alert('获得焦点')}}
    onBlur={() => {alert('失去焦点')}}
    defaultValue={text} //默认文本
/>
```

### API封装

- **/api/request.js**

```js
let baseUrl = 'http://192.168.1.1:8080';  //请求地址

export function get(url, params) {
    let str = '';
    if (params) {
        str = '?';
        for (let i in params) {
            if (params[i] != -1) {
                str += i + "=" + params[i] + "&";
            }
        }
        str = str.substring(0, str.length - 1);
    }
    return new Promise((resolve, reject) => {
        fetch(baseUrl + url + str, {
            method: 'GET'
        }).then((res) => res.json())
            .then((data) => {
                resolve(data);
            }).catch((err) => {
                reject(err);
            }).done();
    });
}

export function post(url, params) {
    return new Promise((resolve, reject) => {
        fetch(baseUrl + url, {
            method: 'POST',
            headers: {
                "Content-Type": "application/json;charset=UTF-8"
            },
            body: JSON.stringify(params)
        }).then((res) => res.json())
            .then((data) => {
                resolve(data);
            }).catch((err) => {
                reject(err);
            }).done();
    });
}
```

- **/api/api.js**

```js
import { get, post } from './request.js'

const api = {
    函数名(params) {
        return post('/url', params);
    },
    函数名(params) {
        return get('/url', params);
    }
}

export default api;
```

