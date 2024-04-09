---
title: typescript之装饰器
date: 2022-06-07 22:12:03
Categories: [typescript]
tags: [ts,typescript]
---

众所周知，typescript就是type + js，它是javascript的超级，不仅有类型约束，还实现了ECMAScript的一些新标准特性甚至一些处于草案阶段的特性，本文要讲到的装饰器就是其中之一。

##### 首先什么是装饰器？

装饰器是一种特殊类型的声明，它能够被附加到[类声明]，[方法]， [访问符]，[属性]或[参数]上。 装饰器使用 `@expression`这种形式，`expression`求值后必须为一个函数，它会在运行时被调用，被装饰的声明信息做为参数传入。

下面定义一个装饰器 @color

```
function color(target){
	//do something
	console.log('color decorator')
}
```

然后这样使用装饰器：

```
@color
function dofunc(){}
```

装饰器组合作用于同一个声明上时，有两种书写方式：

在一行上

```
@a @b xxx
```

多行

```
@a
@b
xxx
```

装饰器装饰不同对象，表现不一样

##### 类装饰器

类装饰器表达式会在运行时当作函数被调用，类的构造函数作为其唯一的参数。

下面定义一个类装饰器

```
function seal(constructor:Function){
	Object.seal(constructor);
	Object.seal(constructor.prototype)
}
```

这样使用

```
@seal
class hello {
	constructor(){
		// do sth
	}
}
```

##### 方法装饰器
(未完待续)
