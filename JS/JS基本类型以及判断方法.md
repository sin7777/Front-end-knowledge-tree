# JS基本类型

## 基本类型

* Null
* undefined
* Bool
* String
* Number
* Symbol
* Object（这个叫引用类型）

### null 和 undefined 的区别

null表示"没有对象"，即该处不应该有值

undefined表示"缺少值"，就是此处应该有一个值，但是还没有定义。

null 和 undefined 在 if 判断中都会被判断为 false

null 在做数学运算时被转换成为 0， undefined 被转换成为 NaN

## 类型的判断方法

> [讲解](https://github.com/Advanced-Frontend/Daily-Interview-Question/issues/23)

* typeof()
* instanceof()
* Object.prototype.toString()
* isPrototypeOf()

使用方法

```JS
//判断变量tmp的是否为数组

Object.prototype.toString.call(tmp)  //"[object Array]"
//这种方法对于所有基本的数据类型都能进行判断

tmp instanceof Array  // 返回 true or false
//instanceof  的内部机制是通过判断对象的原型链中是不是能找到类型的 prototype。

Array.isArray(tmp);
//Array.isArray(); ES5新增方法，优于instanceof，可以检测出 iframes
```

模拟实现 instanceof()

```JS
//判断B是不是A的实例
function instance_of(A, B){
    let L = A.prototype,
        R = B.__proto__;
    while(ture){
        if(R = null){
            return false
        }
        if(L === R){
            return true
        }
        R = R.__proto__
    }
}
```

用typeof来判断基本类型，instanceof判断引用类型

instanceof 检测类型的方式是根据其原型链上的规则，但是基本类型（6个基本类型）没有原型链及相应方法，所以用 instanceof 判断基本类型都会返回 false

使用 instanceof 判断基本类型

```JS
class PrimitiveString {
  static [Symbol.hasInstance](x) {
    return typeof x === 'string'
  }
}
console.log('hello world' instanceof PrimitiveString) // true
```

```JS
let df = "1" //基本类型
//会进行强制类型转换，df转换成为String的实例
df.__proto__  //String {"", constructor: ƒ, anchor: ƒ, big: ƒ, blink: ƒ, …}
//会进行强制类型转换，df转换成为String的实例
df.__proto__.__proto__.__proto__  //null
//不会进行强制类型转换，为什么不会进行？
df instanceof String //false
```

```JS
//案例1
console.log(String instanceof String);//false
//案例2
console.log(Object instanceof Object);//true
console.log(Function instanceof Function);//true
console.log(Function instanceof Object);//true
//案例3
function Foo(){}
function BFoo(){}
Foo.prototype = new BFoo();
console.log(Foo instanceof Function); //true
console.log(Foo instanceof Foo);//false
```

![案例1](../images/原型链1.jpg)
![案例2](../images/原型链2.jpg)
![案例3](../images/原型链3.jpg)
