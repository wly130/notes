## jQuery

**官网：<https://jquery.com/>**

```html
<script src="https://code.jquery.com/jquery-3.6.1.js" integrity="sha256-3zlB5s2uwoUzrXK3BT7AX3FyvojsraNFxCc2vC/7pNI=" crossorigin="anonymous"></script>
```

### 初始化

```js
$(document).ready(() => {
	console.log("方式一");
});

$(()=> {
	console.log("方式二");
});

jQuery(($) => {
	console.log("方式三");
});
```

### 选择器

| 选择器                        | 作用                                              |
| ----------------------------- | ------------------------------------------------- |
| **\$("\*")**                  | 选取**所有元素**                                  |
| **\$(this)**                  | 选取**当前 HTML 元素**                            |
| **\$("p.intro")**             | 选取**class**为 intro 的 p 元素                   |
| **\$("p:first")**             | 选取**第一个**p 元素                              |
| **\$("ul li:first")**         | 选取**第一个 ul**元素的**第一个 li**元素          |
| **\$("ul li:first-child")**   | 选取**每个 ul**元素的**第一个 li**元素            |
| **\$("[href]")**              | 选取**带有 href 属性**的元素                      |
| **\$("a[target='_blank']")**  | 选取**所有 target 属性值**等于'\_blank'的 a 元素  |
| **\$("a[target!='_blank']")** | 选取所有 target 属性值不等于"\_blank"的 a 元素    |
| **\$(":button")**             | 选取所有 type="button"的 input 元素和 button 元素 |
| **\$("tr:even")**             | 选取偶数位置的 tr 元素                            |
| **\$("tr:odd")**              | 选取奇数位置的 tr 元素                            |

### 事件

#### 鼠标事件

| 鼠标事件       | 作用                                    |
| -------------- | --------------------------------------- |
| **onclick**      | 点击事件                                |
| **ondblclick**   | 双击事件                                |
| **onmouseenter** | 鼠标指针穿过元素时                      |
| **onmouseleave** | 鼠标指针移动到元素上方,并按下鼠标按键时 |
| **onmouseup**    | 在元素上松开鼠标按钮时                  |
| **onmouseout**   | 鼠标指针离开被选元素时                  |
| **onmousemove**  | 鼠标指针在指定的元素中移动时            |
| **onmouseover**  | 鼠标指针位于元素上方时                  |
| **onhover**      | 光标悬停事件                            |

#### 键盘事件

| 键盘事件     | 作用           |
| ------------ | -------------- |
| **onkeydown**  | 键被按下的过程 |
| **onkeypress** | 键被按下       |
| **onkeyup**    | 键被松开       |

#### 表单事件

| 表单事件     | 作用           |
| ---------- | -------------- |
| **onsubmit** | 提交表单时     |
| **onchange** | 元素的值改变时 |
| **onfocus**  | 元素获得焦点时 |
| **onblur**   | 元素失去焦点时 |

#### 文档/窗口事件

| 文档/窗口事件 | 作用                 |
| ------------- | -------------------- |
| **ready**     | 页面加载完成时       |
| **load**      | 指定的元素已加载时   |
| **resize**    | 调整浏览器窗口大小时 |
| **scroll**    | 用户滚动指定的元素时 |
| **unload**    | 用户离开页面时       |

### 效果方法

| 方法名            | 作用                                          |
| ----------------- | --------------------------------------------- |
| **animate()**     | 对被选元素应用"自定义"的动画                  |
| **clearQueue**    | 对被选元素移除所有排队函数（仍未运行的）      |
| **delay()**       | 对被选元素的所有排队函数（仍未运行）设置延迟  |
| **dequeue()**     | 移除下一个排队函数，然后执行函数              |
| **fadeIn()**      | 逐渐改变被选元素的不透明度，从隐藏到可见      |
| **fadeOut()**     | 逐渐改变被选元素的不透明度，从可见到隐藏      |
| **fadeTo()**      | 把被选元素逐渐改变至给定的不透明度            |
| **fadeToggle()**  | 在 `fadeIn()` 和 `fadeOut()` 方法之间进行切换 |
| **finish()**      | 对被选元素停止、移除并完成所有排队动画        |
| **hide()**        | 隐藏被选元素                                  |
| **queue()**       | 显示被选元素的排队函数                        |
| **show()**        | 显示被选元素                                  |
| **slideDown()**   | 通过调整高度来滑动显示被选元素                |
| **slideToggle()** | `slideUp()` 和 `slideDown()` 方法之间的切换   |
| **slideUp()**     | 通过调整高度来滑动隐藏被选元素                |
| **stop()**        | 停止被选元素上当前正在运行的动画              |
| **toggle()**      | `hide()` 和 `show()` 方法之间的切换           |
| **html()**        | 设置或返回所选元素的文本内容                  |
| **text()**        | 设置或返回所选元素的内容                      |
| **val()**         | 设置或返回表单字段的值                        |
| **attr()**        | 获取属性值                                    |

