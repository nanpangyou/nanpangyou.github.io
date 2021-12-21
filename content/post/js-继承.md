---
title: js-继承
categories:
  - Note
  - code
tags:
  - blog
  - code
date: 2019-09-23 15:25:24
---

# 使用 JavaScript 实现继承

要求：
要求不使用 class，完成如下需求：

- 写出一个构造函数 Animal
  输入为空
  输出为一个新对象，该对象的共有属性为 {行动: function(){}}，没有自有属性

- 再写出一个构造函数 Human
  Human 继承 Animal
  输入为一个对象，如 {name: 'xxx', birthday: '2000-10-10'}
  输出为一个新对象，该对象自有的属性有 name 和 birthday，共有的属性有物种（人类）、行动和使用工具

- 再写出一个构造函数 Asian
  Asian 继承 Human
  输入为一个对象，如 {city: '北京', name: 'xxx', birthday: '2000-10-10' }
  输出为一个新对象，改对象自有的属性有 name city 和 bitrhday，共有的属性有物种、行动和使用工具和肤色
  既

最后一个新对象是 Asian 构造出来的

```
  Asian 继承 Human，Human 继承 Animal
```

使用原型链

```
function Animal(){

}
Animal.prototype.行动 = function(){}

function Human(options){
	this.name = options.name;
	this.birthday = options.birthday
}
// 方法一
// 通过Object.create 将Human.prototype.__proto__指向Animal.prototype
Human.prototype = Object.create(Animal.prototype)
Human.prototype.物种 = '人类'
Human.prototype.行动 = function(){}
Human.prototype.使用工具	= function(){}

function Asian(options){
	Human.call(this,options);
	this.city = options.city;
}
// 方法二
// 首先 new Human() 执行了三句隐藏的代码
// this = {}
// this.__proto__ = Human.prototype
// retrun this
// 所以我们可以借助new 将子函数的prototpye下的__proto__(也就是共有属性)指向 new 的这个函数的prototype
// 但是，当我们的父函数有自有属性时，不能直接new Human()
// 需要借助下面的代码。搭桥，将子函数的prototype.__proto__ 指向 父函数的 prototype

function fakeHuman(){}
fakeHuman.prototype = Human.prototype
Asian.prototype = new fakeHuman()

// 或者用地下的这句直接就可以了
// Asian.prototype = Object.create(Human.prototype)
```

使用 class 关键字

```
class Animal{
	行动(){}
}

class Human extends Animal{
	constructor(options){
		super(options)
		this.name = options.name;
		this.birthday = options.birthday
		this.物种 = '人类'
	}

	行动(){}
	使用工具(){}
}


class Asian extends Human{
	constructor(options){
		super(options)
		this.city = options.city
	}
}
```

class 方式无法让非函数称为共有属性，只能写入静态属性
