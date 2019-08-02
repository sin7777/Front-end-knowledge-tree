# js函数定义的方法

函数式编程：

函数即数据，函数可以赋值给变量，可以当参数传递给其他函数，还可以从函数里返回等等。

## 函数表达式声明

不会变量提升，可以直接加括号执行变成立即执行函数

```JS
var getRectArea = function(width, height) {
    return width * height;
}

console.log(getRectArea(3,4));
// expected output: 12
```

## function操作符

会变量提升

```JS
function calcRectArea(width, height) {
  return width * height;
}

console.log(calcRectArea(5, 6));
// expected output: 30
```

## Function 构造函数

```JS
var sum = new Function('a', 'b', 'return a + b');
```

## ES6 箭头函数

[讲解](https://juejin.im/post/5aa1eb056fb9a028b77a66fd#heading-8)

```JS
var materials = [
  'Hydrogen',
  'Helium',
  'Lithium',
  'Beryllium'
];
console.log(materials.map(material => material.length));
// expected output: Array [8, 6, 7, 9]
```

当你定义一个箭头函数，在普通函数里常见的this、arguments、caller是统统没有的。

箭头函数this指向的问题：

* 默认绑定外层的this
* 不能用call来改变this的指向

箭头函数与回调函数

```JS
loader.onLoaded(map=>onResourceFirstLoaded(map))
//等效于：
loader.onLoaded(
  function(map){
      return onResourceFirstLoaded(map)
    }
)
```
