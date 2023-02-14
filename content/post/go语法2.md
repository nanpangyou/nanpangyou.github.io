---
title: "Go语法 2"
date: 2023-02-10T16:58:30+08:00
draft: true
---

### for

Go 只有一种循环结构：for 循环。

基本的 for 循环由三部分组成，它们用分号隔开：

初始化语句：在第一次迭代前执行
条件表达式：在每次迭代前求值
后置语句：在每次迭代的结尾执行
初始化语句通常为一句短变量声明，该变量声明仅在 for 语句的作用域中可见。

一旦条件表达式的布尔值为 false，循环迭代就会终止。

注意：和 C、Java、JavaScript 之类的语言不同，Go 的 for 语句后面的三个构成部分外没有小括号， 大括号 { } 则是必须的。

```go
package main

import "fmt"

func main() {
 sum := 0
 for i := 0; i < 10; i++ {
  sum += i
 }
 fmt.Println(sum)
}
```

初始化语句和后置语句是可选的。

```go
package main

import "fmt"

func main() {
 sum := 1
 for ; sum < 1000; {
  sum += sum
 }
 fmt.Println(sum)
}

```

 此时你可以去掉分号，因为 C 的 while 在 Go 中叫做 for。(for 是 Go 中的 “while”)

```go
package main

import "fmt"

func main() {
 sum := 1
 for sum < 1000 {
  sum += sum
 }
 fmt.Println(sum)
}
```

### if 和 else

在 if 的简短语句中声明的变量同样可以在任何对应的 else 块中使用。

### defer

defer 语句会将函数推迟到外层函数返回之后执行。

推迟调用的函数其参数会立即求值，但直到外层函数返回前该函数都不会被调用。

```go
package main

import "fmt"

func main() {
 defer fmt.Println("world")

 fmt.Println("hello")
}
```

### defer 栈

推迟的函数调用会被压入一个栈中。当外层函数返回时，被推迟的函数会按照后进先出的顺序调用。
