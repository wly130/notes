## TypeScript

### 配置环境

- **安装依赖包**

```shell
npm install -g typescript
```

- **编译代码**

```shell
tsc app.ts           #编译单个文件
tsc -w app.ts        #监听模式,文件变化自动编译
tsc --init           #生成tsconfig.json
```

### 变量声明

```typescript
let name: string = "value";
const num: number = 10;
let flag: boolean = true;

//联合类型: 变量可以是多种类型之一
let value: string | number = "str";
value = 123; //也合法
```

- **类型推断** 声明时不指定类型,由赋值自动推断

```typescript
let str = "hello"; //自动推断为string
let arr = [1, 2, 3]; //自动推断为number[]
```

### 数据类型

| 类型        | 说明             | 示例                                |
| ----------- | ---------------- | ----------------------------------- |
| **any**     | 任意类型         | `let val: any = 'all';`             |
| **unknown** | 安全的any        | `let val: unknown = 123;`           |
| **number**  | 数字             | `let num: number = 10;`             |
| **string**  | 字符串           | `let str: string = 'str';`          |
| **boolean** | 布尔             | `let flag: boolean = true;`         |
| **array**   | 数组             | `let arr: number[] = [1, 2, 3];`    |
| **tuple**   | 元组(固定长度)   | `let t: [string, number] = ['a', 1];` |
| **enum**    | 枚举             | 见下方                              |
| **void**    | 无返回值         | `function fn(): void {}`            |
| **never**   | 永不存在的值     | 用于抛出异常或无限循环的函数返回值  |
| **null**    | 空值             | `let val: null = null;`             |
| **undefined** | 未定义         | `let val: undefined = undefined;`   |
| **object**  | 对象             | `let obj: object = {};`             |

#### 枚举(enum)

```typescript
//数字枚举(默认从0开始自增)
enum Direction { Up, Down, Left, Right }
Direction.Up; // 0

//自定义起始值
enum Code { Success = 200, NotFound = 404 }

//字符串枚举
enum Color { Red = "RED", Blue = "BLUE" }

//反向映射(仅数字枚举)
let name = Direction[0]; // "Up"
```

- 字符串枚举**不支持反向映射**
- 推荐用 `as const` 对象替代简单枚举

```typescript
const Direction = { Up: 0, Down: 1 } as const;
type Direction = typeof Direction[keyof typeof Direction]; // 0 | 1
```

#### 字面量类型

```typescript
let align: "left" | "center" | "right"; //只能是这三个值
let num: 1 | 2 | 3;                     //只能是1,2,3
```

### 类型别名与接口

#### 类型别名(type)

```typescript
type ID = string | number;
type User = { name: string; age: number };
type Callback = (data: string) => void;
```

#### 接口(interface)

```typescript
interface User {
    name: string;
    age: number;
    email?: string;          //可选属性
    readonly id: number;     //只读属性
    [key: string]: any;      //索引签名(允许额外属性)
}
```

#### interface 与 type 区别

| 特性           | interface          | type               |
| -------------- | ------------------ | ------------------ |
| **扩展方式**   | extends继承        | &交叉类型          |
| **声明合并**   | 支持(同名自动合并) | 不支持(同名报错)   |
| **联合/元组**  | 不支持             | 支持               |
| **适用场景**   | 对象形状约束       | 任意类型别名       |

#### 接口继承

```typescript
interface Animal {
    name: string;
}
interface Dog extends Animal {
    breed: string;
}
//多继承
interface Pet extends Animal, Trainable {
    owner: string;
}
```

### 函数声明

```typescript
//有返回值
function add(a: number, b: number): number {
    return a + b;
}
//无返回值
function log(msg: string): void {
    console.log(msg);
}
//箭头函数
const multiply = (a: number, b: number): number => a * b;
```

#### 可选参数与默认值

```typescript
function greet(name: string, greeting?: string): string {
    return `${greeting || "Hello"}, ${name}`;
}
function greet(name: string, greeting: string = "Hello"): string {
    return `${greeting}, ${name}`;
}
```

