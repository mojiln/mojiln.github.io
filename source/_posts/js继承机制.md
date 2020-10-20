---
title: js 的继承机制
date: 2019-01-19 19:10:02
categories:
  - 面试题
tags:
  - 面试题
---

# js 的继承机制

<!-- more -->

## ES5 和 ES6 及继承机制

## ES5 继承机制

在 js 万物皆对象，但事还是区分普通对象和函数对象，只有函数对象才有 `prototype` 属性，但所有对象都有`__proto__`属性

```js
function A() {}
var B = new A()
```

那这里就得出几个公式：

```js
B.**proto**== A.prototype;
B.constructor == A;
a.prototype.constuctor = A;
```

那这是实现继承的基础； 也就是说 A.prototype 是 A 的原型对象，A 是构造函数，B 是 A 的实例，原型对象（A.prototype）是 构造函数（A）的一个实例。而此时 this 指向是指向实例。 明白这个关系实现继承就很简单了！看一下下面的代码

### 原型链继承

通过重写子类的原型 等于 父类的一个实例，（父类的实例属相变成子类的原型属性）

```js
function father() {
  this.faName = 'father'
}
father.prototype.getfaName = function() {
  console.log(this.faName)
}
function child() {
  this.chName = 'child'
}
child.prototype = new father()
child.prototype.constructor = child
child.prototype.getchName = function() {
  console.log(this.chName)
}
```

### 组合继承 （比较常用，也有缺点）

通过子类的原型对象指向父类实例的方式来实现继承，那我们不难发现原型链的形成是真正是靠`__proto__` 而非 prototype，子类的实例可以访问父类原型上的方法，是因为子类实例通过`__proto__`与父类的原型对象有连接

```js
//先来个父类，带些属性
function Super() {
  this.flag = true
}
//为了提高复用性，方法绑定在父类原型属性上
Super.prototype.getFlag = function() {
  return this.flag
}
//来个子类
function Sub() {
  this.subFlag = false
}
//实现继承
Sub.prototype = new Super()
//给子类添加子类特有的方法，注意顺序要在继承之后
Sub.prototype.getSubFlag = function() {
  return this.subFlag
}
//构造实例
var es5 = new Sub()
```

### 寄生组合继承 （业内比较提倡的方法）

所谓寄生组合式继承，即通过借助构造函数来继承属性，通过原型链的混成形式来继承方法。 其背后的基本思路是：不必为了指定子类型的原型而调用超类型的构造函数，我们所需要的无非就是超类型原型的一个副本而已。本质上，就是使用寄生式继承来继承超类型的原型，然后再将结果指定给子类型的原型。

```js
function inserit(son, father) {
  var obj = Object.create(father.prototype)
  son.prototype = obj
  obj.constructor = son
}

function SuperType(name, colors) {
  this.name = name
  this.colors = colors
}
SuperType.prototype.sayName = function() {
  return this.name
}

function SubType(job, name, color) {
  SuperType.call(this, name, color)
  this.job = job
}
//核心方法
inserit(SubType, SuperType)
SubType.prototype.sayjob = function() {
  return this.job
}
var instance = new SubType('doctor', 'John', ['red', 'green'])
console.log(instance.sayjob(), instance.sayName()) //doctor,John
```

[x] 原型链继承缺点：父类包含引用类型的属性，那么子类所有实例都会共享该属性（包含引用类型的原型属性会被实例共享），在创建子类实例时，不能向父类的构造函数传递参数
[x] 组合继承的缺点：两次调用父类构造函数：（第一次是在创建子类原型的时候，第二次是在子类构造函数内部）子类继承父类的属性，一组在子类实例上，一组在子类原型上（在子类原型上创建不必要的多余的属性）（实例上的屏蔽原型上的同名属性）效率低
[x] 组合继承的优点：只调用一次父类的构造函数,避免了在子类原型上创建不必要的，多余的属性，原型链保持不变
ES5 的继承机制总结
ES5 的继承机制简单来说就是：实质是先创造子类的实例对象 this，然后再将父类的方法添加到 this 上面（Parent.apply(this)）

