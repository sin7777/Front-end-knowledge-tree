# 类数组

> 常接触到的类数组是函数的arguments属性，以及DOM的HTMLCollection
> [详细讲解](https://github.com/mqyqingfeng/Blog/issues/14)

## 类数组转数组的方式

```JS
var array = ['name', 'age', 'sex'];

var arrayLike = {
    0: 'name',
    1: 'age',
    2: 'sex',
    length: 3
}

// 1. slice
Array.prototype.slice.call(arrayLike); // ["name", "age", "sex"]
// 2. splice
Array.prototype.splice.call(arrayLike, 0); // ["name", "age", "sex"]
// 3. ES6 Array.from
Array.from(arrayLike); // ["name", "age", "sex"]
// 4. apply
Array.prototype.concat.apply([], arrayLike)
// 5.
Array.prototype.map.call(arrayLike, function(item){
    return item;
})
//ES6
function func(...arguments) {
    console.log(arguments); // [1, 2, 3]
}
```

## 类数组的检测

```JS
function isArrayLike(o) {
    if (o &&                                // o is not null, undefined, etc.
        typeof o === 'object' &&            // o is an object
        isFinite(o.length) &&               // o.length is a finite number
        o.length >= 0 &&                    // o.length is non-negative
        o.length===Math.floor(o.length) &&  // o.length is an integer
        o.length < 4294967296)              // o.length < 2^32
        return true;                        // Then o is array-like
    else
        return false;                       // Otherwise it is not
}
```
