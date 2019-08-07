# Iterator

遍历器（Iterator）就是这样一种机制。它是一种接口，为各种不同的数据结构提供统一的访问机制。任何数据结构只要部署 Iterator 接口，就可以完成遍历操作

Iterator 的作用有三个：

* 一是为各种数据结构，提供一个统一的、简便的访问接口
* 二是使得数据结构的成员能够按某种次序排列
* 三是 ES6 创造了一种新的遍历命令for...of循环，Iterator 接口主要供for...of消费。

Iterator 的遍历过程是这样的。

1. 创建一个指针对象，指向当前数据结构的起始位置。也就是说，遍历器对象本质上，就是一个指针对象。
2. 第一次调用指针对象的next方法，可以将指针指向数据结构的第一个成员。
3. 第二次调用指针对象的next方法，指针就指向数据结构的第二个成员。
4. 不断调用指针对象的next方法，直到它指向数据结构的结束位置。

每一次调用next方法，都会返回数据结构的当前成员的信息。具体来说，就是返回一个包含value和done两个属性的对象。其中，value属性是当前成员的值，done属性是一个布尔值，表示遍历是否结束。

原生具备 Iterator 接口的数据结构如下。

* Array
* Map
* Set
* String
* TypedArray
* 函数的 arguments 对象
* NodeList 对象

对象（Object）之所以没有默认部署 Iterator 接口，是因为对象的哪个属性先遍历，哪个属性后遍历是不确定的，需要开发者手动指定。本质上，**遍历器是一种线性处理，对于任何非线性的数据结构，部署遍历器接口，就等于部署一种线性转换。**不过，严格地说，对象部署遍历器接口并不是很必要，因为这时对象实际上被当作 Map 结构使用，ES5 没有 Map 结构，而 ES6 原生提供了。

## 调用 Iterator 接口的场合

1. 解构赋值
2. 扩展运算符
3. yield*
4. 其他场合
   * for...of
   * Array.from()
   * Map(), Set(), WeakMap(), WeakSet()（比如new Map([['a',1],['b',2]])）
   * Promise.all()
   * Promise.race()

## 模拟实现

```JS
let obj = {0:1,1:2,2:3,length:3}
console.log([...obj]) //<<<TypeError: obj is not iterable

//模拟实现
let obj = {0:1,1:2,2:3,length:3,[Symbol.iterator]:function(){
  //内部会自动执行下面 每次调用next 通过done来决定是否停止
  let index  = 0; //当前迭代到了第几个
  let self = this; //this指代的是当前对象
  return {
    next:function(){ // value代表的是当前的内容 done代表的是是否迭代完成
      return {
          value: self[index],
          done: index++ === self.length?true:false
        }
    }
  }
}}

//正规实现
Obj.prototype[Symbol.iterator] = function() {
  var iterator = { next: next };
  var current = this;
  function next() {
    if (current) {
      var value = current.value;
      current = current.next;
      return { done: false, value: value };
    } else {
      return { done: true };
    }
  }
  return iterator;
}
```