## ES6 的继承机制

```js
class Point {
  constructor(x) {
    this.x = 1
    this.p = 2
  }
  print() {
    return this.x
  }
}
Point.prototype.z = '4'
class ColorPoint extends Point {
  constructor(x) {
    this.color = color // ReferenceError
    super(x, y)
    this.x = x // 正确
  }
  m() {
    super.print()
  }
}
```

ES6 继承是通过 **class**、**extends** 关键字来实现继承 Point 是父类，ColorPoint 是子类 通过 class 新建子类 extends 继承父类的方式实现继承，方式比 ES5 简单的多。

### constructor

constructor 方法是类的构造函数，是一个默认方法，通过 new 命令创建对象实例时，自动调用该方法。一个类必须有 constructor 方法，如果没有显式定义，一个默认的 consructor 方法会被默认添加。所以即使你没有添加构造函数，也是会有一个默认的构造函数的。一般 constructor 方法返回实例对象 this ，但是也可以指定 constructor 方法返回一个全新的对象，让返回的实例对象不是该类的实例。

```js
class Points {
constructor(x) {
this.x = 1;
this.p = 2;
}
print() {
return this.x;
}
statc getname(){
return new.target.name;
}
}
// 等同于：
function Points(x) {
this.x = x;
this.p = 2;
}
Points.prototype.print = function() {
return '(' + this.x +')';
}
```

也就是说 constructor 就代表在父类上加属性，而在 class 对象加方法属性 等于在原型上的加。而这些属性方法 只有通过 new 出的实例 或者是 extends 继承出来的实例才可以获取到 所以我们可以得到

```js
new Points().__proto__.print() //可以调用到 Points 的 print 方法
new Points().x = 1 //可以调用到 constructor 里面的 this.x=1
```

### super

super 既可以当做函数使用，也可以当做对象使用两种使用的时候完全不一样， 函数用时 : 在 constructor 中必须调用 super 方法，因为子类没有自己的 this 对象，而是继承父类的 this 对象，然后对其进行加工,而 super 就代表了父类的构造函数。super 虽然代表了父类 A 的构造函数，但是返回的是子类 B 的实例，即 super 内部的 this 指的是 B，因此 super() 在这里相当于

```js
A.prototype.constructor.call(this, props)
```

在 super() 执行时，它指向的是 子类 B 的构造函数，而不是父类 A 的构造函数。也就是说，super() 内部的 this 指向的是 B。所以在第一个 es6 的例子中子类的 this 指向的是自己。、

**当做对象使用** ： 在普通方法中，指向父类的原型对象；在静态方法中，指向父类。 所以在子类的方法中 super.print();指向的是父类原型上的方法。 **但是因为 super 的两种用法，所以 es6 规定在使用必须要明确使用方式，像单独 console.log(super) 就会报错。**

### static

顾名思义是静态方法的意思，类相当于实例的原型， 所有在类中定义的方法， 都会被实例继承。 如果在一个方法前， 加上 static 关键字， 就表示该方法不会被实例继承， 而是直接通过类来调用， 这就称为“ 静态方法”。静态方法调用直接在类上进行，而在类的实例上不可被调用。静态方法通常用于创建 实用/工具 函数。

### new.target

new.target 属性允许你检测函数或构造方法是否通过是通过 new 运算符被调用的。在通过 new 运算符被初始化的函数或构造方法中，new.target 返回一个指向构造方法或函数的引用。在普通的函数调用中，new.target 的值是 undefined。

也就是说 new.target 的功能就是用来检测函数的调用是不是通过 new 去创建一个新对象的，而且 new.target 返回的是一个指向函数的引用，也就是说我们能够确定是哪个函数进行了 new 操作。

## ES6 的继承机制总结

先创建父类实例 this 通过 **class**、 **extends** 、**super** 关键字定义子类，并改变 this 指向,super 本身是指向父类的构造函数但做函数调用后返回的是子类的实例，实际上做了父类.prototype.constructor.call(this)，做对象调用时指向父类.prototype,从而实现继承。
