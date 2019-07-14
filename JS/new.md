# new 运算符

> [详细解说](https://www.cnblogs.com/onepixel/p/5043523.html)

```JS
let tmp = new F();
```

new 运算符做了什么操作？

```JS
var obj  = {};
obj.__proto__ = F.prototype;
F.call(obj);
```

第一行，我们创建了一个空对象obj;

第二行，我们将这个空对象的__proto__成员指向了F函数对象prototype成员对象;

第三行，我们将F函数对象的this指针替换成obj，然后再调用F函数.

我们可以这么理解: 以 new 操作符调用构造函数的时候，函数内部实际上发生以下变化：

1、创建一个空对象，并且 this 变量引用该对象，同时还继承了该函数的原型。

2、属性和方法被加入到 this 引用的对象中。

3、新创建的对象由 this 所引用，并且最后隐式的返回 this.
