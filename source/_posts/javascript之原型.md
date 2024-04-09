---
title: javascript之原型
date: 2021-06-10 22:24:44
categories: [技术,web前端,javascript]
tags: [javascript,js]
---
es5中没有类的概念，他的继承是基于原型的，每个函数都有一个prototype属性，指向函数的原型对象。每一个对象有关联一个原型对象，用__proto__属性可以访问到这个原型对象。
```
function Person(name,gender){
    this.name =name
    this.gender = gender
}
const Brian = new Person('Brian','male')
Person.prototype.getName = function(){
    return this.name
}
Person.prototype.country = "cn"
console.log(Brian.__proto__==Person.prototype) //true
console.log(Brian.getName()) //Brian
console.log(Brian.country) // cn
```
从上面的例子中可以看出，实例的__proto__和 构造函数的prototype属性指向的是同一个对象，也就是原型，实例会继承原型上的属性和方法，country和getName就是继承自原型。

构造函数、原型和实例之间的关系就是：每个函数都有一个原型对象(prototype)，原型对象都包含这一个指向构造函数的指针(constructor),而实例都包含一个指向原型对象的内部指针(proto)。假如我们让原型对象等于另一个类型的实例，这时的原型对象将包含一个指向另一个原型的指针，同样另一个原型中也包含着一个指向另一个构造函数的指针。如果另一个原型又是另一个类型的实例，这样上面描述的关系仍会成立，这样层层递进，构成了实例与原型的链条，就是原型链。
```
function Cat(){}
Cat.prototype.constructor == Cat //true
```
原型中包含一个指向构造函数的指针，也就是说构造函数的原型的构造函数指向其自身，因此，可以把原型看成构造函数的一个特殊实例，其他实例"共享"这个特殊实例的属性和方法。看下面代码
```
function Animal(){}
let cat = new Animal() //普通实例
Animal.prototype.name = 'cat' //把原型看成Animal的一个特殊实例
Animal.prototype.getName = function(){
    return this.name
}
console.log(cat.name) //cat
console.log(cat.getName()) //cat
```

可以看到，就算name和getName是在实例化得到cat后定义的，cat依旧可以访问到他们的，这也说明，实例“共享”原型的属性和方法。

在js中，一切皆对象，由Function创建的是函数对象(包括Function本身)，由Object创建的是普通对象。既然都是对象，除了普通对象外，函数对象也会有一个指向其构造函数原型对象的指针，也就是所有函数的__proto__属性都指向Function.prototype,而Function.prototype是个普通对象，所以Function.prototype.__proto__指向Object.prototype,所有对象都有一个指针指向原型对象，但是由于Object.prototype已经到了最顶端，所以Object.prototype.__proto__ 指向null。

会发现，除Object本身外，其他内置函数的原型都是由Object创建，这其中除了Function.prototype是函数对象外，其余的都是普通对象。
```
Object.prototype instanceof Object //false
typeof Object.prototype == 'object' // true
typeof Function.prototype == 'function' // true
typeof Array.prototype == 'object' //true

```
为什么Object.prototype.constructor是Object，说明Object.prototype是由Object创建的，但是，为什么Object.prototype instanceof Object 的结果却是false呢？原因是instanceof是通过查找原型链判断，由于Object.prototype已经到达最顶端，它的__proto__属性指向null。
然后会有一些有趣的现象
```
Function.constructor == Function // true
Function.__proto__ == Function.prototype //true
Object instanceof Object // true 
Function instanceof Function //true
Function instanceof Object //true
Object instanceof Function //true
```
可以看出Function和Object，我中有你，你中有我，却最终又指向一个null。这是js这门语言的奇妙之处。

[参考:https://www.jianshu.com/p/08c07a953fa0](https://www.jianshu.com/p/08c07a953fa0)