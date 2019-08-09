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

## 合成事件

[官方文档](https://react.docschina.org/docs/events.html)

如果DOM上绑定了过多的事件处理函数，整个页面响应以及内存占用可能都会受到影响。React为了避免这类DOM事件滥用，同时屏蔽底层不同浏览器之间的事件系统差异，实现了一个中间层——SyntheticEvent。

**原理**：React并不是将click事件绑在该div的真实DOM上，而是在document处监听所有支持的事件，当事件发生并冒泡至document处时，React将事件内容封装并交由真正的处理函数运行。

* 合成事件的监听器是**统一注册在document上**的，且仅有冒泡阶段。所以原生事件的监听器响应总是比合成事件的监听器早
* 阻止原生事件的冒泡后，会阻止合成事件的监听器执行

react事件机制分为**两个部分**：1、事件注册 2、事件分发

事件注册部分，所有的事件都会注册到document上，拥有统一的回调函数dispatchEvent来执行事件分发

事件分发部分，首先生成合成事件，注意同一种事件类型只能生成一个合成事件Event，如onclick这个类型的事件，dom上所有带有通过jsx绑定的onClick的回调函数都会按顺序（冒泡或者捕获）会放到Event._dispatchListeners 这个数组里，后面依次执行它。

React使用对象池来管理合成事件对象的创建和销毁，这样减少了垃圾的生成和新对象内存的分配，大大提高了性能

注意点：

* 无法使用return false的方式来阻止事件的一些默认行为，必须得使用preventDefault。
* 无法再异步的情况下调用事件（访问e）

## refs 的作用

下面是几个适合使用 refs 的情况：

* 管理焦点，文本选择或媒体播放。
* 触发强制动画。
* 集成第三方 DOM 库。

## PropTypes

React 也内置了一些类型检查的功能。要在组件的 props 上进行类型检查，你只需配置特定的 propTypes 属性：

```JSX
import PropTypes from 'prop-types';

class Greeting extends React.Component {
  render() {
    return (
      <h1>Hello, {this.props.name}</h1>
    );
  }
}

Greeting.propTypes = {
  name: PropTypes.string
};
```

PropTypes 提供一系列验证器，可用于确保组件接收到的数据类型是有效的。在本例中, 我们使用了 PropTypes.string。当传入的 prop 值类型不正确时，JavaScript 控制台将会显示警告。出于性能方面的考虑，propTypes 仅在开发模式下进行检查。

## SSR 服务器端渲染

> 其实我好像没有怎么了解哇

服务端渲染（以下简称 SSR ）是一个将通过前端框架构建的网站通过后端渲染模板的形式呈现的过程

SSR 有以下两个好处：

* 加快了首屏渲染时间
* 完整的可索引的 HTML 页面（有利于 SEO)
