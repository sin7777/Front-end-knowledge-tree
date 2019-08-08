# Some JS Tips

## 对象的属性

ES5中对象的属性其实分为两种: 数据属性和访问器属性

数据属性

* configurable: 属性是否可删除、重新定义
* enumerable: 属性是否可枚举
* writable: 属性值是否可修改
* value: 属性值

访问器属性

* configurable: 属性是否可删除、重新定义
* enumerable: 属性是否可枚举
* get: 读取属性调用
* set: 设置属性调用

## 词法作用域与动态作用域

JS采用的是词法作用域，也就是说静态作用域，函数的作用域在函数定义的时候就决定了。

而与词法作用域相对的是动态作用域，函数的作用域是在函数调用的时候才决定的。

## 执行上下文创建之后

执行上下文的生命周期有两个阶段：

* 创建阶段：执行上下文会分别创建变量对象，建立作用域链，以及确定this的指向
* 代码执行阶段：会完成变量赋值，函数引用，以及执行其他代码
  
变量对象会包括:

1. 函数的所有形参(如果是函数上下文)
    由名称和对应值组成的一个变量对象的属性被创建
    没有实参，属性值设为undefined
2. 函数声明
    由名称和对应值(函数对象(function-object)) 组成一个变量对象的属性被创建
    如果变量对象已经存在相同名称的属性，则完全替换这个属性
3. 变量声明
    由名称和对应值(undefined) 组成一 个变量对象的属性被创建;
    如果变量名称跟已经声明的形式参数或函数相同，则变量声明不会干扰已经存在的这类属性

## Object.create

Object.create()方法创建一个新对象，使用现有的对象来提供新创建的对象的__proto__

```JS
Object.create(proto, [propertiesObject])
//proto  新创建对象的原型对象
//propertiesObject  新创建对象的可枚举属性
   //configurable  true当且仅当该属性描述符的类型可以被改变并且该属性可以从对应对象中删除
   //enumerable  true 当且仅当在枚举相应对象上的属性时该属性显现
   //value  
   //writable  true 当且仅当与该属性相关联的值可以用assignment operator改变时


Object.create(null)  //创建的对象是一个空对象,没有继承任何方法
var o = Object.create({}) //o.__proto__.__proto__.__proto__ === null
var p = Object.create(Object.prototype) //p.__proto__.__proto__ === null
```

Object.create() 与 new Object() 的区别

```JS
var a = new Object()
var b = Object.create({})
//a.__proto__.__proto__ === null
//b.__proto__.__proto__.__proto__ === null
```

## 数组解构与对象解构

```JS
//数组解构，根据位置的对应
var a, b, rest;
[a, b] = [10, 20];
console.log(a); // 10
console.log(b); // 20

[a, b, ...rest] = [10, 20, 30, 40, 50];
console.log(a); // 10
console.log(b); // 20
console.log(rest); // [30, 40, 50]

//对象解构，根据键值对应
var o = {p: 42, q: true};
var {p, q} = o;

console.log(p); // 42
console.log(q); // true
```

## Object.is()

Object.is() 判断两个值是否相同。如果下列任何一项成立，则两个值相同：

* 两个值都是 undefined
* 两个值都是 null
* 两个值都是 true 或者都是 false
* 两个值是由相同个数的字符按照相同的顺序组成的字符串
* 两个值指向同一个对象
* 两个值都是数字并且
  * 都是正零 +0
  * 都是负零 -0
  * 都是 NaN
  * 都是除零和 NaN 外的其它同一个数字

```JS
//polyfill
if (!Object.is) {
  Object.is = function(x, y) {
    if (x === y) {
      //  +0 !== -0
      return x !== 0 || 1 / x === 1 / y;
    } else {
      //  NaN === NaN
      return x !== x && y !== y;
    }
  };
}
```

## Object.assign()

[Object.assign(target, ...sources)](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)

* 浅拷贝
* 合并具有相同属性的对象
* 继承属性和不可枚举属性是不能拷贝的
* 原始类型会被包装为对象
* 异常会打断后续拷贝任务

## 展开运算符（...）

展开语法

可以在函数调用/数组构造时, 将数组表达式或者string在语法层面展开；

还可以在构造字面量对象时, 将对象表达式按key-value的方式展开。(译者注: 字面量一般指 [1, 2, 3] 或者 {name: "mdn"} 这种简洁的构造方式)

```JS
myFunction(...iterableObj); //函数调用
[...iterableObj, '4', ...'hello', 6]; //字面量数组构造或字符串
let objClone = { ...obj }; //构造字面量对象时,进行克隆或者属性拷贝
```

运用

* 合并数组
* 与解构赋值结合
* 字符串转为真正的数组
* 实现了 Iterator 接口的对象，任何 Iterator 接口的对象，都可以用扩展运算符转为真正的数组。

扩展运算符内部调用的是数据结构的 Iterator 接口，因此只要具有 Iterator 接口的对象，都可以使用扩展运算符，比如 Map 结构。

## forEach 遍历 for……in 遍历 for……of遍历

forEach 遍历写法的问题在于，无法中途跳出forEach循环，break命令或return命令都不能奏效。

for……in 遍历的特点

* 数组的键名是数字，但是for...in循环是以字符串作为键名“0”、“1”、“2”等等。
* for...in循环不仅遍历数字键名，还会遍历手动添加的其他键，甚至包括原型链上的键。
* 某些情况下，for...in循环会以任意顺序遍历键名。

for……of 遍历的特点

* 有着同for...in一样的简洁语法，但是没有for...in那些缺点。
* 不同于forEach方法，它可以与break、continue和return配合使用。
* 提供了遍历所有数据结构的统一操作接口。
