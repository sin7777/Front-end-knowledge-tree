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

1. 新生成了一个对象
2. 链接到原型
3. 绑定 this
4. 返回新对象
