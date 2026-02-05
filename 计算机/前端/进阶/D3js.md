## D3.js

- **`SVG` 数据可视化图表框架**

### CDN

```html
<script src="https://d3js.org/d3.v7.min.js"></script>
```

### 柱状图

```js
var data = [];
for (let i = 0; i < 10; i++) {
	data.push(Math.floor(Math.random() * 101));
}
var width = 500; //画布宽度
var height = 200; //画布高度
//创建画布
var svg = d3.select("body")
	.append("svg")
	.attr("width", width)
	.attr("height", height);
//绘制图形
svg.selectAll("rect")
	.data(data) //数据
	.enter()
	.append("rect")
	.attr("x", function(d, i) { //图形x坐标
		return i * width / data.length;
	})
	.attr("y", function(d) { //图形y坐标
		return height - d;
	})
	.attr("width", 40) //图形宽度
	.attr("height", 100) //图形高度
	.attr("fill", "#ff0000"); //图形颜色
//文字
svg.selectAll("text")
	.data(data)
	.enter()
	.append("text")
	.text(function(d) {
		return d;
	})
	.attr("x", function(d, i) { //文字x坐标
		return i * width / data.length + 20;
	})
	.attr("y", function(d) { //文字y坐标
		return height - d - 5;
	})
	.attr("font-family", "sans-serif") //文字样式
	.attr("font-size", "11px") //文字大小
	.attr("fill", "#ff0000") //文字颜色
```

### 折线图



### 饼图



