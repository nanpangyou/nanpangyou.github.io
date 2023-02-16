---
title: "Go语法3"
date: 2023-02-11T13:19:29+08:00
draft: true
---

## 指针

Go 拥有指针。指针保存了值的内存地址。

`&` 操作符 —— 取地址，`*` 操作符 —— 根据地址取值

类型 `*T` 是指向 T 类型值的指针。其零值为 nil。

```go
var p *int
```

`&` 操作符会生成一个指向其操作数的指针。

```go
i := 42
p = &i
```

* 操作符表示指针指向的底层值。

```go
fmt.Println(*p) // 通过指针 p 读取 i
*p = 21         // 通过指针 p 设置 i
```

这也就是通常所说的“间接引用”或“重定向”。

与 C 不同，Go 没有指针运算。

```go
package main

import "fmt"

func main() {
 i, j := 42, 2701

 p := &i         // 指向 i
 fmt.Println(*p) // 通过指针读取 i 的值
 *p = 21         // 通过指针设置 i 的值
 fmt.Println(i)  // 查看 i 的值

 p = &j         // 指向 j
 *p = *p / 37   // 通过指针对 j 进行除法运算
 fmt.Println(j) // 查看 j 的值
}
```

## 结构体

一个结构体（struct）就是一组字段（field）

```go
package main

import "fmt"

type Vertex struct {
 X int
 Y int
}

func main() {
 fmt.Println(Vertex{1, 2})
}

```

结构体字段使用点号来访问。

```go
package main

import "fmt"

type Vertex struct {
 X int
 Y int
}

func main() {
 v := Vertex{1, 2}
 v.X = 4
 fmt.Println(v.X)
}
```

结构体字段可以通过结构体指针来访问。

如果我们有一个指向结构体的指针 p，那么可以通过 (*p).X 来访问其字段 X。不过这么写太啰嗦了，所以语言也允许我们使用隐式间接引用，直接写 p.X 就可以。

```go
package main

import "fmt"

type Vertex struct {
 X int
 Y int
}

func main() {
 v := Vertex{1, 2}
 p := &v
 p.X = 1e9
 fmt.Println(v)
}
```

结构体文法通过直接列出字段的值来新分配一个结构体。

使用 Name: 语法可以仅列出部分字段。（字段名的顺序无关。）

特殊的前缀 & 返回一个指向结构体的指针。

```go
package main

import "fmt"

type Vertex struct {
 X, Y int
}

var (
 v1 = Vertex{1, 2}  // 创建一个 Vertex 类型的结构体
 v2 = Vertex{X: 1}  // Y:0 被隐式地赋予
 v3 = Vertex{}      // X:0 Y:0
 p  = &Vertex{1, 2} // 创建一个 *Vertex 类型的结构体（指针）
)

func main() {
 fmt.Println(v1, p, v2, v3)
 // 输出 {1 2} &{1 2} {1 0} {0 0}
}

```

## 数组

类型 `[n]T` 表示拥有 n 个 T 类型的值的数组。

表达式

`var a [10]int`
会将变量 a 声明为拥有 10 个整数的数组。

```go
package main

import "fmt"

func main() {
 var a [2]string
 a[0] = "Hello"
 a[1] = "World"
 fmt.Println(a[0], a[1])
 // Hello World
 fmt.Println(a)
 // [Hello World]

 primes := [6]int{2, 3, 5, 7, 11, 13}
 fmt.Println(primes)
 // [2 3 5 7 11 13]
}
```

## 切片

每个数组的大小都是固定的。而切片则为数组元素提供动态大小的、灵活的视角。在实践中，切片比数组更常用。

类型 `[]T` 表示一个元素类型为 T 的切片。

切片通过两个下标来界定，即一个上界和一个下界，二者以冒号分隔：

`a[low : high]`
它会选择一个半开区间，包括第一个元素，但排除最后一个元素。

以下表达式创建了一个切片，它包含 a 中下标从 1 到 3 的元素：

`a[1:4]`

```go
package main

import "fmt"

func main() {
 primes := [6]int{2, 3, 5, 7, 11, 13}

 var s []int = primes[1:4]
 fmt.Println(s)
 // [3 5 7]
}

```

切片就像数组的引用
切片并不存储任何数据，它只是描述了底层数组中的一段。

更改切片的元素会修改其底层数组中对应的元素。

与它共享底层数组的切片都会观测到这些修改。

```go
package main

import "fmt"

func main() {
 names := [4]string{
  "John",
  "Paul",
  "George",
  "Ringo",
 }
 fmt.Println(names)

 a := names[0:2]
 b := names[1:3]
 fmt.Println(a, b)

 b[0] = "XXX"
 fmt.Println(a, b)
 fmt.Println(names)
//[John Paul George Ringo]
// [John Paul] [Paul George]
// [John XXX] [XXX George]
// [John XXX George Ringo]
}
```

切片文法
切片文法类似于没有长度的数组文法。

