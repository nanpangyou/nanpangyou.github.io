---
title: "TS 泛型"
date: 2022-12-22T12:53:32+08:00
---

## TypeScript 泛型

1. 简单泛型

```ts

type Union<A, B> = A ｜ B
type Intersect<A, B> = A & B

interface List<A> {
  [key: number]: A
}

interface Hash<T> {
  [key: string]: T
}
```

2. 稍微复杂一点的泛型(extends)

```ts

type Person = { name: string }

// 补全问号处，使得下面的代码能够正常工作
type LikeString<T> = ？？？
type LikeNumber<T> = ？？？
type LikePerson<T> = ???

// 答案
type LikeString<T> = T extends string ? true : false // T 所代表的集合要么是 string，要么是 string 的子集
type LikeNumber<T> = T extends number ? 1 : 2 // T 所代表的集合要么是 number，要么是 number 的子集
type LikePerson<T> = T extends Person ? 'yes' : 'no' // T 所代表的集合要么是 Person，要么是 Person 的子集

type R1 = LikeString<'hi'>  // true
type R2 = LikeString<true>  // false
type R3 = LikeNumber<6666>  // 1
type R4 = LikeNumber<false>  // 2
type R5 = LikePerson<{ name: 'frank', xxx: 1 }>  // yes
type R6 = LikePerson<{ xxx: 1 }>  // no

```

**extends** 有两点要注意

* 若 T 为 never，则表达式的值为 never
* 若 T 为联合类型，则分开计算

这两条规则只在泛型中有用，其他地方都不会有这种情况

```ts

type X1 = LikeString<never> // X1 的type为never

type ToArray<T> = T extends unknown ? T[] : never
type Result = ToArray<string | number> // Result 的type为 string[] | number[]
type Result2 = ToArray<never> // Result2 的type为 never

```

3. 另一个稍微复杂一点的泛型(keyof)

```ts

type Person = { name: string, age: number }

type GetKeys<T> = ？？？
// 答案
type GetKeys<T> = keyof T

type Result = GetKeys<Person> // Result 的type为 'name' | 'age'

```

4. 另一个稍微复杂一点的泛型(extends keyof) (泛型约束)

```ts

type Person = { name: string, age: number }

type GetKeyType = ???

// 答案
type GetKeyType<T, K extends keyof T> = T[K]

type Result = GetKeyType<Person, 'name'> // Result 的type为 string

type X = Person['name'] // X 的type为 string

```

5. 复杂一点的泛型(TS自带的泛型)

```ts

type Person = { name: string, age: number }

type X1 = Readonly<Person> // X1 的type为 { readonly name: string; readonly age: number; }

type X2 = Partial<Person> // X2 的type为 { name?: string | undefined; age?: number | undefined; } 

type X3 = Required<Person> // X3 的type为 { name: string; age: number; }  去除参数的 ？号

type X4 = Record<string, number>

type X5 = Pick<Person, 'name'> // X5 的type为 { name: string; }

type X6 = Omit<Person, 'name'> // X6 的type为 { age: number; }

type X7 = Exclude<'a' | 'b' | 'c', 'a'> // X7 的type为 'b' | 'c'

type X8 = Extract<'a' | 'b' | 'c', 'a' | 'd'> // X8 的type为 'a'

type X9 = NonNullable<string | number | undefined> // X9 的type为 string | number

type X10 = Parameters<(a: number, b: string) => void> // X10 的type为 [number, string]

type X11 = ConstructorParameters<new (a: number, b: string) => void> // X11 的type为 [number, string]

type X12 = ReturnType<() => string> // X12 的type为 string 返回值类型

// 自己实现

type Readonly<T> = {
  readonly [K in keyof T]: T[K]
}

type Partial<T> = {
  [K in keyof T]?: T[K]
}

type Required<T> = {
  // 减号表示去除 去除参数的 ？号
  [K in keyof T]-?: T[K]
}

type Record<K extends keyof string | number | symbol, V> = {
  [key in K]: V
}

```
