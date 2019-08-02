# 闭包与作用域链

> 关键词：执行环境、作用域链、活动对象、立即执行函数、匿名函数

(JS是静态作用域，函数的作用域在定义的时候已经确定，而动态作用域树函数执行的时候创建)

闭包的定义

闭包是指有权访问另外函数作用域中的变量，是由`[[scope]]`作用域链所带来。执行上下文被销毁，但是AO仍存在于作用域链

* 从理论角度：所有的函数。因为它们都在创建的时候就将上层上下文的数据保存起来了。哪怕是简单的全局变量也是如此，因为函数中访问全局变量就相当于是在访问自由变量，这个时候使用最外层的作用域。
* 从实践角度：以下函数才算是闭包：
即使创建它的上下文已经销毁，它仍然存在（比如，内部函数从父函数中返回）
在代码中引用了自由变量

当函数被调用时，会创建一个执行环境以及相应的作用域链(包括GO和AO)

调用函数 --> 创建执行环境 --> 复制`[[scope]]`作用域链 && 创建活动对象AO，将AO推入作用域链的顶端

AO的完成有4个步骤

```JS
function fun(a, b, c){
  /*
    step 1 创建AO对象
    AO = {}

    step 2 初始化AO对象
    AO = {
      this: undefined,
      arguments: undefined,
      a: undefined,
      b: undefined,
      c: undefined,
      inner: undefined
    }

    step 3 变量赋值
    AO = {
      this: window,
      arguments: [0:1, 1:2, length: 2],
      a: 1,
      b: 2,
      c: undefined,
      inner: undefined
    }

    step 4 函数赋值
    AO = {
      this: window,
      arguments: [0:1, 1:2, length: 2],
      a: function a(){},
      b: 2,
      c: undefined,
      inner: undefined
    }

  */
  var inner = 10
  function a(){
    //todo
  }
}

var global = 20
fun(1,2)  //调用函数
```

闭包的使用：

* 封装对象的私有属性和方法：闭包通常用来创建内部变量，使得这些变量不能被外部随意修改，同时又可以通过指定的函数接口来操作
* 保护函数内的变量安全：如迭代器、生成器（ES6）
* 在内存中维持变量：如果缓存数据、柯里化（高阶函数的降阶处理）

```JS
//非闭包
var data = []
for(var k = 0; k < 3; k++){
  data[k] = function(){
    console.log(k)
  }
}
data[0](); //3
data[1](); //3
data[2](); //3

//闭包
var data = [];
for (var i = 0; i < 3; i++) {
  data[i] = (function (i) {
        return function(){
            console.log(i);
        }
  })(i);
}
data[0](); //0
data[1](); //1
data[2](); //2
```