- 可选参数必须放在**必选参数之后**

#### 剩余参数

```typescript
function sum(...nums: number[]): number {
    return nums.reduce((a, b) => a + b, 0);
}
sum(1, 2, 3); // 6
```

#### 函数类型

```typescript
//类型别名方式
type Fn = (a: number, b: number) => number;
const add: Fn = (a, b) => a + b;

//接口方式
interface Fn {
    (a: number, b: number): number;
}
```

#### 函数重载

```typescript
function reverse(str: string): string;
function reverse(arr: number[]): number[];
function reverse(value: any): any {
    if (typeof value === "string") return value.split("").reverse().join("");
    return value.reverse();
}
```

### 类

```typescript
class User {
    private name: string;
    readonly id: number;
    static count: number = 0; //静态属性

    constructor(name: string, id: number) {
        this.name = name;
        this.id = id;
        User.count++;
    }

    getName(): string {
        return this.name;
    }
}

let user = new User("Tom", 1);
User.count; //通过类名访问静态属性
```

#### 访问修饰符

| 修饰符         | 访问范围                       |
| -------------- | ------------------------------ |
| **public**     | 默认,任何地方可访问            |
| **protected**  | 自身及子类可访问               |
| **private**    | 仅自身类内部可访问             |
| **readonly**   | 只读,只能在声明或构造函数中赋值 |

#### 类的继承

```typescript
class Animal {
    constructor(protected name: string) {}
    move(): string { return `${this.name} is moving`; }
}

class Dog extends Animal {
    constructor(name: string, private breed: string) {
        super(name); //调用父类构造函数
    }
    //重写父类方法
    move(): string { return `${this.name} is running`; }
    bark(): string { return "Woof!"; }
}
```

- **不支持多继承**,支持**多层继承**
- 子类构造函数中必须先调用 `super()`
- 构造函数参数使用修饰符可**自动声明为类属性**

#### 类实现接口

```typescript
interface Printable {
    print(): void;
}

class Doc implements Printable {
    print(): void { console.log("printing..."); }
}
```

#### 抽象类

```typescript
abstract class Shape {
    abstract area(): number; //抽象方法(子类必须实现)
    describe(): string { return `Area: ${this.area()}`; }
}

class Circle extends Shape {
    constructor(private r: number) { super(); }
    area(): number { return Math.PI * this.r ** 2; }
}
```

- 抽象类**不能直接实例化**,只能被继承

### 泛型

```typescript
//泛型函数
function identity<T>(arg: T): T {
    return arg;
}
identity<string>("hello");
identity(123); //类型推断,等同于identity<number>(123)

//泛型接口
interface Response<T> {
    code: number;
    data: T;
}
let res: Response<string[]> = { code: 200, data: ["a", "b"] };

//泛型类
class Container<T> {
    private value: T;
    constructor(val: T) { this.value = val; }
    getValue(): T { return this.value; }
}
```

#### 泛型约束

```typescript
interface HasLength { length: number; }

function logLength<T extends HasLength>(arg: T): T {
    console.log(arg.length);
    return arg;
}
logLength("hello");    //OK, string有length
logLength([1, 2, 3]);  //OK, 数组有length
//logLength(123);      //错误, number没有length
```

#### keyof 与泛型

```typescript
function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
    return obj[key];
}
let user = { name: "Tom", age: 18 };
getProperty(user, "name"); //OK
//getProperty(user, "email"); //错误, "email"不在keyof user中
```

### 类型守卫

```typescript
//typeof守卫
function fn(value: string | number) {
    if (typeof value === "string") {
        value.toUpperCase(); //此处value为string
    }
}

//instanceof守卫
if (err instanceof Error) {
    console.log(err.message);
}

//in守卫
interface Bird { fly(): void; }
interface Fish { swim(): void; }
function move(animal: Bird | Fish) {
    if ("fly" in animal) animal.fly();
    else animal.swim();
}

//自定义类型守卫
function isString(val: unknown): val is string {
    return typeof val === "string";
}
```

