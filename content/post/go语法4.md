---
title: "Go语法4"
date: 2023-02-13T09:18:17+08:00
draft: true
---

## 方法

Go 没有类。不过你可以为结构体类型定义方法。

方法就是一类带特殊的 接收者 参数的函数。

方法接收者在它自己的参数列表内，位于 func 关键字和方法名之间。

```go
package main

import (
 "fmt"
 "math"
)

type Vertex struct {
 X, Y float64
}

func (v Vertex) Abs() float64 {
 return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func main() {
 v := Vertex{3, 4}
 fmt.Println(v.Abs())
}
```

### 方法即函数

记住：方法只是个带接收者参数的函数。

现在这个 Abs 的写法就是个正常的函数，功能并没有什么变化。

```go
package main

import (
 "fmt"
 "math"
)

type Vertex struct {
 X, Y float64
}

func Abs(v Vertex) float64 {
 return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func main() {
 v := Vertex{3, 4}
 fmt.Println(Abs(v))
}
```

你也可以为非结构体类型声明方法。

在此例中，我们看到了一个带 Abs 方法的数值类型 MyFloat。

你只能为在同一包内定义的类型的接收者声明方法，而不能为其它包内定义的类型（包括 int 之类的内建类型）的接收者声明方法。

```go
package main

import (
 "fmt"
 "math"
)

type MyFloat float64

func (f MyFloat) Abs() float64 {
 if f < 0 {
  return float64(-f)
 }
 return float64(f)
}

func main() {
 f := MyFloat(-math.Sqrt2)
 fmt.Println(f.Abs())
}
```
