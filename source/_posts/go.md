---
title: Go
tags: [Go]
date: 2023-04-20 22:00:00
updated: 2023-04-20 20:00:00
categories: 
  - ['Go']
cover: https://unpkg.com/keangj-assets@1.0.6/img/go.png

---

# Go

## 包

包导入方式

``` go
import "fmt"
import "time"
// or
// 分组导入
import (
	"fmt"
  "time"
)

import (
  _"fmt" // 在包名前加 _ 可以匿名导入，无法使用当前包的方法，但会执行当前包内部的 init() 方法
  lib "time"	// 别名
  . "lib" // 加 . 可以将当前包中的全部方法导入本包中
)
```

## 简单值类型

- 数字

  ``` go
  int64
  uint64
  float64
  float32
  complex64
  ```

- 字符串

  ``` go
  // 字符串必须用双引号
  var str string = "字符串"
  str2 := "字符串"
  ```

- 布尔

  ``` go
  a := true
  b := false
  ```
  
  

## 结构体（struct）

``` go
type User struct {
  // 结构体属性的首字母是大写,为共有属性.首字母是小写,为私有属性.
  Username string
  Age      int
  skin		 string // 私有属性 
}
func main() {
  u := User{Username: "tom", Age: 18}
  u2 := User{"jerry", 18}	// key 可以省略，结构体的顺序是固定的
}
```

结构体参数

``` go
type User struct {
	Username string
	Age      int
}

func rename(u User) { // 此处传递的是值
	u.Username = "jay"
}
func rename2(u *User) { // 此处传递的是指针
  (*u).Username = "jay" // (*u).Username 可被简写为 u.Username
}
func main() {
	u := User{"tom", 18}
	rename(u)
	fmt.Println("rename", u) // {"tom", 18}，值没有修改
	rename2(&u)
	fmt.Println("rename2", u) // {"jay", 18}，值被修改
}
```

继承

``` go
// 父
type Animal struct {
  name string
  age	int
}
// 父方法
func (a *Animal) Eat() {
  fmt.Println("Human Eat")
}
func (a *Animal) Run() {
  fmt.Println("Human Run")
}

type Dog s struct {
  Animal	// Dog 继承  Animal
  skin string
}

func (d *Dog) Run() {
  fmt.Println("Dog Run")
}
func (d *Dog) Bark() {
  fmt.Println("Dog 汪")
}

func main() {
  a := Animal{"tom", 18}
  a.Eat()	// Human Eat
  a.Run()	// Human Run
  
  // 两种方式
  d := Dog{Animal{"旺财", 20}, skin: "yellow"}
  // or
  var d Dog
  d.name = "旺财"
  d.age = 20
  d.skin = "yellow"
  
  d.Eat() // Human Eat
  d.Run() // Dog Eat
  d.Bark()	// Dog 汪
}
```



## Interface





## 数组

[n]T

``` go
var a [2]int	// 表示变量 a 为两个整数类型的数组
a[0] = 1
a[1] = 2
b := [2]string{"hello", "world"}	// 变量 b 为两个字符串类型的数组，并为其定义了初始值
c := [...]int{1, 2, 3} // 长度为 3 的数组，... 根据实际数量设置长度
```

可以用 `len(value)` 获取数组的长度

``` go
var a [6]int
fmt.Println(len(a)) // 6
```



## 切片（slice）

数组的大小是固定的。切片可以为数组提供动态的大小

a[low : high]

low (下界，默认值为 0)为开始下标，high （上界，默认值为数组长度）为结束下标（结束下标不包含结束下标的元素）

``` go
func main() {
	primes := [6]int{2, 3, 5, 7, 11, 13}
	var s []int = primes[1:4]	// 取 primes 下标为 1 到下标为 3 的元素
	fmt.Println(s)	// [3 5 7]
}
var a [10]int
// 等价
a[0:10]
a[:10]
a[0:]
a[:]	// 为 a[0:len] 的简写
```

切片为数组的引用

``` go
names := [3]string {
  "jay",
  "tom",
  "jerry",
}
fmt.Println(names)	// [jay tom jerry]
a := names[0:2]
b := names[1:3]
fmt.Println(a, b)	// [jay tom] [tom jerry]
a[1] = "hello wrold"
fmt.Println(names)	// [jay hello world jerry]
fmt.Println(a, b)	// [jay hello world] [hello world jerry]
```

切片的实现

``` go
type slice struct {
  array unsafe.Pointer // 数组的指针
  len int // 长度
  cap int // 容量
}
```



- 切片文法

  切片文法类似于没有长度的数组文法

  动态数组在传参上是引用传递，而且不同元素长度的动态数组它们的形参是一致的

  ``` go
  // 数组
  [3]int{1, 2, 3}
  // 切片
  []int{1, 2, 3}
  ```


- 切片声明的方法

  ``` go
  slice := []int{1, 2, 3}
  
  var slice2 []int
  slice2 = make([]int, 3, 5) // 3 表示长度，5 表示容量
  
  var slice3 []int = make([]int, 3)
  
  slice4 := make([]int, 3)
  ```

