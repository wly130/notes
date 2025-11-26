## Dart

### Dart 特点

- 面向对象的语言，一切数据类型、API 都是对象，都继承自 Object 类

- 强类型语言，同时也是动态类型语言，对不确定类型的可以定义成一个动态类型

- Dart 没有设置定义访问域的关键字，如果某个变量或者方法、类的名称以"_"开头，说明这个变量或者方法、类是私有的，外部不可以调用使用

- Dart 有入口函数

- Dart 吸收了很多现代编程语言的特点，加入了很多便捷的语法支持，可以明显缩减代码量和提高可读性

- 拥有 Future 和 Streams 使用方式，可以进行类似 RxJava 式的使用

### 初始化

```dart
//无返回值
void main() {
}
//有返回值
main() {
    return null;
}
```

### 基础

#### 数据类型

| 关键字        | 类型                      | 示例                      |
| ------------- | ------------------------- | ------------------------- |
| **`var`**     | **所有类型**              | **`var all = '';`**       |
| **`dynamic`** | **所有类型**              | **`dynamic all = '';`**   |
| **`Object`**  | **所有类型**              | **`Object all = '';`**    |
| **`final`**   | **常量**                  | **`final fin = 1;`**      |
| **`const`**   | **常量**                  | **`const con = 2;`**      |
| **`bool`**    | **布尔类型**              | **`bool flag = true;`**   |
| **`double`**  | **小数类型**              | **`double pi = 3.14;`**   |
| **`int`**     | **整数类型**              | **`int width = 200;`**    |
| **`num`**     | **数字类型 (小数和整数)** | **`num number = 12;`**    |
| **`String`**  | **字符串类型**            | **`String str = 'str';`** |

- **`final`和 `const` 的区别**

1. **`final` 的值在运行时确定**
2. **`const` 的值在编译时确定**

- **`dynamic`，`var`，`object` 的区别**

|                        | dynamic | var   | object |
| ---------------------- | ------- | ----- | ------ |
| **数据类型是否可变**   | true    | false | true   |
| **是否会静态类型检查** | false   | false | true   |

#### 常用方法

| 方法名               | 作用                          |
| -------------------- | ----------------------------- |
| **`codeUnitAt()`**   | 当前索引位置字符的UTF-16码    |
| **`startsWith()`**   | 当前字符串是否以指定字符开头  |
| **`endsWith()`**     | 当前字符串是否以指定字符结尾  |
| **`toUpperCase()`**  | 大写                          |
| **`toLowerCase()`**  | 小写                          |
| **`indexOf()`**      | 获取指定字符的索引位置        |
| **`contains()`**     | 字符串是否包含指定字符        |
| **`trim()`**         | 去除字符串的首尾空格          |
| **`length`**         | 字符串长度                    |
| **`replaceFirst()`** | 替换第一次出现t字符位置的字符 |
| **`replaceAll()`**   | 全部替换                      |
| **`str is String`**  | 判断类型是否为String          |
| **`str is! String`** | 判断类型是否不为String        |
| **`str as String`**  | 强制转换成String类型          |

### List() ,Set() 和 Map()

#### List()

```dart
var list = List(); //不限长度,类型和可添加任意类型的数组
var list = List(n); //限定了长度为 n
var list = [1, 2, 3]; //同类型赋值 限定类型和长度
var list = [2, '3', true]; //不同类型赋值 限定类型和长度,任意位置可以用任意类型替换
var list = <String>['a', '2']; //赋值指定泛型
List<String> list = new List(n); //声明长度为 n,类型为String的数组

/**
 * 属性
 */
list.length; //长度
list.reversed; //翻转
list.isEmpty; //是否为空
list.isNotEmpty; //是否不为空

/**
 * 方法
 */
list.add(); //在最后添加单个元素
list.addAll([]); //在最后添加多个元素
list.insert(index, str); //在下标 index 前插入单个元素
list.insertAll(index, []); //在下标 index 前插入多个元素
List<int> list1 = [1, 2, 3];
List<int> list2 = [4, 5, 6];
Iterable<int> list = list1.followedBy(list2); //合并 list1 和 list2
list.toList(growable:false); //生成的List不可再添加和修改
list.remove(); //删除指定元素
list.removeLast(); //删除最后一位
list.removeRange(min, max);//删除下标 [min, max) 范围的元素
list.removeWhere((item)=>item==5); //删除符合条件的元素
list.clear(); //清空数组
list.firstWhere((item) => (item > 2)); //返回符合条件的第一个元素, 否则返回-1
list.indexWhere((e) => (e > 13)); //返回第一个满足条件的元素的 index, 否则返回-1
list.lastWhere((e) => (e > 3)); //从后向前找，返回第一个满足条件的元素
list.forEach((e) {}); //遍历数组
```

#### Set()

```dart
var list = [1, 2, 3, 3, 4, 2];
new Set(list); //去重
```

#### Map()

```dart
Map mapList = {
    "name": "张三",
	"age": 20,
	"sex": "男"
}

/**
 * 属性
 */
mapList.keys; //获取所有的 key 值
mapList.values; //获取所有的 value 值
mapList.isEmpty; //是否为空
mapList.isNotEmpty; //是否不为空
        
/**
 * 方法
 */
mapList.remove(key); //删除指定 key 的数据
mapList.addAll({}); //合并映射, 给映射内增加属性
mapList.containsValue; //查看映射内的值, 返回true/false
```

### 函数

```dart
//无返回值
void Fun() {
}

//有返回值
Function Fun() {
    return true;
}

//必填参数
func(String str) {  
	return str;
}

//可选参数
func({String str}) {  
	return str;
}

// @required 标识的参数必填，其他选填
func({@required String str, int num}) {  
	return str + num;
}

// [] 包裹的参数选填，其他参数必填
func(String str, [int num]) {  
	return str + num;
}
```

### 类

```dart
class Person {
	String name;
	int age;
}
```

### 接口

```dart

```

