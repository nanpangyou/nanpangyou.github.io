---
title: "TS-体操"
date: 2022-12-26T16:05:44+08:00
---

## 一些TS类型体操

### 体操的基本原理

```ts

// 判断一个数组是否为空数组
type A = []
type isEmptyArray<Arr extends unknown[]> = Arr['length'] extends 0 ? true : false

type Result = isEmptyArray<A> // true

```

```ts

// 判断一个元组是否为空
type A = [1]
type NotEmpty<Arr extends unknown[]> = Arr extends [...unknown[], unknown] ? true : false

type Result = NotEmpty<A> // true


type B = [1]
type NotEmpty<Arr extends unknown[]> = Arr extends [...infer Rest, infer Last] ? true : false

type Result = NotEmpty<B> // true


```

```ts

// 反转一个元组
type A  = [1,2,3,4]
type Reverse<Arr extends unknown[]> = Arr extends [...infer Rest, infer Last] ? [Last, ...Reverse<X>] : Arr

type Result = Reverse<A> // [4,3,2,1]

```

```ts

// 模式匹配

type Tuple = ['ji', 'ni', 'tai', 'mei']

type Result1 = Tuple extends [First, ...infer Rest] ? First : never // Result1  is  'ji'

type Result2 = Tuple extends [First, ...infer Rest] ? Rest : never // Result2 is  ['ni', 'tai', 'mei']

```

### 元组的体操

```ts

// 拓展一个元组（例如： 将二元组拓展为三元组）

type Tuple = [1,2]
type ExtendTuple = [ ...Tuple, number | string ]


type A = [1, 2]
type B = [3, 4]
type C = [...A, ...B] // [1, 2, 3, 4]


// 获取元组的最后一项

type Tuple = [1,2,3,4]
type Last<Arr> = Arr extends [...unknown[], infer Last] ? Last : never

type Resulte = Last<Tuple> // 4

// 或者
type Tuple = [1,2,3,4]
// 如果变量取名则所有都需要取名
type Last<Arr> = Arr extends [...items: unknown[], last: infer Last] ? Last : never

type Resulte = Last<Tuple> // 4

```

### 字符串体操

```ts
// 首字母大写

type A = 'hello'
// Capitalize 是 TS 内置的类型
type Result = Capitalize<A> // 'Hello'


type B = 'ji' | 'ni' | 'tai' | 'mei'
// 联合类型使用泛型要每一个都使用
type Result = Capitalize<B> // 'Ji' | 'Ni' | 'Tai' | 'Mei'

// 内置类型还有
type Result1 = Uppercase<B> // 'JI' | 'NI' | 'TAI' | 'MEI'
type Result2 = Lowercase<B> // 'ji' | 'ni' | 'tai' | 'mei'
type Result3 = Uncapitalize<B> // 'ji' | 'ni' | 'tai' | 'mei'
type Result4 = Capitalize<B> // 'Ji' | 'Ni' | 'Tai' | 'Mei'

// 联合类型是没有顺序的 
// Lowercase之后有可能就变成  'jI' | 'ni' | 'tai' | 'mei' => 'ni' | 'tai' | 'mei' | 'ji'

```

```ts

// 字符串链接
type A = 'ji'
type B = 'ni'
type C = 'tai'
type D = 'mei'

type Result = `${A}-${B} ${C} ${D}` // 'ji-ni tai mei'


// 字符串拆分的模式匹配
type A = 'ji ni tai mei'
type First<T extends string> = T extends `${infer First}${infer Rest}` ? First : T

type FisrtWord<T extends string> = T extends `${infer First} ${infer Rest}` ? First : T

type Result = First<A> // 'j'
type Result1 = FisrtWord<A> // 'ji'

// `${infer First}${infer Rest}` 会将字符串拆分为两部分，第一部分是第一个字符，第二部分是剩余的字符串
// 'abcd' => 'a' 
// '' => ''

```

```ts

// 获取字符串的最后一项

// 思路： 先将字符串转为元组，然后使用元组获取最后一项的方法获取字符串的最后一项

type GetLastOfTuple<T extends unknown[]> = T extends [...unknown[], infer Last] ? Last : never

type StringToTuple<T extends string> = T extends `${infer First}${infer Rest}` ? [First, ...StringToTuple<Rest>] : []

type A = 'ji ni tai mei'

type Result = GetLastOfTuple<StringToTuple<A>> // 'i'

```

```ts

// 字符串变联合类型
type A = 'ji ni tai mei'
type B = 'jinitaimei'
type StringToUnion<T extends String> = T extends `${infer First}${infer Rest}` ? First | StringToUnion<Rest> : never

type Result = StringToUnion<A> // "j" | "i" | " " | "n" | "t" | "a" | "m" | "e"
type Result1 = StringToUnion<B> // "j" | "i" | "n" | "t" | "a" | "m" | "e"
// 联合类型会自动去重

```

```ts

// 字符串变元组
type A = 'ji ni tai mei'
type StringToTuple<T extends string> = T extends `${infer First}${infer Rest}` ? [First, ...StringToTuple<Rest>] : []
type StringToTupleWithSpace<T extends string> = T extends `${infer First} ${infer Rest}` ? [First, ...StringToTupleWithSpace<Rest>] : [T]

type Result = StringToTuple<A> // ["j", "i", " ", "n", "i", " ", "t", "a", "i", " ", "m", "e", "i"]
type Result1 = StringToTupleWithSpace<A> //  ["ji", "ni", "tai", "mei"]

```

```ts
```
