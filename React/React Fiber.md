# React Fiber

> 比较难解释，建议长期学习
>
> 参考资料
>
> [浅析 React Fiber](https://juejin.im/post/5be969656fb9a049ad76931f)
>
> [React Fiber 架构理解](https://segmentfault.com/a/1190000018701625)
>
> [React Fiber](https://juejin.im/post/5ab7b3a2f265da2378403e57)

## React Fiber 为什么出现

React v15 版本中有一个非常大的问题是 **“调度阶段不可控”**，这是什么意思？

假如我们更新一个 state，有1000个组件需要更新，每个组件更新需要1ms，那么我们就会有将近1s的时间，主线程被React占着用来调度，这段时间内用户的操作不会得到任何的反馈，只有当 React 中需要同步更新的任务完成后，主线程才被释放。对于这1s内 React 的调度，我们是无能为力的。

GUI 线程与 JS 线程是互斥的

![渲染流程1](https://user-gold-cdn.xitu.io/2018/11/12/16707c4c856a51fa?imageslim)
![渲染流程2](https://user-gold-cdn.xitu.io/2018/11/12/16707c4c8a38ee7c?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

解决这个问题的思路就是：分片！也就是 React Fiber

![渲染流程3](https://user-gold-cdn.xitu.io/2018/11/12/16707c4c87f4a231?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

## React Fiber 是怎么工作的

Fiber 把更新过程碎片化，每执行完一段更新过程，就把控制权交还给 React 负责任务协调的模块，看看有没有其他紧急任务要做，如果没有就继续去更新，如果有优先级更高的任务，那就去做优先级高的任务。

如果任务队列执行时间超过了deathLine，则设置为pending状态挂起状态

一个Fiber执行结束或挂起，会调用基于**requestIdleCallback/requestAnimationFrame**实现的调度器，返回一个新的Fiber任务队列继续进行上述过程

![Fiber流程图](https://segmentfault.com/img/bVbqDhh?w=720&h=405)

### Fiber Node 及 Fiber Tree

* 从流程图上看到会有 Fiber Node 节点，这个是在 react 生成的 Virtual Dom 基础上增加的一层数据结构，主要是为了将递归遍历转变成循环遍历，配合 requestIdleCallback API, 实现任务拆分、中断与恢复。为了实现循环遍历，Fiber Node 上携带了更多的信息。
* 每一个 Fiber Node 节点与 Virtual Dom 一一对应，所有 Fiber Node 连接起来形成 Fiber tree, 是个单链表树结构

> 把之前栈的结构改成递归的结构

```JS
//fiber 结构
fiber {
    stateNode: {},
    child: {},
    return: {},
    sibling: {},
}
```

### 两个阶段：reconciliation 和 commit

Reconciliation 阶段 （React算法，用来比较2颗树，以确定哪些部分需要重新渲染，可以被打断）render之前

* componentWillMount
* componentWillReceiveProps
* shouldComponentUpdate
* componentWillUpdate

在[](https://segmentfault.com/a/1190000018701625)文中 `构建 workInProgress tree 的过程就是 diff 的过程` ？

Commit 阶段 （用于呈现 React 应用的数据更改。通常是setState的结果。最终导致重新渲染。）render之后

* componentDidMount
* componentDidUpdate
* componentWillUnmount

### Fiber 在第一段渲染之后，怎么知道下一段从哪个组件开始渲染呢

Fiber节点拥有return, child, sibling三个属性，分别对应父节点， 第一个孩子， 它右边的兄弟， 有了它们就足够将一棵树变成一个链表， 实现深度优化遍历。

### 怎么决定每次更新的数量

👇

## React Fiber 和 Virtual DOM 是怎么协作的

在React15中，每次更新时，都是从根组件或setState后的组件开始，更新整个子树，我们唯一能做的是，在某个节点中使用SUC断开某一部分的更新，或者是优化SUC的比较效率。

React16则是需要将虚拟DOM转换为Fiber节点，首先它规定一个时间段内，然后在这个时间段能转换多少个FiberNode，就更新多少个。

因此我们需要将我们的更新逻辑分成两个阶段，第一个阶段是将虚拟DOM转换成Fiber, Fiber转换成组件实例或真实DOM（不插入DOM树，插入DOM树会reflow）。Fiber转换成后两者明显会耗时，需要计算还剩下多少时间。并且转换实例需要调用一些钩子，如componentWillMount, 如果是重复利用已有的实例，这时就是调用componentWillReceiveProps, shouldComponentUpdate, componentWillUpdate,这时也会耗时。

## React Fiber 带来的影响

因为 reconciliation 阶段是可以被打断的，所以 reconciliation 阶段会执行的生命周期函数就可能会出现调用多次的情况，从而引起 Bug。所以对于 reconciliation 阶段调用的几个函数，除了 shouldComponentUpdate 以外，其他都应该避免去使用，并且 React16 中也引入了新的 API 来解决这个问题。

于是官方推出了getDerivedStateFromProps，让你在render设置新state，你主要返回一个新对象，它就主动帮你setState。由于这是一个静态方法，你不能取到 this，当然你也不能操作instance，这就阻止了你多次操作setState。这样一来，getDerivedStateFromProps的逻辑应该会很简单，这样就不会出错，不会出错，就不会打断`DFS`过程。

## requestIdleCallback 与 requestAnimationFrame

[你应该知道的requestIdleCallback](https://juejin.im/post/5ad71f39f265da239f07e862)

requestAnimationFrame的回调会在每一帧确定执行，属于高优先级任务，而requestIdleCallback的回调则不一定，属于低优先级任务。

一帧包含了用户的交互、js的执行、以及requestAnimationFrame的调用，布局计算以及页面的重绘等工作。
假如某一帧里面要执行的任务不多，在不到16ms（1000/60)的时间内就完成了上述任务的话，那么这一帧就会有一定的空闲时间，这段时间就恰好可以用来执行requestIdleCallback的回调，如下图所示：

![requestIdleCallback](https://user-gold-cdn.xitu.io/2018/4/18/162d8538cf65118c?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

浏览器本身也不断进化中，随着页面由简单的展示转向WebAPP，它需要一些新能力来承载更多节点的展示与更新。下面是一些自救措施：

* requestAnimationFrame（帧数控制调用）
* requestIdleCallback（闲时调用）
* web worker（多线程调用）
* IntersectionObserver（进入可视区调用）

## 为什么使用深度优化遍历

这涉及一个很经典的消息通信问题。如果是父子通信，我们可以通过props进行通信，子组件可以保存父的引用，可以随时call父组件。如果是多级组件间的通信，或不存在包含关系的组件通信就麻烦了，于是React发明了上下文对象（context）
