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
