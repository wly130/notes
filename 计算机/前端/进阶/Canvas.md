### Canvas 画布

#### 创建画布

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<meta name="viewport" content="width=device-width, initial-scale=1">
		<title>canvas 画布</title>
	</head>
	<body>
		<canvas id="canvas" width="500" height="500">不支持低版本浏览器</canvas>
	</body>
</html>
```

#### 初始化画布

```js
let canvas = document.getElementById('canvas');
let ctx = canvas.getContext("2d");
```

#### 绘制基本图形

##### 矩形

| fillRect(x, y, width, height)       | 绘制一个填充的矩形                   |
| ----------------------------------- | ------------------------------------ |
| **strokeRect(x, y, width, height)** | 绘制一个矩形的边框                   |
| **clearRect(x, y, width, height)**  | 清除指定矩形区域，让清除部分完全透明 |

```js
/**
  * 绘制矩形
  * x		矩形起点的X坐标左上角为原点
  * y		矩形终点的Y坐标
  * width	矩形的宽度
  * height	矩形的高度
  * isClear	是否绘制清除画布
  * isFill	是否是填充
  * bgColor	矩形或边框的颜色
  **/
function drawRect(x, y, width, height, isClear, isFill, bgColor) {
	if (isClear) { //绘制清除画布的矩形区域
		ctx.clearRect(x, y, width, height);
	} else {
		if (isFill) { //绘制填充矩形
			ctx.fillStyle = bgColor;
			ctx.fillRect(x, y, width, height);
		} else { //绘制边框矩形
			ctx.strokeStyle = bgColor;
			ctx.strokeRect(x, y, width, height);
		}
	}
}
```

##### 圆形

| arc(x, y, radius, startAng, endAng, anticlockwise) | 绘制圆形 |
| -------------------------------------------------- | -------- |

```js
/**
  * 绘制圆弧
  * x				圆心X坐标
  * y				圆心Y坐标
  * radius			半径
  * startAngle		开始的角度
  * endAngle		结束的角度
  * anticlockwise	true为逆时针，false为顺时针
  * isFill			是否是填充
  * bgColor			圆弧的颜色
  **/
function drawArc(x, y, radius, startAngle, endAngle, anticlockwise, isFill, bgColor) {
    var startAng = Math.PI * (startAngle / 180);
    var endAng = Math.PI * (endAngle / 180);
	if (isFill) { //绘制填充圆弧
		cxt.fillStyle = bgColor;
		cxt.beginPath();
		cxt.arc(x, y, radius, startAng, endAng, anticlockwise);
		cxt.closePath();
		cxt.fill();
	} else { //绘制边框圆弧
		cxt.strokeStyle = bgColor;
		cxt.beginPath();
		cxt.arc(x, y, radius, startAng, endAng, anticlockwise);
		cxt.stroke();
	}
}
```

##### 线段

```js
/**
  * 绘制线段
  * startX		起点 X 坐标
  * startY		起点 Y 坐标
  * endX		终点 X 坐标
  * endY		终点 Y 坐标
  * lineWidth	线段宽度
  * bgColor		线的颜色
  **/
function drawLine(startX, startY, endX, endY, lineWidth, bgColor) {
	cxt.beginPath();
	cxt.lineWidth = lineWidth;
	cxt.fillStyle = bgColor;
	cxt.moveTo(startX, startY);
	cxt.lineTo(endX, endY);
	cxt.closePath();
	cxt.stroke();
	cxt.fill();
}
```

##### 三角形

```js
/**
  * 绘制三角形
  * startMark	起点坐标 [x, y]
  * mesoMark	第二点坐标 [x, y]
  * endMark		第三点坐标 [x, y]
  * lineColor	边框颜色
  * bgColor		填充颜色
  **/
function drawTriangle(startMark, mesoMark, endMark, lineColor, bgColor) {
	cxt.strokeStyle = lineColor; //线条颜色
	cxt.fillStyle = bgColor; //填充颜色
	cxt.beginPath();
	cxt.moveTo(startMark[0], startMark[1]); //起点
	cxt.lineTo(mesoMark[0], mesoMark[1]); //第二点
	cxt.lineTo(endMark[0], endMark[1]); //第三点
	cxt.closePath();
	cxt.stroke();
	cxt.fill();
}
```

##### 扇形

```js
/**
  * 绘制扇形
  * x				圆心X坐标
  * y				圆心Y坐标
  * radius			半径
  * startAngle		开始的角度
  * endAngle		结束的角度
  * anticlockwise	true为逆时针，false为顺时针
  * isFill			是否是填充
  * bgColor			圆弧的颜色
  **/
function drawSector(x, y, radius, startAngle, endAngle, anticlockwise, isFill, bgColor) {
    var startAng = Math.PI * (startAngle / 180);
    var endAng = Math.PI * (endAngle / 180);
	if (isFill) {
		cxt.fillStyle = bgColor;
		cxt.beginPath();
		cxt.moveTo(x, y);
		cxt.arc(x, y, radius, startAng, endAng, anticlockwise);
		cxt.closePath();
		cxt.fill();
	} else {
		cxt.strokeStyle = bgColor;
		cxt.beginPath();
		cxt.moveTo(x, y);
		cxt.arc(x, y, radius, startAng, endAng, anticlockwise);
		cxt.closePath();
		cxt.stroke();
	}
}
```

