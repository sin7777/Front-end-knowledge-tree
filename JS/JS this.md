# this 的指向问题

>[解释以及面试题](https://segmentfault.com/a/1190000011194676#articleHeader6) *没有看完*

this 指向函数**运行**时所在的环境对象

可以改变this指向：

* new（new不太好说，别回答这个）
* call()
* aplly()
* bind()

## this 优先级

this 有 4 种绑定方式

* 默认绑定
* 隐式绑定
* 显式绑定
* new绑定

优先级：

显式绑定 > 隐式绑定 > 默认绑定

new绑定 > 隐式绑定 > 默认绑定

### 默认绑定

在非严格模式下，默认绑定的this指向全局对象，严格模式下this指向undefined

### 隐式绑定

函数在调用位置，是否有上下文对象，如果有，那么this就会隐式绑定到这个对象上

```JS
function foo() {
  console.log(this.a);
}
var a = "Oops, global";
let obj2 = {
  a: 2,
  foo: foo
};
let obj1 = {
  a: 22,
  obj2: obj2
};
obj2.foo(); // 2 this指向调用函数的对象
obj1.obj2.foo(); // 2 this指向最后一层调用函数的对象
// 隐式绑定丢失
let bar = obj2.foo; // bar只是一个函数别名 是obj2.foo的一个引用
bar(); // "Oops, global" - 指向全局
```

隐式绑定丢失的问题：实际上就是函数调用时，并没有上下文对象，只是对函数的引用，所以会导致隐式绑定丢失。

### 显式绑定

我们可以通过 apply、call、bind 将函数中的 this 绑定到指定对象上。

### new 绑定

1. 创建一个全新的对象。
2. 这个新对象会被执行 [[Prototype]] 连接。
3. 这个新对象会绑定到函数调用的this。
4. 如果函数没有返回其他对象，那么new表达式中的函数调用会自动返回这个新对象。

规则：使用构造调用的时候，this会自动绑定在new期间创建的对象上。

```JS
function foo(a) {
  this.a = a; // this绑定到bar上
}
let bar = new foo(2);
console.log(bar.a); // 2
```

## call() aplly() bind() 三者区别

> [概念和常用地方](https://segmentfault.com/a/1190000011389726)

不同之处：

* 语法：call接受两个参数、aplly接受多个参数、bind一个参数
* 调用：call() aplly()立即调用，bind()只绑定，不调用

常用来：

* 处理伪数组（借用数组方法）
* 实现继承
* 合并数组
