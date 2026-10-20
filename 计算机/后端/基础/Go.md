## Go语言

### 代码结构

```go
package main //包声明,main为可执行程序入口包

import "fmt" //导入标准库

func main() { //程序入口
    fmt.Println("Hello, Go")
}
```

- 每个Go程序由**包(package)**组成,`main`包为程序入口
- `import` 导入的包必须在代码中使用,否则编译报错
- 文件名小写,使用下划线分隔,如 `user_service.go`

### 变量

```go
var name string = "value" //完整声明
var age = 18              //类型推断
count := 10               //短变量声明(仅限函数内)
```

| 声明方式         | 作用域     | 说明             |
| ---------------- | ---------- | ---------------- |
| **var**          | 全局/局部  | 标准声明方式     |
| **:=**           | 仅函数内   | 短变量声明       |
| **const**        | 全局/局部  | 常量声明         |

- 变量声明后未赋值,自动获得**零值**(int→0, string→"", bool→false, pointer→nil)

### 常量

```go
const name = "value"       //单个常量
const (                    //批量常量
    StatusOK   = 200
    StatusFail = 500
)
```

| 进制     | 写法             |
| :------- | ---------------- |
| 二进制   | a := 0b1100100   |
| 八进制   | a := 0o144       |
| 十进制   | a := 100         |
| 十六进制 | a := 0x64        |

- **iota** 枚举常量,在const块中自增(从0开始)

```go
const (
    Sunday = iota  // 0
    Monday         // 1
    Tuesday        // 2
)
```

### 数据类型

#### 基本数据类型

| 整数型         | 字节数     | 范围                     |
| :------------- | :--------- | :----------------------- |
| **int8**       | **1 字节** | **-2^7^ ~ 2^7^-1**       |
| **int16**      | **2 字节** | **-2^15^ ~ 2^15^-1**     |
| **int32/rune** | **4 字节** | **-2^31^ ~ 2^31^-1**     |
| **int64**      | **8 字节** | **-2^63^ ~ 2^63^-1**     |
| **int**        | **平台相关** | 通常为64位               |
| **uint8/byte** | **1 字节** | **0 ~ 2^8^-1**           |
| **uint16**     | **2 字节** | **0 ~ 2^16^-1**          |
| **uint32**     | **4 字节** | **0 ~ 2^32^-1**          |
| **uint64**     | **8 字节** | **0 ~ 2^64^-1**          |

| 浮点型       | 字节数     | 说明           |
| :----------- | :--------- | :------------- |
| **float32**  | **4 字节** | 单精度浮点数   |
| **float64**  | **8 字节** | 双精度浮点数   |

| 其他类型       | 说明                     |
| :------------- | :----------------------- |
| **bool**       | true / false             |
| **string**     | UTF-8编码的不可变字符串  |
| **byte**       | uint8的别名              |
| **rune**       | int32的别名,表示Unicode码点 |

#### 引用数据类型

| 类型       | 说明                     |
| ---------- | ------------------------ |
| **slice**  | 动态数组                 |
| **map**    | 键值对集合               |
| **channel**| 并发通信管道             |
| **pointer**| 指针                     |
| **interface**| 接口                   |

#### 类型转换

```go
var i int = 100
var f float64 = float64(i) //int → float64
var s string = fmt.Sprintf("%d", i) //int → string
```

- Go**不支持隐式类型转换**,必须显式转换
- Go**没有**继承、包装类、泛型(1.18前)等概念

### 函数

```go
//函数定义
func 函数名(参数名 参数类型) 返回值类型 {
    return 返回值
}
```

#### 多返回值

```go
func divide(a, b float64) (float64, error) {
    if b == 0 {
        return 0, fmt.Errorf("除数不能为零")
    }
    return a / b, nil
}
```

#### 命名返回值

```go
func add(a, b int) (result int, err error) {
    result = a + b
    return //自动返回result和err
}
```

#### 可变参数

```go
func sum(nums ...int) int {
    total := 0
    for _, n := range nums {
        total += n
    }
    return total
}
sum(1, 2, 3)     //直接传入
sum(nums...)     //slice展开传入
```

- Go**没有方法重载**,同名函数只能有一个
- 函数是**一等公民**,可作为变量、参数、返回值

### 指针

```go
var a int = 10
var p *int = &a   //p指向a的地址
fmt.Println(*p)   //解引用,获取值:10
*p = 20           //通过指针修改a的值
```

- Go**只有值传递**,传递指针可实现"引用传递"效果
- Go**没有指针运算**
- `new(T)` 分配内存返回指针,`&T{}` 创建并返回指针

### 数组

```go
var arr [5]int                    //声明长度为5的数组
arr := [5]int{1, 2, 3, 4, 5}     //声明并初始化
arr := [...]int{1, 2, 3}         //自动推断长度
arr2 := [5]int{0: 1, 4: 5}       //指定下标初始化
```

