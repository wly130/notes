## TypeScript

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
```

- **变量类型**

| 关键字  | 类型     | 示例                      |
| ------- | -------- | ------------------------- |
| any     | 任意     | var all: any = ‘all’;     |
| number  | 数字     | var num: number = 10;     |
| string  | 字符串   | var str: string = ‘str’;  |
| boolean | 布尔     | var flag: boolean = true; |
| enum    | 枚举     |                           |
| void    | 无返回值 |                           |
| 类型[]  | 数组     | var number[];             |

#### 函数声明

```typescript
function test([参数]: [类型]): [返回值类型] {
    return [返回值];
}
```

