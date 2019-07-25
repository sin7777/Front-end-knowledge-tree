# React的设计思路

> 在 React 中，界面完全由数据驱动；
>
> 在 React 中，一切都是组件；
>
> props 是 React 组件之间通讯的基本方式。

**自动化的 UI 状态管理**：只需关注UI所处的最终状态

**虚拟DOM 操作**：React 通过比较虚拟 DOM 和真实 DOM 之间的差别，查明哪个改变很重要，然后在一个称为 Reconciliation 的过程中作出最少量的 DOM 改变，以确保一切保持最新。

**多组件**：用来创建真正可组合 UI 的 API

**JSX语法**：完全在 JavaScript 中定义 UI

**只关心View**：只是 MVC 架构中的 V
