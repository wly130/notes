## Angular1+

- **CDN: https://cdnjs.cloudflare.com/ajax/libs/angular.js/1.8.3/angular.min.js**

### 绑定

```html
<div ng-app="myApp" ng-controller="myCtrl">
    <input ng-model="name"> <!-- 双向数据绑定 -->
    {{name}}
    <button ng-click="Click()">点击</button>
    <button ng-disabled="flag">点击</button>
</div>

<script>
    var app = angular.module('myApp', []);
    app.controller('myCtrl', ($scope) => {
        $scope.name = ""; //定义变量
        $scope.Click = () => { //定义函数
            $scope.title = "Hello" + $scope.name;
        }
        $scope.flag = true; //是否禁用标签
    });
</script>
```

### 循环渲染

```html
<div ng-app="myApp" ng-controller="myCtrl">
    <ul>
        <li ng-repeat="item in list">
            {{$index}} <!-- 下标,从0开始 -->
            {{item}}
        </li>
    </ul>
</div>
<script>
    var app = angular.module('myApp', []);
    app.controller('myCtrl', ($scope) => {
        $scope.list = [1, 2, 3, 4];
    });
</script>
```

### 条件渲染

```html
<div ng-app="myApp" ng-controller="myCtrl">
    <ul>
        <!-- true时显示,false时移除元素 -->
        <li ng-if="flag">显示</li> 
        <li ng-if="!flag">隐藏</li>
    </ul>
    <ul>
        <!-- true时显示,false时隐藏元素 -->
        <li ng-show="flag">显示</li>
        <li ng-show="!flag">隐藏</li>
    </ul>
    <ul>
        <!-- false时显示,true时隐藏元素 -->
        <li ng-hide="flag">隐藏</li>
        <li ng-hide="!flag">显示</li>
    </ul>
</div>
<script>
    var app = angular.module('myApp', []);
    app.controller('myCtrl', ($scope) => {
        $scope.flag = true;
    });
</script>
```

### 过滤器

```html
<p>{{ lastName | lowercase }}</p>
```

- **自定义过滤器**

```js
app.filter('过滤器名', function() {
	return function(text) {
    	return true;
    }
});
```

| 过滤器    | 描述                   |
| --------- | ---------------------- |
| currency  | 格式化数字为货币格式   |
| filter    | 从数组项中选择一个子集 |
| lowercase | 格式化字符串为小写     |
| orderBy   | 根据某个表达式排列数组 |
| uppercase | 格式化字符串为大写     |

### $location

```js
$location.absUrl(); //网站地址
$location.url(); 
$location.protocol(); //协议名
$location.host(); //主机地址
$location.port(); //端口号
$location.path(); //当前URL的路径
$location.search(); 
$location.hash(); 
$location.replace(); 
```

### 网络请求

```js
var app = angular.module('myApp', []);
app.controller('myCtrl', ($scope) => {
    $http({
		method: 'GET', //请求类型
		url: 'url' //请求地址
	}).then((res) => {
        //请求成功
		console.log(res.data);	
	}, (err) => {
		//请求失败
	});
});
```

### 路由

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <script src="https://cdnjs.cloudflare.com/ajax/libs/angular.js/1.8.3/angular.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/angular-route/1.8.3/angular-route.min.js"></script>
</head>
<body>
    <div ng-app="myApp">
        <ul>
            <li><a href="#!/">首页</a></li>
            <li><a href="#!/page1">页面1</a></li>
            <li><a href="#!/page2">页面2</a></li>
        </ul>
        <div ng-view></div>
    </div>
</body>
<script>
    var router = angular.module('myApp', ['ngRoute']);
    router.config(['$routeProvider', ($routeProvider) => {
        $routeProvider
            .when('/', { template: '首页' })
            .when('/page1', { template: '页面1' })
            .when('/page2', { template: '页面2' })
            .otherwise({ redirectTo: '/' });
    }]);
</script>
</html>
```

## Angular2+

### 绑定属性

```html
<div [class]="testClass"></div> <!-- 绑定 class -->
<div class="{{testClass}}"></div> <!-- 绑定 class -->
<div [innerText]="title"></div> <!-- 绑定 富文本 -->
<div [ngStyle]="{'color': 'red'}"></div> <!-- 绑定 style 样式 -->
<div [style.fontSize]="'40px'"></div> <!-- 绑定 style 样式属性值 -->
```

### 循环渲染

- **index.component.html**

```html
<ul>
	<li *ngFor="let item of list;let i">{{ i }}.{{ item }}</li>
</ul>
```

- **index.component.ts**

```typescript
import { Component } from '@angular/core';
import { CommonModule } from '@angular/common';

@Component({
	selector: 'app-index',
	templateUrl: './index.component.html',
	styleUrls: ['./index.component.css'],
	standalone: true,
	imports: [CommonModule],
})
export class IndexComponent {
	constructor() { }
	public list: any[] = [1, 2, 3];
}
```

### 条件渲染

- **index.component.html**

```html
<div *ngIf="num==10">true</div>
```

- **index.component.ts**

```typescript
import { Component } from '@angular/core';
import { CommonModule } from '@angular/common';

