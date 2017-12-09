---
title: JS原型与原型链
date: 2017-10-30 16:56:56
category: 总结
tags: JS
---

#### 问题的引入


用控制台输入如下命令

```
$ var arr = [];
$ arr.length // 0
```
问题：为什么arr会有length属性？

<!--more-->

#### 1. 显示原型prototype

* JavaScript中每一个函数，都有一个prototype属性， 这个属性指向函数的原型对象


##### 1.1 自定义函数的prototype
```
var fn = function() {}
console.log( fn.prototype );
fn.prototype.constructor === fn;   // true
```

![](https://user-gold-cdn.xitu.io/2017/11/12/4af86fc38f20ab3e1096f05f7e0e0db9)

##### 1.2 内置构造器的prototype

Array是什么？
答：Array是一个函数


![](https://user-gold-cdn.xitu.io/2017/11/12/603bed777f274b1b84c6f4c8926b4339)

那么Array一定也有一个prototype属性了


![](https://user-gold-cdn.xitu.io/2017/11/13/15fb4ff1bef7bc3e)

```
$ Array.prototype.constructor // f Array()
$ Array.prototype.constructor === Array // true
$ Number.prototype.constructor === Number // true
$ String.prototype.constructor === String // true
$ Function.prototype.constructor === Function // true
$ Object.prototype.constructor === Object // true
```
##### 1.3 改变prototype

![](https://user-gold-cdn.xitu.io/2017/11/13/15fb5618a38b850a)

结论：
* 给prototype添加或者删除属性会影响所有已经或者未实例化的对象
* 重写prototype只会影响新new出来的对象


#### 2. 隐式原型\__proto__
* JavaScript中任意对象都有一个内置属性__proto__， 指向创建这个对象的函数(constructor)的prototype

```
var arr = new Array();
arr.__ptoto__ === Array.prototype // true
```
根据"JavaScript中任意对象都有一个内置属性\__proto\__， 指向创建这个对象的函数(constructor)的prototype"，函数Array创建了对象arr，因此arr的\__proto\__指向Array.prototype。即

![](https://user-gold-cdn.xitu.io/2017/11/13/15fb4feb2eb4e810)

* 此处提一下Object.create(),Object.create接收两个参数，第一个参数用来作为新对象的隐式原型, 第二个参数是对象的属性描述符。因此
```
var person = {LEG_NUM:2 };
var xiaoMing = Object.create(person);
xiaoMing.__proto__ === person; // true
```
##### 2.1 改变\__proto\__

![](https://user-gold-cdn.xitu.io/2017/11/13/15fb5628e12d0785)

结论
* 改变arr.__proto__等价于改变Array.prototype



#### 3. 原型链
* 当我们访问某个对象中的某个属性时，如果该对象本身具有这个属性，则直接使用，如果该对象本身没有这个属性，那么就会沿着\__proto\__依次查找

![](https://user-gold-cdn.xitu.io/2017/11/13/15fb5576c544462e)

上图中，访问obj.z的时候在obj中没有访问到，就会沿着\__proto\__向上查找，于是在foo.prototype中找到了返回3.

访问obj.hasOwnProperty()时，同样在obj中没有找到这个属性，沿着\__proto\__向上查找，直到在Object.prototype中找到这个属性。

#### 4. instanceof

instanceof, 左边是一个对象，右边必须是一个函数或构造器。它会判断右边构造器的prototype属性是否出现在左边对象的原型链上。


```
[] instanceof Array // true
[] instanceof Object // true
```

![](https://user-gold-cdn.xitu.io/2017/11/13/15fb55bf5ff07694)

#### 5. 原型链的优缺点

* 优点: 节省内存、方便维护
* 缺点: 更改原型将影响所有指向它的对象
以下是《Javascript高级程序设计》中使用到的例子

![](https://user-gold-cdn.xitu.io/2017/11/13/15fb55df5e74932a)

改变person1的friends，会同时改变person2的friends，因为它们共用一个prototype.这显然不是我们想要的。可以通过如下方法改进

![](https://user-gold-cdn.xitu.io/2017/11/13/15fb55f9e4dab238)

#### 小结

* 显式原型的作用
用来实现基于原型的继承与属性的共享。
* 隐式原型的作用
构成原型链，同样用于实现基于原型的继承。举个例子，当我们访问obj这个对象中的x属性时，如果在obj中找不到，那么就会沿着\__proto\__依次查找。
* 二者的关系
隐式原型指向创建这个对象的函数(constructor)的prototype


#### 参考文献

* https://www.zhihu.com/question/34183746
* https://www.kancloud.cn/wangfupeng/zepto-design-srouce/173684
* 《Javascript高级程序设计》(第3版)第6章
