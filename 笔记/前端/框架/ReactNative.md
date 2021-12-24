## React Native

**[React Native](https://reactnative.dev/) 是Facebook开发的跨平台移动应用开发框架**

### 初始化

```shell
npm install -g create-react-native-app    #安装脚手架
create-react-native-app 项目名    #创建项目
npm start   #运行项目

# Expo是一个用于开发 ReactNative 的平台(https://expo.dev/)
# 用 expo 创建 React Native 项目
npm install expo-cli -g   #安装脚手架
expo init 项目名    #创建项目
expo start   #运行项目
```

### 目录结构

<img src="../../img/ReactNative%E7%9B%AE%E5%BD%95.png" style="zoom:100%;float:left;" />

| 名称         | 描述                                |
| :----------- | :---------------------------------- |
| **android**  | Android项目目录，包含了环境配置文件 |
| **ios**      | iOS项目目录，包含了XCode的环境      |
| **App.js**   | 页面人口文件                        |
| **index.js** | 入口文件                            |
| **app.json** | app的配置文件                       |

### 初始化

**App.js**

```react
import React from 'react';
import { 
    StyleSheet, //样式组件
    Text, //文本组件
    View, //视图组件
    ScrollView, //滚动组件
    Image, //图片组件
    ImageBackground, //背景图片组件
    TextInput, //输入框
    Button, //按钮
    Switch, //开关
    ActivityIndicator, //加载
    Modal, //弹出框
    RefreshControl, //下拉刷新
    SectionList, //列表
    TouchableHightlight, //高亮触摸
    TouchableOpacity //透明触摸
} from 'react-native';

const App = () => {
    return (
        <View>
        	<Text></Text>
        </View>
    );
}

//CSS样式
const styles = StyleSheet.create({

});

export default App;
```

### 原生组件

#### 基础组件

```react
<View></View>  //视图组件
<Text>你好</Text>  //文字组件
<ScrollView></ScrollView>  //滚动组件
<TouchableHightlight //高亮触摸，用户点击时，组件会产生高亮效果
	onPress={() => {alert('点击事件')}}
	activeOpacity={0.7} //透明度
	underlayColor="#ff0000" //点击时的背景颜色
>
</TouchableHightlight>
<TouchableOpacity //透明触摸，用户点击时，组件会出现透明效果
   	onPress={() => {alert('点击事件')}}
   	activeOpacity={0.7} //透明度
    underlayColor="#ff0000" //点击时的背景颜色
>
</TouchableOpacity>
<TouchableWithoutFeedback //无反馈性触摸，用户点击时，组件不会出现任何视觉变化
   	onPress={() => {alert('点击事件')}}
>
</TouchableWithoutFeedback>
```

#### 图片组件

```react
<Image source={ require('./img/img.jpg') } /> //本地图片
<Image source={{ uri:'https://img.jpg' }} /> //网络图片
<ImageBackground source={{ uri:'https://img.jpg' }} resizeMode="cover"> //背景图片
</ImageBackground>
```

**resizeMode属性值**

| 属性值      | 作用                                                       |
| ----------- | ---------------------------------------------------------- |
| cover(默认) | 在显示比例不变的情况下填充整个显示区域, 可能部分会显示不了 |
| contain     | 比例不变, 显示整张图片                                     |
| stretch     | 不保持图片原来的宽,高比.填充整个显示区域                   |
| center      | contain模式基础上支持等比放大                              |

#### 表单组件

```react
<TextInput //输入框
    value={text} //输入值
    multiline={true} //是否多行输入
    numberOfLines={4} //多行输入行数
    password={true} //是否为密码
    editable={false} //是否可编辑
    maxLength={11} //输入长度
    enablesReturnKeyAutomatically={true} //没有文字时禁用确认按钮
    onChangeText={text => setText(text)} //文本变化回调函数
    onFocus={() => {alert('获得焦点')}}
    onBlur={() => {alert('失去焦点')}}
    defaultValue={text} //默认文本
/>
<Button //按钮
	title="文本"
    onPress={() => {alert('点击事件')}}
    color="#000000" //文本颜色
    disabled={false}
/>
<Switch //开关
    disabled={false}
	trackColor={{ false: "#767577", true: "#81b0ff" }} //开关轨道的自定义颜色
	thumbColor={isEnabled ? "#f5dd4b" : "#f4f3f4"} //前景开关手柄的颜色
	onValueChange={value} //更改开关的值时调用
	value={isEnabled} //开关的值
/>
```

#### 提示组件

```react
<ActivityIndicator //加载组件
    animating={true} //是否显示
    color="00ff00" //颜色
    size="small" //大小
/>
<Modal //弹出框
	animationType="slide" //动画
	transparent={true} //是否将填补整个视图
	visible={false} //是否可见
	onRequestClose={() => {alert('点击返回按钮回调函数')}}
></Modal>
<RefreshControl //下拉刷新
	refreshing={refreshing}
	onRefresh={onRefresh}
/>

let list = [{
    title: "title1",
    data: ["data1", "data2", "data3"]
}, {
    title: "title2",
    data: ["data1", "data2", "data3"]
}, {
    title: "title2",
    data: ["data1", "data2", "data3"]
}];
const Item = ({ title }) => (
	<View>
		<Text>{title}</Text>
	</View>
);
<SectionList //分段列表
	sections={list} //列表数组
    renderItem={({ item }) => <Item title={item} />} //默认渲染器
/>
<StatusBar //顶部状态栏
	animated={true} //动画
	backgroundColor="#61dafb" //状态栏背景颜色
	barStyle={statusBarStyle} //状态栏文本颜色
	showHideTransition="fade" //过渡动画
	hidden={false} //是否隐藏 
/>
```

**`animationType`属性和`showHideTransition`属性**

- **`slide` 从底部滑入**
- **`fade` 淡入淡出**
- **`none` 没有动画**

#### 样式组件

```react
//初始化
const styles = StyleSheet.create({});
//合并样式,属性相同时，style2 覆盖 style1
const styles = StyleSheet.compose(style1, style2);
```

### API封装

- **/api/request.js**

```js
let baseUrl = 'http://192.168.1.1:8080';  //请求地址

export function get(url, params) {
    let str = '';
    if (Object.keys(params).length > 0) {
        str = '?';
        for (let i in params) {
        	str += i + "=" + params[i] + "&";
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

### 第三方UI组件库

- **Teaset**    [文档链接](https://github.com/rilyu/teaset)
- **NativeBase**    [文档链接](https://nativebase.io/)
- **React Native Elements**    [文档链接](https://reactnativeelements.com/)
- **React-Native-Maps**    [文档链接](https://github.com/react-native-maps/react-native-maps)
- **React Native icon**    [文档链接](https://github.com/oblador/react-native-vector-icons)
- **UI Kitten**    [文档链接](https://akveo.github.io/react-native-ui-kitten/)
- **react-native-material-kit**    [文档链接](https://github.com/xinthink/react-native-material-kit)
