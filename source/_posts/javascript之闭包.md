---
title: javascript之闭包
date: 2021-04-09 22:55:36
categories: [技术,web前端]
tags: javascript
---
javascript中闭包是一个很重要的概念，它有三个特点：
- 函数嵌套函数
- 函数内部可以引用外部的参数和变量
- 参数和变量不会被垃圾回收机制回收

闭包一般用来设计私有的方法和变量
```
function aaa() {  	
    var a = 1;  		
    return function(){		
        alert(a++)	
    };
}
var fun = aaa();
fun();// 1 执行后 a++，，然后a会驻留在内存中
fun();// 2   
fun = null;//a被回收！！
```
闭包的好处是，使得一个变量长期驻留在内存，避免全局变量的污染。但是，正是由于常驻内存，会增大内存使用量，处理不当就会造成内存泄漏。

看下面这段代码
```
function add () {  
    var test = 0;  
    return function () {    
        test++;    
        return test;  
    }
}
var test = add();
console.log(test()); // 1
console.log(test()); // 2
console.log(test()); // 3
console.log(test()); // 4
console.log(test()); // 5

```
闭包保持对test的引用，除非手动清除，否则它会常驻内存。