@Component({
	selector: 'app-index',
	templateUrl: './index.component.html',
	styleUrls: ['./index.component.css'],
	standalone: true,
	imports: [CommonModule],
})
export class IndexComponent {
	constructor() { }
	public num: number = 10;
}
```

### 鼠标事件

- **index.component.html**

```html
<button (click)="getInput($event)">点击</button>
```

- **index.component.ts**

```typescript
import { Component } from '@angular/core';

@Component({
	selector: 'app-index',
	templateUrl: './index.component.html'
})

export class IndexComponent {
    public getInput(e: any) {
		console.log(e); //获取点击事件对象
	}
}
```

### 键盘事件

| 事件                | 作用                |
| ------------------- | ------------------- |
| **keydown.enter**   | **监听回车键**      |
| **keydown.space**   | **监听空格键**      |
| **keydown.control** | **监听 `Ctrl` 键**  |
| **keyup.shift**     | **监听 `Shift` 键** |
| **keyup.alt**       | **监听 `Alt` 键**   |

- **index.component.html**

```html
<input type="text" #input (keydown.enter)="getInput($event)">
```

- **index.component.ts**

```typescript
import { Component } from '@angular/core';

@Component({
	selector: 'app-index',
	templateUrl: './index.component.html'
})

export class IndexComponent {
    public value: string = '';
    
	public getInput(e: any) {
		this.value = e.target.value; //获取输入值
	}
}
```

### 表单事件

#### 获取 `input` 输入值

- **index.component.html**

```html
<input type="text" #input (input)="getInput(input.value)">
<div>{{value}}</div>
```

- **index.component.ts**

```typescript
import { Component } from '@angular/core';

@Component({
	selector: 'app-index',
	templateUrl: './index.component.html'
})

export class IndexComponent {
    public value: string = '';
    
	public getInput(e: string) {
		this.value = e;
	}
}
```

#### 获取 `select` 值

- **index.component.html**

```html
<select #select (change)="getSelect(select.value)">
	<option>1</option>
	<option>2</option>
	<option>3</option>
</select>
<span>{{value}}</span>
```

- **index.component.ts**

```typescript
import { Component } from '@angular/core';

@Component({
	selector: 'app-index',
	templateUrl: './index.component.html'
})

export class IndexComponent {
    public value: string = '';
    
	public getSelect(e: string) {
		this.value = e;
	}
}
```

### 路由

- **src/app/app-routing.module.ts**

```typescript
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
//引入
import { IndexComponent } from './index/index.component';

const routes: Routes = [{
	path: '',
	redirectTo: "index",
	pathMatch: "full"
}, {
	path: 'index',
	component: IndexComponent
}];

@NgModule({
	imports: [RouterModule.forRoot(routes)],
	exports: [RouterModule]
})
export class AppRoutingModule { }
```

### 网络请求

#### 请求封装

- **src/api/api.ts**

```typescript
import { Component, Injectable } from '@angular/core';
import { Requset } from './request';

@Injectable({
	providedIn: 'root'
})
export class Api {
	constructor(public http: Requset) { }
	public getIinfo(params: object) {
		return this.http.get('/api/getIinfo', params);
	}
    public postIinfo(params: object) {
		return this.http.post('/api/postIinfo', params);
	}
}
```

- **src/api/request.ts**

```typescript
import { Component, Injectable } from '@angular/core';
import { HttpClient, HttpHeaders } from '@angular/common/http';
import { Observable } from 'rxjs';

@Injectable({
	providedIn: 'root'
})
export class Requset {
	public httpOptions = {
		headers: new HttpHeaders({ 'Content-Type': 'application/json' })
	};

	constructor(public http: HttpClient) { }

	public get(url: string, params: object) {
		return new Observable(e => {
			this.http.get(url, params).subscribe(res => {
				e.next(res);
			}, err => {
				e.error(err);
			});
		});
	}

	public post(url: string, data: object) {
		return new Observable(e => {
			this.http.post(url, data, this.httpOptions).subscribe(res => {
				e.next(res);
			}, err => {
				e.error(err);
			});
		});
	}
}
```

- **页面使用**

```typescript
import { Component } from '@angular/core';
import { Api } from '../../api/api'; //引入

@Component({
	selector: 'app-index',
	templateUrl: './index.component.html',
	styleUrls: ['./index.component.css']
})

export class IndexComponent {
	constructor(private api: Api) { }
    
    private params: object = {}

	public getIinfo() {
		this.api.getIinfo(this.params).subscribe(res => {
			console.log(res);
		});
	}
}
```

#### 跨域配置

- **src/proxy.conf.json**

```typescript
{
	"/api": {
		"target": "http://localhost:3000",
		"secure": false,
		"pathRewrite": {
			"^/api": ""
		}
	}
}
```

- **angular.json**

​	**在 `serve` 下添加**

```json
"options": {
	"browserTarget": "your-application-name:build",
	"proxyConfig": "src/proxy.conf.json"
}
```