- 切片的长度和容量

  切片拥有 **长度** 和 **容量**。

  切片的长度就是它所包含的元素个数。

  切片的容量是从它的第一个元素开始数，到其底层数组元素末尾的个数。

  切片 `s` 的长度和容量可通过表达式 `len(s)` 和 `cap(s)` 来获取。

  ``` go
  s := []int{1, 2, 3, 4, 5}
  // 截取的切片和原切片指向同一个地址，可以使用 copy() 将底层数组的值一起进行拷贝
  s = S[:0]	// 截取切片使其长度为 0
  s = s[:4] // 拓展其长度
  s = s[2:] // 舍弃前两个值
  ```

  切片的零值是 `nil`，nil 切片的长度和容量为 0 且没有底层数组。

  ``` go
  var s []int
  fmt.Println(s, len(s), cap(s))	// [] 0 0
  fmt.Println(s == nil)	// true
  ```

  向切片追加元素

  ``` go
  var numbers = make([]int, 3, 4)
  fmt.Println(numbers, len(numbers), cap(numbers))	// [0 0 0] 3 4
  numbers = append(numbers, 1)
  fmt.Println(numbers, len(numbers), cap(numbers)) // [0 0 0 1] 4 4
  // 向一个容量已经满的切片追加元素，会拓展一个新的容量（新的容量为之前容量的两倍）
  // 每次扩容都会生成一个新的地址
  numbers = append(numbers, 2)
  fmt.Println(numbers, len(numbers), cap(numbers)) // [0 0 0 1 2] 5 8
  ```
  



## 变量

声明方式

``` go
// 声明一个变量默认值为 0
var a int

// 声明一个变量，并为它赋一个初始值
var b int = 1

// 声明时如果存在初始值，则可以省略数据类型
var c = "hello world"

// 省略关键字，只能在函数内使用
func main() {
	d := "hi"
}

// 声明多个变量
var e, f int = 1, 2
var g, h = 6, "jay"

// 多行多变量声明
var (
	i int = 66
  j bool = false
)
```

## 常量

常量的声明与变量类似，使用 `const` 关键字。常量不可以修改，且不能使用 `:=` 声明

``` go
const n int = 10
```

可以使用 `const` 定义枚举类型

``` go
const (
	APPLE = 0
  BANANA = 1
  ORANGE = 2
)

// 可以添加 iota 关键字，试每行的 iota 都累加 1，第一行的 iota 默认值为 0
// 使用 iota 可以减少 hard code，只能在 const() 中使用
const (
	APPLE = iota	// 0
  BANANA				// 1
  ORANGE				// 2
)
```



## 条件语句

### if else

``` go
// 第一个表达式声明的变量只存在于大括号作用域中，第一个表达式可省略
if num := 9; num < 0 {
  fmt.Println(num)
} else if num < 10 {
  fmt.Println(num)
} else {
  fmt.Println(num)
}

fmt.Println(num) // error
```

### switch case

``` go
// case 可以有多个值，用逗号隔开。默认不用加 break，执行完不会继续执行。如需继续执行需要在 case 后加 fallthrough
switch time.Now().Weekday() {
case time.Saturday, time.Sunday:
  fmt.Println("休息日")
  fallthrough
default:
  fmt.Println("工作日")
}
```

判断类型

``` go
var i any = true
switch t := i.(type) {
case bool:
  fmt.Println("布尔")
case int:
  fmt.Println("整数")
default:
  fmt.Printf("未知类型 %T\n", t)
}
```



## for 循环

``` go
// 三个表达式，等价 JS 的 for(初始化:在第一次迭代前执行; 条件表达式:在每次迭代前求值; 后续:在每次迭代的结尾执行)
// 初始化时的变量声明仅在 for 的作用域中存在
for j := 1; j <= 3; j++ {
  fmt.Println(j)
}

// 一个表达式，等价 JS 的 while(表达式)
i := 1
for i <= 3 {
  fmt.Println(i)
  i++
}

// 没有表达式，等价 JS 的 while(true)
k := 1
for {
  if k > 3 {
    break	// 没有 break 将无限循环
  }
  fmt.Println("loop")
  k++
}
```



## Range 遍历

``` go
// 用range可以枚举 array、slice、string、map、channel等不同类型
// 对于channel，range返回一个值，
// array、slice、string、map等其他类型返回一对儿
nums := []int{2, 3, 4}
for k, v := range nums {
	fmt.Println("num:", v)	// 依次打印出 2 3 4
	fmt.Println("i:", k)	// 依次打印出 0 1 2
}
```



## 函数

``` go
// 使用 func 声明函数，形参的类型需写在形参后
func add(a int, b int) int {
  return a + b
}

// 连续有多个函数形参类型相同时，除了最后一个类型外，其他的可以省略
func add(a, b int) int {
  return a + b
}

// 多返回值
func fn(a string, b int) (string, int) {
  return a, b
}

func fn2(a string, b int) (c int, d int) {
  // 给有名称的返回值变量赋值
  c = 888
  d = 666
  // 返回值已经声明，不需要在 return 后写
  return
}
func fn2(a string, b int) (c int, d int) {
  c = 888
  d = 666
  return 1, 2 // 此处返回 1, 2 回覆盖声明的返回
}
// 同形参一样可以省略除最后一个类型外的其他类型
func fn2(a string, b int) (c, d int) {
  c = 888
  d = 666
  return
}
```

