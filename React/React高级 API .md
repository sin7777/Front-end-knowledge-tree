# React 高级 API

## Context

[官方API](https://react.docschina.org/docs/context.html#when-to-use-context)

Context 是 React 中跨层级的组件数据传递的一种方式，Context 提供了一个无需为每层组件手动添加 props，就能在组件树间进行数据传递的方法。

你不想在组件树中通过逐层传递props或者state的方式来传递数据时，可以使用Context来实现跨层级的组件数据传递。

如果要Context发挥作用，需要用到两种组件，一个是Context生产者(Provider)，通常是一个父节点，另外是一个Context的消费者(Consumer)，通常是一个或者多个子节点。所以Context的使用基于生产者消费者模式。

### API

* React.createContext
* Context.Provider
* Class.contextType
* Context.Consumer

### 几个可以直接获取Context的地方

实际上，除了实例的context属性(this.context)，React组件还有很多个地方可以直接访问父组件提供的Context。比如构造方法：

constructor(props, context)

比如生命周期：

* componentWillReceiveProps(nextProps, nextContext)
* shouldComponentUpdate(nextProps, nextState, nextContext)
* componetWillUpdate(nextProps, nextState, nextContext)

对于面向函数的无状态组件，可以通过函数的参数直接访问组件的Context。

## 错误边界

在 React 中，我们通常有一个组件树。如果任何一个组件发生错误，它将破坏整个组件树。没有办法捕捉这些错误，我们可以用错误边界优雅地处理这些错误。

错误边界有两个作用

* 如果发生错误，显示回退UI
* 记录错误

## 高阶组件

[完美参考，面试问到的话要点开鸭](https://juejin.im/post/5cad39b3f265da03502b1c0a)

高阶组件是参数为组件，返回值为新组件的函数。

```JS
const EnhancedComponent = higherOrderComponent(WrappedComponent);
```

HOC 是纯函数，没有副作用

> 什么是纯函数：如果函数的调用参数相同，则永远返回相同的结果。它不依赖于程序执行期间函数外部任何状态或数据的变化，必须只依赖于其输入参数。 该函数不会产生任何可观察的副作用，例如网络请求，输入和输出设备或数据突变。

* HOC 的实现方式
  * 属性代理
    * 可操作所有传入的props
    * 可操作组件的生命周期
    * 可操作组件的static方法
    * 获取refs
  * 反向继承
    * 可操作所有传入的props
    * 可操作组件的生命周期
    * 可操作组件的static方法
    * 获取refs
    * 可操作state
    * 可以渲染劫持
* HOC 可是实现的功能
  * 组合渲染
  * 条件渲染
  * 操作props
  * 获取refs
  * 状态管理
  * 操作state
  * 渲染劫持
* 注意事项
  * 不要在 render 内创建高阶组件（React Diff 的相同组件更新的方法，会被卸载重载，损耗性能）

![使用HOC的动机](https://user-gold-cdn.xitu.io/2019/4/10/16a04a94cd939f75?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

Mixin 带来的风险：

* Mixin 可能会相互依赖，相互耦合，不利于代码维护
* 不同的Mixin中的方法可能会相互冲突
* Mixin非常多时，组件是可以感知到的，甚至还要为其做相关处理，这样会给代码造成滚雪球式的复杂性

HOC的出现可以解决这些问题：

* 高阶组件就是一个没有副作用的纯函数，各个高阶组件不会互相依赖耦合
* 高阶组件也有可能造成冲突，但我们可以在遵守约定的情况下避免这些行为
* 高阶组件并不关心数据使用的方式和原因，而被包裹的组件也不关心数据来自何处。高阶组件的增加不会为原组件增加负担

## Portals

Portal 提供了一种将子节点渲染到存在于父组件以外的 DOM 节点的优秀的方案。

对于 portal 的一个典型用例是当父组件有 overflow: hidden 或 z-index 样式，但你需要子组件能够在视觉上 “跳出(break out)” 其容器。例如，对话框、hovercards以及提示框。

特点：事件冒泡

事件冒泡和普通react子节点一样，是因为portal仍然存在于**React tree**中，而不用考虑其在真是DOM tree中的位置