- 数组长度**固定**,是类型的一部分(`[3]int` ≠ `[5]int`)
- 数组是**值类型**,赋值和传参都会复制

### Slice

```go
s := []int{1, 2, 3}        //声明slice
s := make([]int, 5)        //make创建,len=5,cap=5
s := make([]int, 0, 10)    //len=0,cap=10
s2 := s[1:3]               //切片操作(左闭右开)
```

| 操作             | 写法                     | 说明             |
| ---------------- | ------------------------ | ---------------- |
| **追加元素**     | `s = append(s, 1)`       | 返回新slice      |
| **复制**         | `copy(dst, src)`         | 复制元素         |
| **长度**         | `len(s)`                 | 元素个数         |
| **容量**         | `cap(s)`                 | 底层数组容量     |
| **删除元素**     | `s = append(s[:i], s[i+1:]...)` | 删除下标i  |

#### 遍历

```go
//for range遍历
for index, value := range s {
    fmt.Println(index, value)
}
//普通for循环
for i := 0; i < len(s); i++ {
    fmt.Println(s[i])
}
```

### Map

```go
m := make(map[string]int)            //创建空map
m := map[string]int{"a": 1, "b": 2}  //声明并初始化
```

| 操作           | 写法                        | 说明               |
| -------------- | --------------------------- | ------------------ |
| **添加/修改**  | `m["key"] = value`          | 键存在则覆盖       |
| **获取**       | `val := m["key"]`           | 键不存在返回零值   |
| **判断存在**   | `val, ok := m["key"]`       | ok为true表示存在   |
| **删除**       | `delete(m, "key")`          | 删除指定键         |
| **长度**       | `len(m)`                    | 键值对数量         |
| **遍历**       | `for k, v := range m {}`   | 无序遍历           |

### 结构体

```go
//定义结构体
type User struct {
    Name string
    Age  int
}

//创建实例
u := User{Name: "Tom", Age: 18}     //指定字段
u := User{"Tom", 18}                //按顺序
u := &User{Name: "Tom"}             //返回指针
u := new(User)                      //返回指针,字段为零值
```

### 方法

```go
//值接收者
func (u User) GetName() string {
    return u.Name
}

//指针接收者(可修改原值)
func (u *User) SetName(name string) {
    u.Name = name
}
```

- 方法绑定在**类型**上,类似Java的实例方法
- 使用**指针接收者**可修改原始数据
- 同一类型的方法,接收者类型应**统一**

### 接口

```go
//定义接口
type Animal interface {
    Speak() string
    Move() string
}

//实现接口(隐式实现,无需声明)
type Dog struct{}
func (d Dog) Speak() string { return "汪汪" }
func (d Dog) Move() string  { return "跑" }

type Cat struct{}
func (c Cat) Speak() string { return "喵喵" }
func (c Cat) Move() string  { return "跳" }

//使用接口(多态)
func MakeSound(a Animal) {
    fmt.Println(a.Speak())
}
MakeSound(Dog{}) //汪汪
MakeSound(Cat{}) //喵喵
```

| 接口概念       | 说明                                       |
| -------------- | ------------------------------------------ |
| **隐式实现**   | 无需implements关键字,实现方法即满足接口    |
| **空接口**     | `interface{}` / `any`(Go1.18+),可接收任何值 |
| **类型断言**   | `val, ok := i.(Type)` 判断接口变量类型     |
| **类型switch** | `switch v := i.(type)` 多类型分支判断      |
| **接口嵌入**   | 接口可组合其他接口                         |

### 组合(替代继承)

```go
//Go没有继承,使用结构体嵌入实现组合
type Animal struct {
    Name string
}
func (a Animal) Speak() string {
    return a.Name + "发出声音"
}

type Dog struct {
    Animal  //嵌入Animal,Dog拥有Animal的字段和方法
    Breed string
}

d := Dog{Animal: Animal{Name: "旺财"}, Breed: "柴犬"}
d.Speak() //直接调用嵌入结构体的方法
```

- Go使用**组合(嵌入)**替代继承
- 嵌入结构体的字段和方法可直接访问
- 外层可**重写**嵌入方法(类似override)

### 错误处理

```go
//error接口
type error interface {
    Error() string
}

//返回错误
func divide(a, b int) (int, error) {
    if b == 0 {
        return 0, errors.New("除数不能为零")
    }
    return a / b, nil
}

//错误处理
result, err := divide(10, 0)
if err != nil {
    fmt.Println("错误:", err)
    return
}
```

#### defer

```go
func readFile() {
    f, err := os.Open("file.txt")
    if err != nil { return }
    defer f.Close() //延迟执行,函数返回前关闭文件
    //...操作文件
}
```

