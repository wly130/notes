## Python

### 注释

```python
# 单行注释

'''
多行注释
'''
"""
多行注释
"""
```

### 数据类型

```python
type(value) # 判断数据类型
```

#### 字符串

```python
str = "String"
str2 = "WLY"
str[n] 		# 获取下标为 n 的字符
str[n:m] 	# 获取下标为 [n, m) 范围的字符
str + str2 	# 拼接字符串
str * 2 	# 复制字符串
str = '''
	多行文本
'''
```

#### 数字

```python
num = 100 	# 整数
flo = 3.14 	# 浮点数
n ** m 		# n 的 m次方
```

#### 列表

- **有序,可重复,可修改**

```python
list = [1, 2, 3, 4]
list[n] 		# 下标为 n 的元素
list[-n] 		# 倒数第 n 个元素
list[n:m]		# 获取下标为 [n, m] 范围的元素
list[n] = 1 	# 修改下标为 n 的元素
del list[n] 	# 删除下标为 n 的元素
list.append()	# 添加元素到尾部
list.remove()	# 删除元素
list.clear()	# 清空列表
list.copy()		# 复制列表
```

#### 元组

- **有序,可重复,不可修改**

```python
tup = (1, 2, 3, 4)
tup[n] 		# 下标为 n 的元素
tup[-n] 	# 倒数第 n 个元素
tup[n:m]	# 获取下标为 [n, m] 范围的元素
```

#### 字典

- **无序,不可重复,可修改**

```python
obj = {
    "name": "str",
    "age": 22
}
obj["name"]				# 获取属性值
obj["sex"] = "value" 	# 添加属性
del obj["name"] 		# 删除属性 name
obj.clear()     		# 清空字典
```

#### 集合

- **无序,不可重复,可修改**

```python
setList = {1, 2, 3, 4, 5}
setList.add(value) 		# 添加元素
setList.remove(value) 	# 删除元素
```

### 判断语句

```python
if True:
    # 执行语句
elif False:
    # 执行语句
else:
    # 执行语句
```

### 循环语句

```python
# while 循环
while True:
    # 执行语句
else:
    # 执行语句
    
# For 循环
for i in range(n):
    # 循环[0, n) 范围的数
else:
    # 执行语句
```

### 异常处理

```python
try:
	# 正常语句
    raise 	# 抛出异常
except Error:
    # 异常处理
else:
    # 没有异常
finally:
    # 必须执行
```

### 时间获取

```python
import time  # 引入time模块

time.time() 	# 当前时间戳
time.localtime()# 当前时间
time.ctime()  	# 当前时间的易读字符串
time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()) # 当前时间格式化
```
- **时间元组**

| 序号 | 字段         |
| :--- | :----------- |
| 0    | 年           |
| 1    | 月           |
| 2    | 日           |
| 3    | 小时         |
| 4    | 分钟         |
| 5    | 秒           |
| 6    | 星期         |
| 7    | 一年的第几日 |
| 8    | 夏令时       |

- **获取日历**

```python
import calendar
# 2022年1月 日历
calendar.month(2022, 1)
```

### 函数

```python
def 函数名(prams):
    return prams
```

### 爬虫

#### 请求网站

```python
import urllib.request

baseUrl = "https://movie.douban.com/top250"
head = {
	"User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/101.0.4951.67 Safari/537.36"
}
# 请求网页
req = urllib.request.Request(baseUrl, headers=head)
# 获取数据
res = urllib.request.urlopen(req)
# 获取网页代码
html = res.read().decode('utf-8')
```

#### BeautifulSoup(网站解析器)

```python
from bs4 import BeautifulSoup

soup = BeautifulSoup(html, "html.parser") # 解析网页
```

- **.find_all()方法**

| 参数                | 作用                   | 例子                                 |
| ------------------- | ---------------------- | ------------------------------------ |
| **name**            | **标签搜索**           | **`soup.find_all(name="div")`**      |
| **class_/id/title** | **属性搜索**           | **`soup.find_all(class_="item")`**   |
| **string**          | **标签内容搜索**       | **`soup.find_all(string="str")`**    |
| **recursive**       | **是否搜索所有子节点** | **`soup.find_all(recursive=False)`** |

- **.select()方法(CSS选择器搜索)**

```python
soup.select(".class")  	# class属性搜索
soup.select("div")  	# 标签搜索
```

#### re(正则表达式)

```python
import re

# 从第一个开始搜索
re.match(key, str)
# 搜索第一个匹配的值
re.search()
# 搜索所有匹配的值
re.findall()
# 替换匹配的值
re.sub()
```

#### 获取网页文字

```python
items = soup.select("div")
list = []
for i in range(len(items)):
    list.append((items[i].string)
```

