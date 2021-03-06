# React的diff算法

[详细讲解](https://zhuanlan.zhihu.com/p/20346379)

## 虚拟DOM只是轻量级的Js对象

一个真实的DOM节点有太多属性，直接对DOM进行操作的开销太大，所以引入虚拟DOM的概念

用 JavaScript 对象结构表示 DOM 树的结构；然后用这个树构建一个真正的 DOM 树，插到文档当中

当状态变更的时候，重新构造一棵新的对象树。然后用新的树和旧的树进行比较，记录两棵树差异

深度优先

把2所记录的差异应用到步骤1所构建的真正的DOM树上，视图就更新了
Virtual DOM 本质上就是在 JS 和 DOM 之间做了一个缓存

## React中改进的Diff算法主要从三个层面进行的

* **Tree diff**  同级比较，只做同层次树的比较，如果不同，会删除重新构建，一次遍历O(n)
* **Component diff**  同级之间组件不同，树不同
  * 如果是同一类型的组件，按照原策略继续比较 virtual DOM tree。
  * 如果不是，则将该组件判断为 dirty component，从而替换整个组件下的所有子节点。
  * 对于同一类型的组件，有可能其 Virtual DOM 没有任何变化，如果能够确切的知道这点那可以节省大量的 diff 运算时间，因此 React 允许用户通过 shouldComponentUpdate() 来判断该组件是否需要进行 diff。
* **element diff**  同层级之间通过设置key的区分，是否真正提交性能（通过key查找，还是通过遍历，这是性能的区分）
  * 提供了三种节点操作，分别为：INSERT_MARKUP（插入）、MOVE_EXISTING（移动） 和 REMOVE_NODE（删除）。

## React 中 keys 的作用是什么

Keys 是 React 用于追踪哪些列表中元素被修改、被添加或者被移除的辅助标识。

在开发过程中，我们需要保证某个元素的 key 在其同级元素中具有唯一性。

* React Diff 算法中 React 会借助元素的 Key 值来判断该元素是新近创建的还是被移动而来的元素，从而减少不必要的元素重渲染。
* React 还需要借助 Key 值来判断元素与本地状态的关联关系，因此我们绝不可忽视转换函数中 Key 的重要性。