可变参数函数

``` go
// numbers 为 int 的数组（切片）
func sum(numbers ...int) int {
	total := 0
	for _, n := range numbers {
		total += n
	}
	return total
}
func main() {
	fmt.Println(sum(1, 2))
	fmt.Println(sum(1, 2, 3))
	num := []int{1, 2, 3, 4}
  // ... 和 JS 的解构类似，不过 GO 的 ... 在变量后
	fmt.Println(sum(num...))
}
```

匿名函数

``` go
sum := func(a, b int) int {
  return a + b
}
sum(1, 2)

// 声明一个匿名函数并立即调用
func(a, b int) int {
  return a + b
}(1, 2)
```



## 指针

指针保存了值的内存地址，类似于 `JS` 中的引用类型

``` go
func swap(x *int, y *int) {
  var temp int
  temp = *x
  *x = *y
  *y = temp
}
func main() {
  a := 1
  b := 2
  swap(&a, &b)
  fmt.Println("a:", a, "b:", b) // a: 2 b: 1
}
```

类型 `*T` 是指向 `T` 类型值的指针。其零值为 `nil`

`&` 操作符会生成一个指向其操作数的指针

``` go
i := 1
p = &i // p 是一个指针，指向 i 的地址
fmt.Println(p) // 直接打印指针 p 获取的是指针的地址：0x1400001c0d0
fmt.Println(&i) // 通过取址符 & 获取 i 的地址：0x1400001c0d0
fmt.Println(*p)	// 通过 * 获取指针的值：1
fmt.Println(&p) // 通过取址符 & 获取指针 p 的地址：0x1400000e028
```

`*` 操作符表示指针指向的底层值

``` go
fmt.Println(*p) // 通过指针 p 读取 i。通过 * 操作符
*p = 21         // 通过指针 p 设置 i
```



## defer

`defer` 关键字会将函数推迟到外层函数返回之后执行。

推迟的函数调用会被压入一个栈中。当外层函数返回时，被推迟的函数会安装后近先出的顺序调用

推迟调用的函数其参数会立即求值，但直到外层函数返回前该函数都不会被调用。

``` go
fmt.Println("counting")
for i := 0; i < 3; i++ {
  defer fmt.Println(i)
}
fmt.Println("done")
// counting
// done
// 2 最后被压入栈首先执行
// 1 第二个被压入栈
// 0 最先被压入栈最后执行
```

``` go
func returnFn () int {
  fmt.Println("return fn")
  return 0
}
func deferFn () {
  fmt.Println("defer fn")
}
func fn() int {
  defer deferFn()
  return returnFn()
}
// return fn
// defer fn 在函数 return 后执行
```



## Map

声明 map

``` go
// 变量 mapExample 是 map 类型，key 是 string，value 是 string
var mapExample map[string]string
// map 在使用前需要先用 make 为其分配数据空间
mapExample = make(map[string]string, 10)
mapExample["t1"] = "test1"
mapExample["t2"] = "test2"
fmt.Println(mapExample) // map[t1:test1 t2:test2]

// 使用 make 声明
mapExample2 := make(map[string]string)

// 直接使用 map 声明
mapExample3 := map[string]string {
  "t1": "test1",
  "t2": "test2"
}
```

map 的使用

``` go
mapExample := make(map[string]string)
// 添加
mapExample["t1"] = "type1"
mapExample["t2"] = "type2"
// 修改
mapExample["t2"] = "type3"
// 删除
delete(mapExample, "t2")
// 遍历
for key, value := range mapExample {
  fmt.Println(key, value)
}
// 是否有 key
_, hasKey := mapExample["t2"]
// 获取 length
fmt.Println(len(mapExample))
```





## go mod

``` sh
go mod download # 下载 go.mod 文件中指明的所有依赖
go mod edit # 编辑 go.mod 文件
go mod graph # 查看现有的依赖结构
go mod init # 生成 go.mod 文件
go mod tidy # 整理现有的依赖
go mod vendor # 导出项目所有的依赖到vendor目录
go mod verify # 校验一个模块是否被篡改过
go mod why # 查看为什么需要依赖某模块
```



## go 相关命令

``` sh
go version	# 查看版本
go env # 查看环境变量
go env -w GO111MODULE=on # 设置 GO111MODULE
# aoto: 默认值, 项目包含 go.mod 文件时启用 Go modules. on: 启用, 推荐设置. off: 禁用
go env GO111MODULE	# 查看设置
go env -w GOPROXY=https://goproxy.cn,direct # 设置代理, 也可以把 GOPROXY=https://goproxy.cn,direct 加到 go 命令前来临时使用. 如: GOPROXY=https://goproxy.cn,direct go mod tidy
go env GOPROXY	# 查看代理
```

