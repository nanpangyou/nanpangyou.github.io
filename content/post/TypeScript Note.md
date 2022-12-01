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