- `defer` 语句在**函数返回前**执行
- 多个defer按**后进先出(LIFO)**顺序执行
- 常用于关闭文件、释放锁、数据库连接归还

#### panic / recover

```go
func safeDivide(a, b int) (result int) {
    defer func() {
        if err := recover(); err != nil {
            fmt.Println("恢复panic:", err)
            result = 0
        }
    }()
    return a / b //b为0时触发panic
}
```

| 概念         | 说明                                       |
| ------------ | ------------------------------------------ |
| **error**    | 普通错误,应正常处理                        |
| **panic**    | 严重错误,程序崩溃,类似Java的RuntimeException |
| **recover**  | 在defer中捕获panic,恢复程序执行            |
| **defer**    | 延迟执行,类似Java的finally                 |

### 字符串操作

```go
s := "Hello, World"
```

| 操作             | 写法                              | 说明               |
| ---------------- | --------------------------------- | ------------------ |
| **长度**         | `len(s)`                          | 字节长度           |
| **拼接**         | `s1 + s2`                         | 直接拼接           |
| **格式化**       | `fmt.Sprintf("%s-%d", s, n)`      | 格式化字符串       |
| **分割**         | `strings.Split(s, ",")`           | 返回[]string       |
| **替换**         | `strings.Replace(s, "old", "new", -1)` | -1表示全部替换 |
| **包含**         | `strings.Contains(s, "Hello")`    | 返回bool           |
| **前缀/后缀**    | `strings.HasPrefix(s, "He")`      | 返回bool           |
| **大小写**       | `strings.ToUpper(s)` / `strings.ToLower(s)` | 转换大小写 |
| **去空格**       | `strings.TrimSpace(s)`            | 去除首尾空白       |
| **子串**         | `s[0:5]`                          | 左闭右开           |
| **字符转byte**   | `[]byte(s)`                       | string → []byte    |
| **byte转字符**   | `string(b)`                       | []byte → string    |
| **遍历rune**     | `for i, r := range s {}`          | 按字符遍历         |

### IO操作

| os包方法         | 作用                     |
| ---------------- | ------------------------ |
| **os.Open**      | 打开文件(只读)           |
| **os.Create**    | 创建文件                 |
| **os.ReadFile**  | 读取整个文件(Go1.16+)   |
| **os.WriteFile** | 写入整个文件(Go1.16+)   |
| **os.Remove**    | 删除文件                 |
| **os.MkdirAll**  | 创建多级目录             |
| **os.Stat**      | 获取文件信息             |

```go
//读取文件
data, err := os.ReadFile("test.txt")
if err != nil {
    fmt.Println("读取失败:", err)
    return
}
fmt.Println(string(data))

//写入文件
err = os.WriteFile("test.txt", []byte("内容"), 0644)

//逐行读取
file, _ := os.Open("test.txt")
defer file.Close()
scanner := bufio.NewScanner(file)
for scanner.Scan() {
    fmt.Println(scanner.Text())
}
```

### 并发编程

#### goroutine

```go
//启动goroutine
go func() {
    fmt.Println("并发执行")
}()

//等待goroutine完成
var wg sync.WaitGroup
for i := 0; i < 5; i++ {
    wg.Add(1)
    go func(n int) {
        defer wg.Done()
        fmt.Println("goroutine", n)
    }(i)
}
wg.Wait() //阻塞等待所有goroutine完成
```

#### channel

```go
ch := make(chan int)       //无缓冲channel(同步)
ch := make(chan int, 10)   //有缓冲channel(异步)

//发送与接收
go func() { ch <- 42 }()  //发送数据
val := <-ch                //接收数据

//关闭channel
close(ch)

//遍历channel
for val := range ch {
    fmt.Println(val)
}

//select多路复用
select {
case msg := <-ch1:
    fmt.Println("收到ch1:", msg)
case msg := <-ch2:
    fmt.Println("收到ch2:", msg)
case <-time.After(3 * time.Second):
    fmt.Println("超时")
}
```

#### sync包

| 类型             | 作用                           |
| ---------------- | ------------------------------ |
| **sync.Mutex**   | 互斥锁,保护共享资源            |
| **sync.RWMutex** | 读写锁,允许多读单写            |
| **sync.WaitGroup** | 等待一组goroutine完成        |
| **sync.Once**    | 确保函数只执行一次             |
| **sync.Map**     | 并发安全的map                  |

```go
//互斥锁
var mu sync.Mutex
mu.Lock()
//操作共享资源
mu.Unlock()

//读写锁
var rwmu sync.RWMutex
rwmu.RLock()   //读锁
rwmu.RUnlock()
rwmu.Lock()    //写锁
rwmu.Unlock()

//只执行一次
var once sync.Once
once.Do(func() {
    fmt.Println("仅执行一次")
})
```

