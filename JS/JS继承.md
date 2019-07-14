# 继承

> OO语言都支持两种继承方式：接口继承和实现继承。接口继承只继承方法签名，实现继承继承实际的方法。
>
> [详细讲解](https://juejin.im/post/58f94c9bb123db411953691b#heading-10)

理解继承需要先理解三个概念：**构造函数**、**原型**以及**实例**。

构造函数、原型、实例的关系：每个构造函数都有一个原型对象，原型对象包括一个指向构造函数的指针、实例都包括一个指向原型内部的指针。

原型链有下面几个特点：

* 原型链的构成是一个类型的原型对象等于另一个类型的实例，从而构成原型链
* 原型链的根节点都是Object，其中包含很多默认方法
* 可以重写方法
* 不能通过字面量的方法定义新方法，这样会导致链的断裂

原型链的缺点：

* 共享
* 不能给超类型构造函数传递参数（借用构造函数可以传入参数，call/aplly）

所以一般不单独使用原型链

## 组合继承

借用原型链和构造函数的优点，让不同的实例既可以拥有自己的属性，又可以共用方法。

```JS
function Parent(name) {
    this.name = name;
}

Parent.prototype.sayName = function() {
    console.log('parent name:', this.name);
}
Parent.prototype.doSomething = function() {
    console.log('parent do something!');
}
function Child(name, parentName) {
    Parent.call(this, parentName);
    this.name = name;
}

Child.prototype = new Parent();
Child.prototype.constructor = Child;
Child.prototype.sayName = function() {
    console.log('child name:', this.name);
}

var child = new Child('son');
child.sayName();       // child name: son
child.doSomething();   // parent do something!
```

## 寄生组合式继承

```JS
function Parent(name) {
    this.name = name;
}
Parent.prototype.sayName = function() {
    console.log('parent name:', this.name);
}

function Child(name, parentName) {
    Parent.call(this, parentName);  
    this.name = name;
}
//////////////////////////////////////////////////
//原型继承
function create(proto) {
    function F(){}
    F.prototype = proto;
    return new F();
}
Child.prototype = create(Parent.prototype);
Child.prototype.constructor = Child;

//组合原型继承
function inheritPrototype(Parent, Child) {
    Child.prototype = Object.create(Parent.prototype);//修改
    Child.prototype.constructor = Child;
}
inheritPrototype(Parent, Child);
//////////////////////////////////////////////

Child.prototype.sayName = function() {
    console.log('child name:', this.name);
}

var parent = new Parent('father');
parent.sayName();    // parent name: father


var child = new Child('son', 'father');
child.sayName();     // child name: son
```

## ES6继承

```JS
class Parent {
    constructor(name) {
        this.name = name;
    }
    doSomething() {
    console.log('parent do something!');
    }
    sayName() {
    console.log('parent name:', this.name);
    }
}

class Child extends Parent {
    constructor(name, parentName) {
        super(parentName);
        this.name = name;
    }
    sayName() {
    console.log('child name:', this.name);
    }
}

const child = new Child('son', 'father');
child.sayName();            // child name: son
child.doSomething();        // parent do something!

const parent = new Parent('father');
parent.sayName();           // parent name: father
```