### 元素方法

| 方法名            | 作用                                     |
| ----------------- | ---------------------------------------- |
| **append()**      | 在被选元素的结尾插入内容                 |
| **prepend()**     | 在被选元素的开头插入内容                 |
| **after()**       | 在被选元素之后插入内容                   |
| **before()**      | 在被选元素之前插入内容                   |
| **remove()**      | 删除被选元素及其子元素                   |
| **empty()**       | 从被选元素中删除子元素                   |
| **addClass()**    | 向被选元素添加一个或多个类               |
| **removeClass()** | 从被选元素删除一个或多个类               |
| **toggleClass()** | 对被选元素进行添加/删除类的切换操作      |
| **css()**         | 设置或返回样式属性                       |
| **width()**       | 设置或返回元素的宽度，不包括内边距和边框 |
| **height()**      | 设置或返回元素的高度，不包括内边距和边框 |
| **innerWidth()**  | 返回元素的宽度，包括内边距               |
| **innerHeight()** | 返回元素的高度，包括内边距               |
| **outerWidth()**  | 返回元素的宽度，包括内边距和边框         |
| **outerHeight()** | 返回元素的高度，包括内边距和边框         |

### 循环渲染

#### `html()` 渲染

```js
let div = '';
for (let i = 0; i < 10; i++) {
	div += `<div>${i}</div>`;
}
$('.div').html(div);
```

#### `append()` 渲染

```js
let data = [{
	name: "一",
	value: 1
}, {
	name: "二",
	value: 2
}, {
	name: "三",
	value: 3
}]
for (let i = 0; i < data.length; i++) {
	let str = `<li>${data[i].name}</li>`;
	str += `<li>${data[i].value}</li>`;
	$(".div").append(str);
}
```

### Ajax 方法

| 方法名              | 作用                                               |
| ------------------- | -------------------------------------------------- |
| **\$.ajax()**       | 执行异步 AJAX 请求                                 |
| **\$.get()**        | 使用 AJAX 的 GET 请求从服务器加载数据              |
| **\$.getScript()**  | 使用 AJAX 的 GET 请求从服务器加载并执行 JavaScript |
| **\$.post()**       | 使用 AJAX 的 POST 请求从服务器加载数据             |
| **.ajaxComplete()** | 完成时运行的函数                                   |
| **.ajaxError()**    | 失败时运行的函数                                   |
| **.ajaxSend()**     | 发送之前运行的函数                                 |
| **.ajaxStart()**    | 第一个 AJAX 请求开始时运行的函数                   |
| **.ajaxStop()**     | 所有的 AJAX 请求完成时运行的函数                   |
| **.ajaxSuccess()**  | AJAX 请求成功完成时运行的函数                      |
| **.load()**         | 从服务器加载数据，并把返回的数据放置到指定的元素中 |

- **`Ajax` 方法封装**

```js
/**
 * @param {string} type 请求类型 post/get
 * @param {Object} params 请求数据
 * @param {string} url 请求地址
 */
function Api(type, url, params) {
    return new Promise((resolve, reject) => {
        $.ajax({
			type: type,
			data: params,
			url: url,
			dataType: "json",
   			async: false,
   			timeout: 300,
			success: (res) => resolve(res),
			error: (erro) => reject(error)
		});
    });
};

//api接口封装
function 函数名(params){
	return Api('get', '请求地址', params);
}
function 函数名(params){
	return Api('post', '请求地址', params);
}

//调用api接口
函数名(params)
.then(res => {}) //请求成功
.catch(err => {}) //请求失败
```

