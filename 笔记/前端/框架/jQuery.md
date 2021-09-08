## jQuery

**官网：<https://jquery.com/>**

```html
<script src="https://code.jquery.com/jquery-3.6.0.js"></script>
```

### jQuery 选择器

| 选择器                    | 作用                                              |
| ------------------------- | ------------------------------------------------- |
| \$("\*")                  | 选取**所有元素**                                  |
| \$(this)                  | 选取**当前 HTML 元素**                            |
| \$("p.intro")             | 选取**class**为 intro 的 p 元素                   |
| \$("p:first")             | 选取**第一个**p 元素                              |
| \$("ul li:first")         | 选取**第一个 ul**元素的**第一个 li**元素          |
| \$("ul li:first-child")   | 选取**每个 ul**元素的**第一个 li**元素            |
| \$("[href]")              | 选取**带有 href 属性**的元素                      |
| \$("a[target='_blank']")  | 选取**所有 target 属性值**等于'\_blank'的 a 元素  |
| \$("a[target!='_blank']") | 选取所有 target 属性值不等于"\_blank"的 a 元素    |
| \$(":button")             | 选取所有 type="button"的 input 元素和 button 元素 |
| \$("tr:even")             | 选取偶数位置的 tr 元素                            |
| \$("tr:odd")              | 选取奇数位置的 tr 元素                            |

### jQuery 事件

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

| 键盘事件     | 作用           |
| ------------ | -------------- |
| **onkeydown**  | 键被按下的过程 |
| **onkeypress** | 键被按下       |
| **onkeyup**    | 键被松开       |

| 表单事     | 作用           |
| ---------- | -------------- |
| **onsubmit** | 提交表单时     |
| **onchange** | 元素的值改变时 |
| **onfocus**  | 元素获得焦点时 |
| **onblur**   | 元素失去焦点时 |

| 文档/窗口事件 | 作用                 |
| ------------- | -------------------- |
| **ready**     | 页面加载完成时       |
| **load**      | 指定的元素已加载时   |
| **resize**    | 调整浏览器窗口大小时 |
| **scroll**    | 用户滚动指定的元素时 |
| **unload**    | 用户离开页面时       |

### jQuery 效果方法

| 方法名            | 作用                                         |
| ----------------- | -------------------------------------------- |
| **animate()**     | 对被选元素应用"自定义"的动画                 |
| **clearQueue**    | 对被选元素移除所有排队函数（仍未运行的）     |
| **delay()**       | 对被选元素的所有排队函数（仍未运行）设置延迟 |
| **dequeue()**     | 移除下一个排队函数，然后执行函数             |
| **fadeIn()**      | 逐渐改变被选元素的不透明度，从隐藏到可见     |
| **fadeOut()**     | 逐渐改变被选元素的不透明度，从可见到隐藏     |
| **fadeTo()**      | 把被选元素逐渐改变至给定的不透明度           |
| **fadeToggle()**  | 在 fadeIn() 和 fadeOut() 方法之间进行切换    |
| **finish()**      | 对被选元素停止、移除并完成所有排队动画       |
| **hide()**        | 隐藏被选元素                                 |
| **queue()**       | 显示被选元素的排队函数                       |
| **show()**        | 显示被选元素                                 |
| **slideDown()**   | 通过调整高度来滑动显示被选元素               |
| **slideToggle()** | slideUp() 和 slideDown() 方法之间的切换      |
| **slideUp()**     | 通过调整高度来滑动隐藏被选元素               |
| **stop()**        | 停止被选元素上当前正在运行的动画             |
| **toggle()**      | hide() 和 show() 方法之间的切换              |

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

- **Ajax 方法封装**

```js
/**
   * type 		请求类型 post/get
   * url 		请求地址
   * data		请求数据
   * dataType	请求数据类型 json/jsonp
   * async		是否异步请求
   * timeout	请求时间 (ms)
   * success 	请求成功回调函数
   * error 		请求失败回调函数
   * complete	请求完成回调函数 (成功和失败都执行)
   */
function api(type, data, url) {
    return new Promise((resolve, reject) => {
        $.ajax({
			type: type,
			data: data,
			url: url,
			dataType: "json",
   			async: false,
   			timeout: 5000,
			success: (res) => {
				resolve(res); 	//返回接口数据
			},
			error: (error) => {
				reject(error);  //返回错误信息
			},
        	complete: () => {}
		});
    });
};

//api接口封装
function 函数名(params){
	return api('get', params, '请求地址');
}
function 函数名(params){
	return api('post', params, '请求地址');
}

//调用api接口
函数名(params)
.then(res => {}) //请求成功
.catch(result => {}) //请求s
```