这是一个数组文法：

`[3]bool{true, true, false}`
下面这样则会创建一个和上面相同的数组，然后构建一个引用了它的切片：

`[]bool{true, true, false}`

切片的默认行为
在进行切片时，你可以利用它的默认行为来忽略上下界。

切片下界的默认值为 0，上界则是该切片的长度。

对于数组

`var a [10]int`
来说，以下切片是等价的：

```go
a[0:10]
a[:10]
a[0:]
a[:]
```

## 切片的长度与容量

切片拥有 长度 和 容量。

切片的长度就是它所包含的元素个数。

切片的容量是从它的第一个元素开始数，到其底层数组元素末尾的个数。

切片 s 的长度和容量可通过表达式 len(s) 和 cap(s) 来获取。

你可以通过重新切片来扩展一个切片，给它提供足够的容量。试着修改示例程序中的切片操作，向外扩展它的容量，看看会发生什么。

```go
package main

import "fmt"

func main() {
 s := []int{2, 3, 5, 7, 11, 13}
 printSlice(s)

 // 截取切片使其长度为 0
 s = s[:0]
 printSlice(s)

 // 拓展其长度
 s = s[:4]
 printSlice(s)

 // 舍弃前两个值
 s = s[2:]
 printSlice(s)
}

func printSlice(s []int) {
 fmt.Printf("len=%d cap=%d %v\n", len(s), cap(s), s)
 // len=6 cap=6 [2 3 5 7 11 13]
 // len=0 cap=6 []
 // len=4 cap=6 [2 3 5 7]
 // len=2 cap=4 [5 7]
}
```

## nil 切片

切片的零值是 nil。

nil 切片的长度和容量为 0 且没有底层数组。

## 用 make 创建切片

切片可以用内建函数 make 来创建，这也是你创建动态数组的方式。

make 函数会分配一个元素为**零值**的数组并返回一个引用了它的切片：

```go
a := make([]int, 5)  // len(a)=5
```

要指定它的容量，需向 make 传入第三个参数：

```go
b := make([]int, 0, 5) // len(b)=0, cap(b)=5

b = b[:cap(b)] // len(b)=5, cap(b)=5
b = b[1:]      // len(b)=4, cap(b)=4
```

```go
package main

import "fmt"

func main() {
 a := make([]int, 5)
 printSlice("a", a)

 b := make([]int, 0, 5)
 printSlice("b", b)

 c := b[:2]
 printSlice("c", c)

 d := c[2:5]
 printSlice("d", d)
}

func printSlice(s string, x []int) {
 fmt.Printf("%s len=%d cap=%d %v\n",
  s, len(x), cap(x), x)
}

```

## 切片的切片

切片可包含任何类型，甚至包括其它的切片。

## 向切片追加元素

为切片追加新的元素是种常用的操作，为此 Go 提供了内建的 append 函数。

`func append(s []T, vs ...T) []T`

append 的第一个参数 s 是一个元素类型为 T 的切片，其余类型为 T 的值将会追加到该切片的末尾。

append 的结果是一个包含原切片所有元素加上新添加元素的切片。

当 s 的底层数组太小，不足以容纳所有给定的值时，它就会分配一个更大的数组。返回的切片会指向这个新分配的数组。

```go
package main

import "fmt"

func main() {
 var s []int
 printSlice(s)

 // 添加一个空切片
 s = append(s, 0)
 printSlice(s)

 // 这个切片会按需增长
 s = append(s, 1)
 printSlice(s)

 // 可以一次性添加多个元素
 s = append(s, 2, 3, 4)
 printSlice(s)
}

func printSlice(s []int) {
 fmt.Printf("len=%d cap=%d %v\n", len(s), cap(s), s)
}
// len=0 cap=0 []
// len=1 cap=1 [0]
// len=2 cap=2 [0 1]
// len=5 cap=6 [0 1 2 3 4]
// 目前发现的规律 cap的值永远为2的n次方，且cap的值永远大于等于len的值
```

## Range

for 循环的 range 形式可遍历切片或映射。

当使用 for 循环遍历切片时，每次迭代都会返回两个值。第一个值为当前元素的下标，第二个值为该下标所对应元素的一份副本。

```go
package main

import "fmt"

var pow = []int{1, 2, 4, 8, 16, 32, 64, 128}

func main() {
 for i, v := range pow {
  fmt.Printf("%d = %d\n", i, v)
 }
}
// 输出
// 0 = 1
// 1 = 2
// 2 = 4
// 3 = 8
// 4 = 16
// 5 = 32
// 6 = 64
// 7 = 128
```

可以将下标或值赋予 `_` 来忽略它。

```go
for i, _ := range pow
for _, value := range pow
```

若你只需要索引，忽略第二个变量即可。

```go
for i := range pow
```

## 映射

映射将键映射到值。

映射的零值为 nil 。nil 映射既没有键，也不能添加键。

make 函数会返回给定类型的映射，并将其初始化备用。
