---
title: "Go语法"
date: 2023-02-07T11:53:08+08:00
---


## golang简单语法

### Hello Wold

```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, World!")
}
```

### 包

按照约定，包名与导入路径的最后一个目录一致。例如，`"math/rand"` 包由 `package rand` 语句导入。

```go
package main

import (
	"fmt"
	"math/rand"
)

func main() {
	fmt.Println("My favorite number is", rand.Intn(10))
}


```

### 导入

```go

import "fmt"
import "math"
// 和以下形式等价（分组导入）
import (
	"fmt"
	"math"
)

```

### 导出名

在 Go 中，如果一个名字以大写字母开头，那么它就是已导出的。例如，Pizza 就是个已导出名，Pi 也同样，它导出自 math 包。pizza 和 pi 则是未导出的。

### 函数

函数可以没有参数或接受多个参数。

在本例中，add 接受两个 int 类型的参数。

注意类型在变量名 之后。

```go

package main

import "fmt"

func add(x int, y int) int {
	return x + y
}

func main() {
	fmt.Println(add(42, 13))
}

```

当连续两个或多个函数的已命名形参类型相同时，除最后一个类型以外，其它都可以省略。

`x int, y int` 等价于 `x, y int`

### 多值返回

函数可以返回任意数量的返回值。

swap 函数返回了两个字符串。

```go
package main

import "fmt"

func swap(x, y string) (string, string) {
	return y, x
}

func main() {
	a, b := swap("hello", "world")
	fmt.Println(a, b)
}

```

### 命名返回值

Go 的返回值可被命名，它们会被视作定义在函数顶部的变量。

返回值的名称应当具有一定的意义，它可以作为文档使用。

没有参数的 return 语句返回已命名的返回值。也就是 直接 返回。

```go
package main

import "fmt"

func split(sum int)( x int) {
	x = sum * 4 / 9
	c := 19
	fmt.Println(c)
	return c
}

func main() {
	fmt.Println(split(17))
}
```

### 短变量声明

在函数中，简洁赋值语句 := 可在类型明确的地方代替 var 声明。

函数外的每个语句都必须以关键字开始（var, func 等等），因此 := 结构不能在函数外使用。