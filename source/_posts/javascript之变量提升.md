---
title: javascript之变量提升和函数提升
date: 2022-02-16 01:51:24
categories: [技术,web前端,javascript]
tags: javascript变量提升
---
### 变量提升和函数提升是什么
按照字面的意思，感觉像是变量被提到了代码的最前面，但其实不是，js作为一门解释性语言，它是一段一段执行的，在编译阶段一些变量被提前放入到内存中，导致了"变量提升"。初次之外还有函数提升，函数提升优先于变量提升。需要注意的是，变量提升只是声明提升，提升后值为undefined。

变量提升的例子

```
console.log(a)//undefined
var a = '123'
console.log(a) //123
```

变量a的声明“提升”到顶部，此时还没赋值，所以打印出undefined。

函数提升

```
b() 
function b(){
	console.log('b')
}

```

上面的代码能够正常执行，说明存在函数提升，需要注意的是，var b = function(){}这种形式声明一个函数属于变量提升，下面的代码将会报错

```
b()
var b = function(){
	console.log('b')
}
```

无论是变量提升还是函数提升都跟js的执行机制有关，发生提升的原因会在"javascript之执行栈"这篇文章中提及。