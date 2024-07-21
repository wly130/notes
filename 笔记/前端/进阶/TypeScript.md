### TypeScript

#### 配置环境

- **安装依赖包**

```shell
npm install -g typescript
```

- **编译代码**

```shell
tsc app.ts
```

#### 变量声明

```typescript
var [变量名]: [类型] = 值;

//联合类型: 设置多种类型,只能赋值指定的类型，赋值其它类型会报错
var [变量名]: [类型1]|[类型2] = 值;
```

- **变量类型**

| 关键字  | 类型         | 示例                        |
| ------- | ------------ | --------------------------- |
| any     | 任意         | `var all: any = ‘all’;`     |
| number  | 数字         | `var num: number = 10;`     |
| string  | 字符串       | `var str: string = ‘str’;`  |
| boolean | 布尔         | `var flag: boolean = true;` |
| enum    | 枚举         |                             |
| void    | 无返回值     |                             |
| 类型[]  | 数组         | `var number[] = [1, 2, 3];` |
| never   | 永不存在的值 |                             |

#### 函数声明

- **有参数**

```typescript
//有返回值
function test([参数]: [类型]): [返回值类型] {
    return [返回值];
}
//无返回值
function test([参数]: [类型]): void {}
```

- **可选参数**

```typescript
function test([参数]?: [类型]) {}
```

- **参数数量不确定**

```typescript
function test(...[参数]: 类型[]) {}
```

- **函数重载**

> **方法名字相同，而参数(类型，数量，名称，顺序)不同，返回类型可以相同也可以不同**

#### 接口

- **定义接口**

```typescript
interface 接口名 {
    [变量名]:[类型],
    [index:索引类型]:[值类型]  //数组
}
```

- **调用接口**

```typescript
var value:接口名 = { 
	[变量名]:[值]
} 
```

- **接口继承**

```typescript
interface 接口1 { 
	[变量名]:[类型]
}

interface 接口2 { 
	[变量名]:[类型]
}

//单继承
interface 接口3 extends 接口1 { 
	[变量名]:[类型]
}
 
//多继承
interface 接口4 extends 接口1, 接口2 { 
	[变量名]:[类型]
}
```

#### 类

- **创建类**

```typescript
class 类名 { 
    // 字段 
    [变量名]:[类型]; 
 
    // 构造函数 
    constructor([参数]: [类型]) {}
}
```

- **创建实例化对象**

```typescript
var obj = new 类名([参数]);
```

- **类的继承, 不支持继承多个类, 支持多重继承**

```typescript
class 类1 {}
class 类2 extends 类1 {}
class 类3 extends 类2 {}
```

- **`super` 关键字**

```typescript
class 类1 {
    方法名(): [类型] {}
}

class 类2 extends 类1 {
    super.方法名(); //调用父类的方法
}
```

- **`static` 关键字**

```typescript
class 类名 {  
	static [变量名]:[类型]; 
} 

//静态成员可以直接通过类名调用
类名.[变量名] = value;
```

- **`instanceof` 运算符**

```typescript
class 类名 {}
var obj = new 类名();
obj instanceof 类名; //判断对象是否是指定类型
```

- **访问控制修饰符**

> **public（默认）**	 公有，可以在任何地方被访问
>
> **protected** 		受保护，可以被其自身以及其子类和父类访问
>
> **private** 		     私有，只能被其定义所在的类访问

- **类实现接口**

```typescript
interface 接口名 { 
	[变量名]:[类型]
}

class 类名 implements 接口名 {
	[变量名]:[类型] = value;
}
```

#### 泛型

- **参数或返回值类型不确定时使用**

```typescript
function 函数名<T>([参数]: T): T {
    return [参数];
}

let obj1 = 函数名<string>("str");
let obj2 = 函数名<number>(123);
let obj3 = 函数名<boolean>(true);
```
