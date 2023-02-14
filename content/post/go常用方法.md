---
title: "Go常用方法"
date: 2023-02-14T15:33:33+08:00
---

## go 中常用的方法

### strings

 字符串长度

 ```go
  s := "hello"
  fmt.Println(len(s))
 ```

 字符串分割

 ```go
  s := "hello,world"
  fmt.Println(strings.Split(s, ","))
 ```

 字符串包含

 ```go
  s := "hello,world"
  fmt.Println(strings.Contains(s, "hello"))
 ```

 大小写转换

 ```go
  s := "hello,world"
  fmt.Println(strings.ToUpper(s))
  fmt.Println(strings.ToLower(s))
 ```

 是否包含前缀或后缀

 ```go
  s := "hello,world"
  fmt.Println(strings.HasPrefix(s, "hello"))
  fmt.Println(strings.HasSuffix(s, "world"))
 ```

 字符串替换

 ```go
  s := "hello,world"
  fmt.Println(strings.Replace(s, "hello", "hi", 1))
 ```

 字符串拼接

 ```go
  s := []string{"hello", "world"}
  fmt.Println(strings.Join(s, ","))
 ```

 字符串查找

 ```go
  s := "hello,world"
  fmt.Println(strings.Index(s, "world"))
 ```

 字符串查找（从后）

 ```go
  s := "hello,world"
  fmt.Println(strings.LastIndex(s, "world"))
 ```
