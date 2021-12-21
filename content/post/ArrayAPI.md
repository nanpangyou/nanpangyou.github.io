---
title: ArrayAPI

date: 2019-04-09 15:03:23
---

# Array 的常见 API，有哪些会修改原数组，哪些并不会修改原数组

开发过程中，尽量不要修改后端接口返回的原始数据，当后端返回的是数组时，借用原生的数组 API 很方便操作。
但是要注意，有些 API 是会修改原数组的

## 会修改原数组的 API

1. `pop()`
   pop() 删除数组最后一个元素，如果数组为空，则不改变数组，返回 undefined，改变原数组，**返回被删除的元素**

```
var oldArray = [1,2,3,4,5,6]
var delItem = oldArray.pop();
console.log(delItem);     // 6
console.log(oldArray);    // [1,2,3,4,5]
```

2. `push()`
   push() 向数组末尾添加一个或多个元素，改变原数组，**返回新数组的长度**

```
var oldArray = [1,2,3,4,5];
var newArrayLength = oldArray.push(7);
console.log(newArrayLength);   // 6
console.log(oldArray);     //[1,2,3,4,5,7]
```

3. `reverse()`
   reverse() 颠倒数组中元素的顺序，改变原数组，**返回该数组**

```
var oldArray = [1,2,3,4,5];
var newArray = oldArray.reverse();
console.log(newArray);   // [5,4,3,2,1]
console.log(oldArray);   // [5,4,3,2,1]
```

4. `shift()`
   shift() 把数组的第一个元素删除，若空数组，不进行任何操作，返回 undefined,改变原数组，**返回第一个元素的值**

```
var oldArray = [1,2,3,4,5]
var delItem = oldArray.shift();
console.log(oldArray);    // [2,3,4,5];
console.log(delItem);   // 1
```

5. `sort()`
   sort() 对数组元素进行排序，改变原数组，**返回该数组**

```
var oldArray = [2,3,1,4,5];
var newArray = oldArray.sort();
console.log(oldArray);  // [1,2,3,4,5]
console.log(newArray);  // [1,2,3,4,5]
```

6. `splice()`
   splice() 从数组中添加/删除项目，改变原数组，**返回被删除的元素**

```
var oldArray = [1,2,3,4,5,6]
var delItems = oldArray.splice(2,3);
console.log(oldArray);  // [1,2,6]
console.log(delItems);  // [3,4,5]
```

7. `unshift()`
   unshift() 向数组的开头添加一个或多个元素，改变原数组，**返回新数组的长度**

```
var oldArray = [1,2,3,4,5];
var newArrayLength = oldArray.unshift(7);
console.log(oldArray);  // [7,1,2,3,4,5]
console.log(newArrayLength);  //  6
```

## 不会修改原数组的 API

1. `concat()`
   concat()  连接两个或多个数组，并将新的数组返回，不改变原数组，**返回新的数组**

```
var firstArray = [1,2,3];
var secondArray = [4,5,6];
var newArray = firstArray.concat(secondArray);
console.log(firstArray);  // [1,2,3]
console.log(secondArray);  // [4,5,6]
console.log(newArray);  // [1,2,3,4,5,6]
```

2. `join()`
   join() 把数组中所有元素放入一个字符串，将数组转换为字符串，不改变原数组，**返回字符串**

```
var oldArray = ["a","b","c","d"];
var newString = oldArray.join("-");
console.log(oldArray);  // ["a","b","c","d"]
console.log(newString);  // "a-b-c-d"
```

3. `slice()`
   slice()  从已有的数组中返回选定的元素，提取部分元素，放到新数组中，参数解释：1：截取开始的位置的索引，包含开始索引；2：截取结束的位置的索引，不包含结束索引。不改变原数组，**返回一个新数组**

```
var oldArray = [1,2,3,4,5,6]
var newArray = oldArray.slice(2,4);
console.log(oldArray);   // [1,2,3,4,5,6]
console.log(newArray);   // [3,4]
```

4. `toString()`
   toString() 把数组转为字符串，不改变原数组，**返回数组的字符串形式**

```
var oldArray = [1,2,3,4,5];
var newString = oldArray.toString();
console.log(oldArray)  // [1,2,3,4,5]
console.log(newString)  // "1,2,3,4,5"
```

**多多总结，多多避坑**

![啾咪~~](timg.jpg)
