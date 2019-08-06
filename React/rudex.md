# rudex

> [这个文章！](https://segmentfault.com/a/1190000015367584)

Redux的主要作用是管理程序状态的。

当今流行的前端框架，都是使用 MVVM 的设计模式，也就 Model，View，View-Model。框架承担了大部分 View-Model 的工作，我们只需要把 Model 和 View 的映射关系定义清楚就行。用公式描述就是View = Render(Model)。所以本质来说，用户看到的页面，是Model 在某个状态下的视觉呈现。

---
应用的state统一放在store里面维护，当需要修改state的时候，dispatch一个action给reducer，reducer算出新的state后，再将state发布给事先订阅的组件。

所有对状态的改变都需要dispatch一个action，通过追踪action，就能得出state的变化过程。整个数据流都是单向的，可检测的，可预测的。当然，另一个额外的好处是不再需要一层一层的传递props了，因为Redux内置了一个发布订阅模块。

![rudex](https://segmentfault.com/img/bVbnAc6?w=638&h=479)

* store: 存放state
* action：视图触发事件
* reducer：store的变化规则