### 泛型(Go 1.18+)

```go
//泛型函数
func Max[T int | float64 | string](a, b T) T {
    if a > b {
        return a
    }
    return b
}

//泛型结构体
type Stack[T any] struct {
    items []T
}
func (s *Stack[T]) Push(item T) {
    s.items = append(s.items, item)
}
func (s *Stack[T]) Pop() (T, bool) {
    var zero T
    if len(s.items) == 0 {
        return zero, false
    }
    item := s.items[len(s.items)-1]
    s.items = s.items[:len(s.items)-1]
    return item, true
}

//类型约束
type Number interface {
    ~int | ~int64 | ~float32 | ~float64
}
func Sum[T Number](nums []T) T {
    var total T
    for _, n := range nums {
        total += n
    }
    return total
}
```

| 泛型概念       | 说明                                   |
| -------------- | -------------------------------------- |
| **[T any]**    | 类型参数,any等同于interface{}          |
| **~int**       | 底层类型为int的所有类型                |
| **constraints**| 类型约束,限制可用的类型范围            |
| **类型推断**   | 多数情况可省略类型参数                 |

### 数据库操作(database/sql)

- **安装驱动**

```shell
go get github.com/go-sql-driver/mysql
```

- **连接数据库**

```go
import (
    "database/sql"
    _ "github.com/go-sql-driver/mysql" //导入驱动(匿名导入)
)

db, err := sql.Open("mysql", "root:000000@tcp(127.0.0.1:3306)/my_project?charset=utf8mb4")
if err != nil {
    panic("数据库连接失败: " + err.Error())
}
defer db.Close()

db.SetMaxOpenConns(25)  //最大打开连接数
db.SetMaxIdleConns(5)   //最大空闲连接数
err = db.Ping()          //测试连接
```

- **CRUD操作**

```go
//添加
_, err = db.Exec("INSERT INTO user(name, age) VALUES(?, ?)", "Tom", 18)

//修改
_, err = db.Exec("UPDATE user SET age=? WHERE name=?", 20, "Tom")

//删除
_, err = db.Exec("DELETE FROM user WHERE name=?", "Tom")

//查询单行
var name string
var age int
err = db.QueryRow("SELECT name, age FROM user WHERE id=?", 1).Scan(&name, &age)

//查询多行
rows, err := db.Query("SELECT id, name, age FROM user WHERE age > ?", 18)
if err != nil { panic(err) }
defer rows.Close()
for rows.Next() {
    var id, age int
    var name string
    rows.Scan(&id, &name, &age)
    fmt.Println(id, name, age)
}
```

- **预处理语句**

```go
stmt, err := db.Prepare("INSERT INTO user(name, age) VALUES(?, ?)")
if err != nil { panic(err) }
defer stmt.Close()

for i := 0; i < 100; i++ {
    stmt.Exec(fmt.Sprintf("user%d", i), i)
}
```

- **事务操作**

```go
tx, err := db.Begin()
if err != nil { panic(err) }

_, err = tx.Exec("UPDATE account SET balance=balance-100 WHERE id=1")
if err != nil {
    tx.Rollback()
    return
}
_, err = tx.Exec("UPDATE account SET balance=balance+100 WHERE id=2")
if err != nil {
    tx.Rollback()
    return
}
tx.Commit()
```

### 常用标准库

| 包名             | 功能                               |
| ---------------- | ---------------------------------- |
| **fmt**          | 格式化输入输出                     |
| **os**           | 操作系统交互(文件、环境变量等)     |
| **io**           | IO原语接口                         |
| **bufio**        | 缓冲IO                             |
| **strings**      | 字符串操作                         |
| **strconv**      | 字符串与基本类型转换               |
| **sort**         | 排序                               |
| **math**         | 数学函数                           |
| **time**         | 时间日期                           |
| **sync**         | 并发同步原语                       |
| **context**      | 上下文控制(超时、取消)             |
| **net/http**     | HTTP服务器与客户端                 |
| **encoding/json**| JSON编解码                         |
| **errors**       | 错误处理                           |
| **log**          | 日志                               |
| **path/filepath**| 文件路径操作                       |
| **regexp**       | 正则表达式                         |
| **database/sql** | 数据库操作接口                     |
| **testing**      | 测试框架                           |

### 常用命令

| 命令               | 作用                       |
| ------------------ | -------------------------- |
| `go run main.go`   | 编译并运行                 |
| `go build`         | 编译生成可执行文件         |
| `go mod init 名称` | 初始化模块                 |
| `go mod tidy`      | 整理依赖                   |
| `go get 包名`      | 下载依赖包                 |
| `go test`          | 运行测试                   |
| `go fmt`           | 格式化代码                 |
| `go vet`           | 静态检查                   |
| `go env`           | 查看环境变量               |
