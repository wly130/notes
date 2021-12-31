## Flutter

**[Flutter](https://flutter.dev/) 是Google开源的构建用户界面工具包，支持移动端、Web端、桌面端和嵌入式平台**

### 初始化

- **[下载FlutterSDK](https://docs.flutter.dev/get-started/install/windows)**
- **配置环境变量**
- **创建项目**

```shell
flutter create 项目名
```

- **运行项目**

```shell
flutter run
```

### 目录结构

<img src="../../img/Flutter%E7%9B%AE%E5%BD%95%E7%BB%93%E6%9E%84.png" style="zoom:120%;margin:0;" />

| 文件或目录          | 说明                                                    |
| ------------------- | ------------------------------------------------------- |
| **.dart_tool**      | 一些dart工具库所在的位置和信息目录                      |
| **android**         | Android项目目录                                         |
| **ios**             | iOS项目目录                                             |
| **lib**             | 开发代码目录                                            |
| **test**            | 测试代码目录                                            |
| .**gitignore**      | git忽略配置文件                                         |
| .**metadata**       | IDE 用来记录某个 Flutter 项目属性的的隐藏文件           |
| .**packages**       | pub 工具需要使用的，包含 package 依赖的 yaml 格式的文件 |
| **flutter_app.iml** | 工程文件的本地路径配置                                  |
| **pubspec.lock**    | 当前项目依赖所生成的文件                                |
| **pubspec.yaml**    | 当前项目的一些配置文件                                  |

### 组件

#### 基本组件

- **根组件**

```dart
MaterialApp(
	title, 	//单行描述
	home, 	//默认显示界面
	color, 	//界面主色
	theme, 	//应用程序小部件使用的颜色
)
```

- **视图组件**

```dart
Container(
	alignment,//对齐方式
	padding,  //内边距
	color,    //背景色
    decoration, //样式
	width,    //宽度
	height,   //高度
	margin,   //外边距
	child,    //内容
)
```

- **顶部导航**

```dart
AppBar(
    leading, 	//左侧区域
    automaticallyImplyLeading, //左侧区域配置
    title,		//标题
    bottom,		//底部区域
    elevation,	//阴影高度，默认为4
    shape,		//描边形状
    backgroundColor, //背景色 
    brightness,	//主题色调，dark和light
    textTheme,	//文本主题样式
    primary,	//是否显示状态栏
    centerTitle,//否居中显示
    titleSpacing,//title区域内边距
    toolbarOpacity,//toolbar区域透明度
    bottomOpacity,//bottom区域透明度
)
```

- **文本组件**

```dart
Text(
	"文本",
	style, 			//样式
	strutStyle，    //段落的间距样式
	textAlign, 		//文字对齐方式
	textDirection, 	//文字排列方向 ltr 左到右，rtl右到左
	locale, 		//语言
	softWrap, 		//文字是否应该在软断行出断行
	textScaleFactor,//文字的缩放比例
	maxLines, 		//文本最大行数
	semanticsLabel, //图像的语义描述
)
```

- **图片组件**

```dart
Image(
	image, 	  //图片地址
    width,    //宽度
	height,   //高度
)
```

- **底部导航**

```dart
bottomNavigationBar: BottomNavigationBar(
	items: <BottomNavigationBarItem>[
	  	BottomNavigationBarItem(
	    	icon, //图标
	    	title,//标题
	    	backgroundColor, //背景色
	  	)
	]
	currentIndex: 0, //当前页
   	selectedItemColor,  //被选择item的颜色
   	unselectedItemColor,//未选择item的颜色
   	onTap: (){},   //item点击事件
   	type,          //风格
   	showSelectedLabels: true, //是否显示文字
),
```

- **布局组件**

```dart

```

- **列表组件**

```dart
ListView(
	ListTile(
		leading: Icon(Icons.alarm), //图标
        title: Text('文本')
    )
)
```

- **网格布局**
- 

### 列表渲染

```dart
class MyApp extends StatelessWidget {
	List datas = [
		{'id': '1', 'name': '张三'},
	  	{'id': '2', 'name': 'ls'},
	  	{'id': '3', 'name': 'ww'}
	];
    
	List<Widget> getData() {
	  	var list = datas.map((item) {
	    	return ListTile(
	      		title: Text(item!['name']),
	      		subtitle: Text(item['id']),
	    	);
	  	});
	  	return list.toList();
	}

	@override
	Widget build(BuildContext context) {
	  	return MaterialApp(
	    	home: Scaffold(
	      		body: ListView(
	        		children: this.getData(),
	      		),
	    	),
	  	);
	}
}

class MyApp extends StatelessWidget {
	List datas = [
		{'id': '1', 'name': '张三'},
	  	{'id': '2', 'name': 'ls'},
	  	{'id': '3', 'name': 'ww'}
	];
    
    List<Widget> getData(context, index) {
	  	return ListTile(
            title: Text(datas[index]['name']),
            subtitle: Text(datas[index]['id'])
        );
	}

	@override
	Widget build(BuildContext context) {
	  	return MaterialApp(
	    	home: Scaffold(
	      		body: ListView.builder(
            		itemCount: datas.length,
            		itemBuilder: this.getData(context, index)
            	})
	    	),
	  	);
	}
}

```

