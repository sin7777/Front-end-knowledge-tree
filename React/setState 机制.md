# setState

setState 用来驱动UI

```JSX
const count = this.state.count； //读取状态
this.setState({count: count + 1}）； //更新状态
this.state.count = count + 1; //无意义
```

this.state只是一个对象，单纯去修改一个对象的值是没有意义的，去驱动UI的更新才是有意义的

setState通过引发一次组件的更新过程来引发重新绘制

* setState 只在**合成事件**和**生命周期**中是“异步”的，在原生事件和 setTimeout 中都是同步的。
* setState的“异步”并不是说内部由异步代码实现，其实本身执行的过程和代码都是同步的，只是**合成事件和生命周期的调用顺序在更新之前**，导致在合成事件和生命周期中没法立马拿到更新后的值，形式了所谓的“异步”，当然可以通过第二个参数 setState(partialState, callback) 中的callback拿到更新后的结果。
* setState 的批量更新优化也是建立在“异步”（合成事件、钩子函数）之上的，在原生事件和setTimeout 中不会批量更新，在“异步”中如果对同一个值进行多次 setState ， setState 的批量更新策略会对其进行覆盖，取最后一次的执行，如果是同时 setState 多个不同的值，在更新时会对其进行合并批量更新。

![setState](https://pic1.zhimg.com/80/4fd1a155faedff00910dfabe5de143fc_hd.png)

* 合成事件

合成事件，react为了解决跨平台，兼容性问题，自己封装了一套事件机制，代理了原生的事件，像在jsx中常见的onClick、onChange这些都是合成事件
合成事件中也有batchedUpdates方法，是通过同样的事务完成的

* 生命周期

整个生命周期中就是一个事物操作,所以标识位isBatchingUpdates = true,所以流程到了enqueueUpdate()时,实例对象都会加入到dirtyComponents 数组中

* 原生事件

原生事件是指非react合成事件，原生自带的事件监听 addEventListener ，或者也可以用原生js、jq直接 document.querySelector().onclick 这种绑定事件的形式都属于原生事件

原生事件绑定不会通过合成事件的方式处理，自然也不会进入更新事务的处理流程。setTimeout也一样，在setTimeout回调执行时已经完成了原更新组件流程，不会放入dirtyComponent进行异步更新，其结果自然是同步的。

```JSX
onClick = () => {
    this.setState({ index: this.state.index + 1 });
    this.setState({ index: this.state.index + 1 });
}
// 最后解析为,后面的数据会覆盖前面的更改，所以最终只加了一次.
Object.assign(
  previousState,
  {index: state.index+ 1},
  {index: state.index+ 1},
)
//正确写法
onClick = () => {
    this.setState((prevState, props) => {
      return {quantity: prevState.quantity + 1};
    });
    this.setState((prevState, props) => {
      return {quantity: prevState.quantity + 1};
    });
}
```
