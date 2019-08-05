# vue项目的难点

## 处理数组的时候不会触发视图响应式变化

在 vue3.0 之前，通过`Object.defineProperty`是无法对下面情况做响应式变化的 [传送门](https://juejin.im/post/5b2fe92be51d4558bf7c3add)

* 数组
  * 使用下标更新数组元素；
  * 使用赋值方式改变数组长度；
  * 使用下标增删数组元素；
* 增删元素

```JS
vm.items[indexOfItem] = newValue //利用索引直接设置数组的某一项
vm.items.length = newLength //修改数组的长度
```

在项目中实现的方法是通过官方推荐的 `Vue.set` 的方法来实现相应式的数组的

`Vue.set` 的实现原理其实很简单

在处理数组时对数组的7个操作进行监听

```JS
push()
pop()
shift()
unshift()
splice()
sort()
reverse()
```

正常的数组的 `__proto__` 属性指向 `Array.prototype`，而 vue 中的数组的 `__proto__` 属性指向一个对象，这个对象的 `__proto__` 属性才指向 `Array.prototype`，上面的7个方法都是在这个对象上面定义的。在这7个方法中实现更新视图。

在处理对象时，会判断这个对象是否是响应式的(`__ob__`属性)，如果是新加的属性**添加依赖**，以后再直接修改这个新的属性的时候就会触发页面渲染。

[此处链接到Vue的双向绑定](../Vue/双向绑定.md)

## 计算属性和监听属性的区别（待讲，还没有彻底搞清楚）

> [可看](https://juejin.im/post/5afbfce56fb9a07ac0226f21)

计算属性 `computed` 是依赖于数据劫持，自动监听依赖值得变化，计算之后的内容，可以避免在模板中放入太多的逻辑会让模板过重且难以维护。

* 数据监听
* 可以缓存

```JS
//Date.now() 不是响应式依赖，所以now的值不会变化
computed: {
  now: function () {
    return Date.now()
  }
}
```

为什么不直接写成方法？  可以避免重复计算

监听属性 `watch` 是监听值得变化，**触发一些回调**，处理事件逻辑

1. 如果一个数据依赖于其他数据，那么把这个数据设计为computed的  
2. 如果你需要在某个数据变化时做一些事情，使用watch来观察这个数据变化
