## Echarts

### Echarts是一个数据可视化库

### 使用

- **官网: <https://echarts.apache.org/zh/index.html>**

- **安装依赖包**

```shell
npm install echarts@4.9.0 --save
```

- **初始化**

```js
import * as echarts from "echarts";

//配置信息
let option = {};
//获取容器
let chartDom = document.getElementById('id');
//初始化
let myChart = echarts.init(chartDom);
myChart.setOption(option);
```

