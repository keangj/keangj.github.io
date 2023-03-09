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

### 常量

常量的声明与变量类似，使用 `const` 关键字。常量时不能修改的，且不能使用 `:=` 声明

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
const (
	APPLE = iota	// 0
  BANANA				// 1
  ORANGE				// 2
)
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
  return
}
// 同形参一样可以省略除最后一个类型外的其他类型
func fn2(a string, b int) (c, d int) {
  c = 888
  d = 666
  return
}
```



### 数组

[n]T

``` go
var a [2]int	// 表示变量 a 为两个整数类型的数组
a[0] = 1
a[1] = 2
b := [2]string{"hello", "world"}	// 变量 b 为两个字符串类型的数组，并为其定义了初始值
```

### 切片（slice）

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
a[:]
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

### 切片文法

切片文法类似于没有长度的数组文法

动态数组在传参上是引用传递，而且不同元素长度的动态数组它们的形参是一致的

``` go
// 数组
[3]int{1, 2, 3}
// 切片
[]int{1, 2, 3}
```

切片声明的方法

``` go
slice := []int{1, 2, 3}

var slice2 []int
slice2 = make([]int, 3, 5) // 3 表示长度，5 表示容量

var slice3 []int = make([]int, 3)

slice4 := make([]int, 3)
```



### 切片的长度与容量

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
numbers = append(numbers, 2)
fmt.Println(numbers, len(numbers), cap(numbers)) // [0 0 0 1 2] 5 8
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
p = &i // p 指向 i 的地址
```

`*` 操作符表示指针指向的底层值

``` go
fmt.Println(*p) // 通过指针 p 读取 i
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
```

