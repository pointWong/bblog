---
title: javascript之执行栈
date: 2022-03-16 20:19:33
categories: [技术,web前端]
tags: [js,javascript,js执行栈]
---

##### 什么是执行栈？

要想搞清楚什么是执行栈，首先得了解什么是执行上下文。简单来讲，只要有js代码运行，它一定是运行在执行上下文中，也就是说执行上下文是js代码运行环境的抽象概念，它有三种类型：

全局执行上下文：浏览器中的全局对象就是 `window `对象，`this` 指向这个全局对象。ps：它只有一个。

函数执行上下文：函数被调用时创建的上下文，每次调用函数都会创建一个上下文。

eval函数执行上下文：eval函数的执行上下文，它只存在于eval函数中。ps：eval函数不推荐使用。

执行栈就是保存执行上下文的一种数据结构，他遵循LIFO原则，在js中，开始运行代码时，会创建一个全局执行上下文，压入执行栈中，紧接着每遇到一个函数就会创建一个函数执行上下文，继续压入执行栈。js引擎会执行栈顶的函数，执行完将其弹出栈，继续执行下一个在栈顶的函数，如此反复，直到执行栈为空。

看下面代码

```
console.log('1')
function a(){
	console.log('a')
}
function b(){
 a()
 console.log('b')
}
b()
console.log('2')
```

首先是创建全局执行上下文压入栈中并执行，输入‘’1“，然后遇到函数b被调用，此时创建函数执行上下文压入执行栈，执行b函数，遇到函数a被调用，又创建一个函数执行上下文压入栈中，执行a函数，输出‘a',函数a执行完毕，弹出栈，接着执行函数b,输出‘b’,函数b执行完毕，弹出栈，接下来就是全局上下文，输出2，执行完毕，栈为空。

说道这里，必须要补充一个概念：上下文周期

执行上下文分为三个阶段：创建阶段、执行阶段和回收阶段。

##### 创建阶段

就是函数被调用，但是其内部代码被执行之前，这个阶段主要：

1、确定this的指向。

2、创建词法环境LexicalEnvironment

3、创建变量环境VarialEnvironment

伪代码表示：

```
ExectionContext = {
	this:{},//this在代码执行是才被确认的值
	LexicalEnvironment:{},//词法环境
	VarialEnvironment:{}//变量环境
}
```

词法环境

词法环境由两个部分组成：

- 全局环境：是一个没有外部环境的词法环境，其外部环境引用为` null`，有一个全局对象，`this` 的值指向这个全局对象
- 函数环境：用户在函数中定义的变量被存储在环境记录中，包含了`arguments` 对象，外部环境的引用可以是全局环境，也可以是包含内部函数的外部函数环境

伪代码表示：

```
GlobalExectionContext = {  // 全局执行上下文
  LexicalEnvironment: {       // 词法环境
    EnvironmentRecord: {     // 环境记录
      Type: "Object",           // 全局环境
      // 标识符绑定在这里 
      outer: <null>           // 对外部环境的引用
  }  
}

FunctionExectionContext = { // 函数执行上下文
  LexicalEnvironment: {     // 词法环境
    EnvironmentRecord: {    // 环境记录
      Type: "Declarative",      // 函数环境
      // 标识符绑定在这里      // 对外部环境的引用
      outer: <Global or outer function environment reference>  
  }  
```

变量环境

变量环境也是一个词法环境，因此它具有上面定义的词法环境的所有属性

在 ES6 中，词法环境和变量环境的区别在于前者用于存储函数声明和变量（ `let` 和 `const` ）绑定，而后者仅用于存储变量（ `var` ）绑定。上下文创建阶段，会在代码中扫描变量和函数声明，然后将函数声明存储在环境中，但变量会被初始化为`undefined`(`var`声明的情况下)和保持`uninitialized`(未初始化状态)(使用`let`和`const`声明的情况下)。这也是var声明的变量和函数声明存在提升的实际原因。

##### 执行阶段

在这阶段，执行变量赋值、代码执行

如果 `Javascript` 引擎在源代码中声明的实际位置找不到变量的值，那么将为其分配 `undefined` 值

##### 回收阶段

出栈等待虚拟机回收释放占用的内存。

