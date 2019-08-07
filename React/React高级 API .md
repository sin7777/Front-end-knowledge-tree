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
