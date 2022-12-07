---
title: "TypeScript Note"
date: 2022-12-01T23:00:12+08:00
---

## 使用类型谓词做类型收窄

```ts
type Rect = {
 height: number;
 width: number;
}

type Circle = {
 point: [number,number];
 radius: number;
}

const f1 = (a: Rect |Cricle) => {
 if(isRect(a)){
  a // 这里a的类型就是Rect
 }else if(isCircle(a)){
  a // 这里a的类型就是Circle
 }
}

// 使用类型谓词做类型收窄 这里要使用function关键字，不能使用箭头函数
function isRect(x: Rect | Circle): x is Rect {
 return 'height' in x && 'width' in x;
}

function isCricle(x: Rect | Circle): x is Circle {
 return 'point' in x && 'radius' in x;
}

```

## 可辨别联合（ kind ）

```ts

// 可辨别联合 它们都有一个kind属性  kind也可以用其他任意key代替，如 type ， tag ， category 等
type Circle = { kind: 'circle', point: [number, number], radius: number };
type Square = { kind: 'square', x: number};
type Shape = Circle | Square;

const f1 = (a: Shape) =>{
 if(a.kind === 'circle'){
  a // 这里a的类型就是Circle
 }else if(a.kind === 'square'){
  a // 这里a的类型就是Square
 }
}

// 使用同名，可辨别的简单类型的key做区分


```

## 交叉类型

### 可以使用```&```来实现交叉类型

```ts

type A = {
 name: string;
}

type B = {
 age: number;
}

type C = A & B;


const c:C = {
 name: 'xxx',
 age: 18
}

```

如果 A、B 无交集，则可能得到 never 也可能是属性 never

#### 记录一个犯的错

```ts

type A = {name: string} // 代表对象拥有一个属性为name，并且该属性的值为string，（并不是只能有一个name属性）

type B = {age: number} // 不是指对象只能有一个属性age，而是指对象拥有一个属性为age，并且该属性的值为number 

type C = A | B

const d = {name : 'xxx', gender: 'male'}

const c: C = d //这样赋值是不会报错的

const c：C = {name: 'xxx', gender: 'male'}  //这样赋值会报错

// ts 只在声明同时赋值的时候会严格检查类型，而在赋值的时候不会严格检查类型

```

## 类型兼容

小类型可以复制给大类型

```ts

// 简单类型
type A = string | number;
const a: A = 'hi'


// 简单对象
type Person = { name: string; age: number; }

const student = { name: 'Tom', age: 18, grade: 3 };
const aa: Person = student; // ok


```

```ts

// 获取一个没有声明对象类型的类型
const user = { name: 'Tom', age: 18, grade: 3 };
type User = typeof user;

```

## 指定this

```ts

type Person = { name: string }

function f(this: Person, word: string){
  console.log(this.name + word)
}

// 1. 拼凑 person.f()
const person: Person & {F: typeof f} = { name: 'xxx', F: f }
person.f('hi')

// 2. call
f.call(person, 'hi')

// 3. apply
f.apply(person, ['hi'])

// 4. bind
const f1 = f.bind(person)
f1('hi')

// =====================
f.bind(person)('hi')

// =====================
const f1 = f.bind(person)  // this => person
const f2 = f1.bind(null, 'hi') // word => 'hi'
f2()
```

## as const

在开发过程中，有时需要ts将类型自动推导变小

```ts

const a = [1,3,4]
// 这里的 a 的类型可以是 number[] 也可以是 readonly [1,2,3], 逻辑都是可以说过去的
// 一般没有明确说明时，a 的类型是 number[]
const b = [1,3,4] as const
// 这里的 b 的类型就是 readonly [1,2,3]，这样就可以保证 b 的值不会被修改
// 如果使用了 as const 那么 b.push(5) 就会报错, 因为 b 的类型是 readonly [1,2,3]
// 如果不使用 as const 那么 b.push(5) 就不会报错, 因为 b 的类型是 number[]




// 使用场景

function f(a: number, b: number, c: number){
  console.log(a + b + c)
}
const a = [1,2,3]
f(...a) // 由于 a 的类型是 number[]，所以这里会报错，因为 f 的参数类型是 number, number, number，不能保证数组内容不可变

const a = [1,2,3] as const
f(...a) // 这里就不会报错了，因为 a 的类型是 readonly [1,2,3]，这样就可以保证数组内容不可变

```

## 函数参数析构

```ts

type Config = {
  url: string
  method: 'get' | 'post'
  data?: unknown
  params?: unknown
}


// 普通析构
const ajax = ({ url, method }: Config) => {}

// 剩余参数析构
const ajax = ({ url, method, ...rest}: Config) => {}

// 默认值
const ajax = ({ url, method, ...rest}: Config = { url: '', method: 'get' }) => {}

// 默认值的另一种写法
const ajax = ({ url, method, ...rest} = { url: '', method: 'get' } as Config) => {}
```

## 索引签名 (Index Signature) 和 映射类型 (Mapped Types)

```ts

## 索引签名

type Person = {
  [k: string]: string
  length: number
}

type Person2 = {
  [k: number]: string
  length: number
}

## 映射类型 (常用于范型)

type Person = {
  [k in string]: string
  length: number
}

type Person2 = {
  [k in number]: string
  length: number
}

```
