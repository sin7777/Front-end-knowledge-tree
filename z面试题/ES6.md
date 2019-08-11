# 面试问你 ES6

> [ES6面试、复习干货知识点汇总（全）](https://juejin.im/post/5c061ed2f265da61357258ee)

## 你对 ES6 有什么理解

ES6是新一代的JS语言标准，对分JS语言核心内容做了升级优化，规范了JS使用标准，新增了JS原生方法，使得JS使用更加规范，更加优雅，更适合大型应用的开发。

## 有了var为什么还要用let

ES6 之前，声明变量只能用 var，但是 var 的变量声明不是块级作用域的，会带来很多问题（for循环var变量泄露，变量覆盖等问题），let 声明有自己的块级作用域，而且不会变量提升。

## ES6 对 String 字符串类型的常用升级优化

* **新增字符串模板**，在拼接大段字符串时，用反斜杠(`)取代以往的字符串相加的形式，能保留所有空格和换行，使得字符串拼接看起来更加直观，更加优雅。
* ES6在String原型上**新增了includes()方法**，用于取代传统的只能用indexOf查找包含字符的方法(indexOf返回-1表示没查到不如includes方法返回false更明确，语义更清晰)

## ES6 对 Array 数组类型做的常用升级优化

* **数组解构赋值**。ES6可以直接以let [a,b,c] = [1,2,3]形式进行变量赋值
* **扩展运算符**。ES6新增的扩展运算符(...)(重要),可以轻松的实现数组和松散序列的相互转化，可以取代arguments对象和apply方法，轻松获取未知参数个数情况下的参数集合。
* ES6在Array原型上**新增了find()方法**，用于取代传统的只能用indexOf查找包含数组项目的方法,且修复了indexOf查找不到NaN的bug([NaN].indexOf(NaN) === -1).此外还新增了copyWithin(), includes(), fill(),flat()等方法，可方便的用于字符串的查找，补全,转换等。

## ES6 对 Number 数字类型做的常用升级优化

* ES6在Number原型上新增了isFinite(), isNaN()方法，用来取代传统的全局isFinite(), isNaN()方法检测数值是否有限、是否是NaN。
* ES6在Math对象上新增了Math.cbrt()，trunc()，hypot()等等较多的科学计数法运算方法，可以更加全面的进行立方根、求和立方根等等科学计算。

## ES6 对 Object 类型做的常用升级优化(重要)

* **对象属性变量式声明**，ES6可以直接以变量形式声明对象属性或者方法，。比传统的键值对形式声明更加简洁，更加方便，语义更加清晰。尤其在对象解构赋值时，这种写法的好处体现的最为明显
* **对象的解构赋值**，let {apple, orange} = {apple: 'red appe', orange: 'yellow orange'};
* **对象的扩展运算符(...)**，最常用也最好用的用处就在于可以轻松的取出一个目标对象内部全部或者部分的可遍历属性，从而进行对象的合并和分解

```JS
let {apple, orange, ...otherFruits} = {apple: 'red apple', orange: 'yellow orange', grape: 'purple grape', peach: 'sweet peach'};
// otherFruits  {grape: 'purple grape', peach: 'sweet peach'}
// 注意: 对象的扩展运算符用在解构赋值时，扩展运算符只能用在最有一个参数(otherFruits后面不能再跟其他参数)
let moreFruits = {watermelon: 'nice watermelon'};
let allFruits = {apple, orange, ...otherFruits, ...moreFruits};
```

* **super 关键字**。ES6在Class类里新增了类似this的关键字super。同this总是指向当前函数所在的对象不同，super关键字总是指向当前函数所在对象的原型对象。
* ES6在Object原型上新增了**is()方法**，做两个目标对象的相等比较，用来完善'==='方法，（Object.is(NaN, NaN) // true，Object.is(+0, -0) // false)
* ES6在Object原型上新增了**assign()方法**，用于对象新增属性或者多个对象合并

> Object.assign()： 忽略enumerable为false的属性，只拷贝对象自身的可枚举的属性。

* ES6在Object原型上新增了getOwnPropertyDescriptors()方法
* ES6在Object原型上新增了**getPrototypeOf()和setPrototypeOf(**)方法，用来获取或设置当前对象的prototype对象。这个方法存在的意义在于，ES5中获取设置prototype对像是通过__proto__属性来实现的，然而__proto__属性并不是ES规范中的明文规定的属性，只是浏览器各大产商“私自”加上去的属性，只不过因为适用范围广而被默认使用了，再非浏览器环境中并不一定就可以使用。
* ES6在Object原型上还新增了**Object.keys()，Object.values()，Object.entries()方法**，用来获取对象的所有键、所有值和所有键值对数组。

## ES6对 Function 函数类型做的常用升级优化?(重要)

* **箭头函数**(核心)。箭头函数是ES6核心的升级项之一，箭头函数里没有自己的this,这改变了以往JS函数中最让人难以理解的this运行机制。主要优化点:
  * 箭头函数内的this指向的是函数定义时所在的对象，而不是函数执行时所在的对象
  * 箭头函数不能用作构造函数，因为它没有自己的this，无法实例化
  * 也是因为箭头函数没有自己的this,所以箭头函数 内也不存在arguments对象。
* 函数默认赋值
* 新增了**双冒号运算符**，用来取代以往的bind，call,和apply。(浏览器暂不支持，Babel已经支持转码)

```JS
foo::bar;
// 等同于
bar.bind(foo);

foo::bar(...arguments);
// 等同于
bar.apply(foo, arguments);
```

## Symbol是什么，有什么作用

Symbol是ES6引入的第七种数据类型，所有Symbol()生成的值都是独一无二的，可以从根本上解决对象属性太多导致属性名冲突覆盖的问题。对象中Symbol()属性不能被for...in遍历，但是也不是私有属性。

## Set是什么，有什么作用

Set是ES6引入的一种类似Array的新的数据结构，Set实例的成员类似于数组item成员，区别是Set实例的成员都是唯一，不重复的。这个特性可以轻松地实现数组去重。

## Map是什么，有什么作用

Map是ES6引入的一种类似Object的新的数据结构，Map可以理解为是Object的超集，打破了以传统键值对形式定义对象，对象的key不再局限于字符串，也可以是Object。

Object 结构提供了“字符串—值”的对应，Map 结构提供了“值—值”的对应，是一种更完善的 Hash 结构实现。可以更加全面的描述对象的属性。

### Map 和 Object 的区别

## Proxy是什么，有什么作用

Proxy是ES6新增的一个构造函数，可以理解为JS语言的一个代理，用来改变JS默认的一些语言行为，包括拦截默认的get/set等底层方法，使得JS的使用自由度更高，可以最大限度的满足开发者的需求。比如通过拦截对象的get/set方法，可以轻松地定制自己想要的key或者value。

## Reflect是什么，有什么作用

Reflect是ES6引入的一个新的对象，他的主要作用有两点，一是将原生的一些零散分布在Object、Function或者全局函数里的方法(如apply、delete、get、set等等)，统一整合到Reflect上，这样可以更加方便更加统一的管理一些原生API。其次就是因为Proxy可以改写默认的原生API，如果一旦原生API别改写可能就找不到了，所以Reflect也可以起到备份原生API的作用，使得即使原生API被改写了之后，也可以在被改写之后的API用上默认的API。

## Promise是什么，有什么作用

Promise是ES6引入的一个新的对象，他的主要作用是用来解决JS异步机制里，回调机制产生的“回调地狱”。它并不是什么突破性的API，只是封装了异步回调形式，使得异步回调可以写的更加优雅，可读性更高，而且可以链式调用。

## Iterator是什么，有什么作用？(重要)

Iterator是ES6中一个很重要概念，它并不是对象，也不是任何一种数据类型。因为ES6新增了Set、Map类型，他们和Array、Object类型很像，Array、Object都是可以遍历的，但是Set、Map都不能用for循环遍历。Iterator是为Set、Map、Array、Object新增一个统一的遍历API

## for...in 和 for...of 有什么区别

ES6规定，有所部署了载了Iterator接口的对象(可遍历对象)都可以通过for...of去遍历，而for..in仅仅可以遍历对象。

for...in循环，只能获得对象的键名，不能直接获取键值。ES6 提供for...of循环，允许遍历获得键值。

```JS
var arr = ['a', 'b', 'c', 'd'];
for (let a in arr) {
  console.log(a); // 0 1 2 3 键名
}
for (let a of arr) {
  console.log(a); // a b c d 键值
}
```

## Generator函数是什么，有什么作用

如果说JavaScript是ECMAScript标准的一种具体实现、Iterator遍历器是Iterator的具体实现，那么Generator函数可以说是Iterator接口的具体实现方式。

**执行Generator函数会返回一个遍历器对象**，每一次Generator函数里面的yield都相当一次遍历器对象的next()方法，并且可以通过next(value)方法传入自定义的value,来改变Generator函数的行为。

Generator函数可以通过配合 Thunk 函数更轻松更优雅的实现异步编程和控制流管理

* 用 Generator 封装 Ajax

```JS
function* main() {
  var result = yield request("http://some.url");
  var resp = JSON.parse(result);
    console.log(resp.value);
}
function request(url) {
  makeAjaxCall(url, function(response){
    it.next(response);
  });
}
var it = main();
it.next();
```

* 利用 Generator 函数，可以在任意对象上部署 Iterator 接口。

```JS
function* iterEntries(obj) {
  let keys = Object.keys(obj);
  for (let i=0; i < keys.length; i++) {
    let key = keys[i];
    yield [key, obj[key]];
  }
}
let myObj = { foo: 3, bar: 7 };

for (let [key, value] of iterEntries(myObj)) {
  console.log(key, value);
}
// foo 3
// bar 7
```

## async函数是什么，有什么作用

async函数可以理解为内置自动执行器的Generator函数语法糖，它配合ES6的Promise近乎完美的实现了异步编程解决方案。

## Class、extends是什么，有什么作用

Class类可以通过extends实现继承。它和ES5构造函数的不同点：

* 类的内部定义的所有方法，都是不可枚举的。
* ES6的 class 类必须用 new 命令操作，而 ES5 的构造函数不用 new 也可以执行。
* ES5 的继承，实质是先创造子类的实例对象this，然后再将父类的方法添加到this上面。ES6 的继承机制完全不同，实质是先将父类实例对象的属性和方法，加到this上面（所以必须先调用super方法），然后再用子类的构造函数修改this。

## module、export、import是什么，有什么作用

module、export、import是ES6用来统一前端模块化方案的设计思路和实现方案。export、import的出现统一了前端模块化的实现方案。

import引入的模块是**静态加载**（编译阶段加载）而不是动态加载（运行时加载，CommonJS）。

import引入export导出的接口值是动态绑定关系，即通过该接口，可以取到模块内部实时的值。