### 工具类型

| 工具类型             | 作用                           |
| -------------------- | ------------------------------ |
| `Partial<T>`         | 所有属性变为可选               |
| `Required<T>`        | 所有属性变为必选               |
| `Readonly<T>`        | 所有属性变为只读               |
| `Pick<T, K>`         | 从T中挑选指定属性K             |
| `Omit<T, K>`         | 从T中排除指定属性K             |
| `Record<K, T>`       | 构造键为K、值为T的对象类型     |
| `Exclude<T, U>`      | 从T中排除U(联合类型差集)       |
| `Extract<T, U>`      | 从T中提取U(联合类型交集)       |
| `NonNullable<T>`     | 排除T中的null和undefined       |
| `ReturnType<F>`      | 获取函数F的返回值类型          |
| `Parameters<F>`      | 获取函数F的参数类型元组        |
| `Awaited<T>`         | 获取Promise解析后的类型        |

```typescript
interface User { name: string; age: number; email: string; }

type UserPreview = Pick<User, "name" | "age">;  // {name, age}
type UserNoEmail = Omit<User, "email">;          // {name, age}
type UserOptional = Partial<User>;               // 所有属性可选
type UserReadonly = Readonly<User>;              // 所有属性只读
```

### 常用语法

#### 可选链与空值合并

```typescript
//可选链(?.) 安全访问可能为null/undefined的属性
let city = user?.address?.city;

//空值合并(??) 左侧为null/undefined时取右侧值
let name = input ?? "default";

//非空断言(!) 告诉编译器该值不为null/undefined
let el = document.querySelector("#app")!;
```

#### 类型断言

```typescript
//将变量断言为指定类型
let value = <string>someValue;        //尖括号语法
let value = someValue as string;      //as语法(推荐,JSX中必须用)

//双重断言(慎用)
let value = someValue as unknown as TargetType;
```

#### 交叉类型(&)

```typescript
type A = { name: string };
type B = { age: number };
type C = A & B; // { name: string; age: number }
```

### 模块

#### 导出

```typescript
//命名导出
export interface User { name: string; }
export function greet(user: User): string { return `Hi ${user.name}`; }
export const VERSION = "1.0";

//默认导出
export default class App { }
```

#### 导入

```typescript
//命名导入
import { User, greet, VERSION } from "./module";
//重命名
import { greet as sayHi } from "./module";
//默认导入
import App from "./module";
//全部导入
import * as mod from "./module";
//仅导入类型
import type { User } from "./module";
```

### tsconfig.json

```json
{
    "compilerOptions": {
        "target": "ES2020",
        "module": "ESNext",
        "lib": ["ES2020", "DOM"],
        "outDir": "./dist",
        "rootDir": "./src",
        "strict": true,
        "esModuleInterop": true,
        "skipLibCheck": true,
        "forceConsistentCasingInFileNames": true,
        "resolveJsonModule": true,
        "declaration": true,
        "sourceMap": true,
        "baseUrl": ".",
        "paths": { "@/*": ["src/*"] }
    },
    "include": ["src/**/*"],
    "exclude": ["node_modules", "dist"]
}
```

| 配置项                   | 说明                                 |
| ------------------------ | ------------------------------------ |
| **target**               | 编译目标ES版本                       |
| **module**               | 模块系统(ESNext/CommonJS)            |
| **strict**               | 开启所有严格类型检查                 |
| **outDir**               | 编译输出目录                         |
| **rootDir**              | 源码根目录                           |
| **esModuleInterop**      | 允许default导入CommonJS模块          |
| **skipLibCheck**         | 跳过.d.ts类型检查(加快编译)          |
| **declaration**          | 生成.d.ts类型声明文件                |
| **sourceMap**            | 生成sourceMap便于调试                |
| **paths**                | 路径别名映射                         |
| **noImplicitAny**        | 禁止隐式any(strict已包含)            |
| **strictNullChecks**     | 严格null/undefined检查(strict已包含) |
