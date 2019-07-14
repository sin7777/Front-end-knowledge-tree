# JS基本类型

## 基本类型

* Null
* undefined
* Bool
* String
* Number
* Object
* Symbol

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

模拟实现instanceof()

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
