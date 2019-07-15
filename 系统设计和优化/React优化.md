# React项目优化

## 代码体积优化

* 使用 生产版本
* 使用 webpack-bundle-analyzer 可视化 webpack 输出文件的大小
* 使用动态 import，懒加载 React 组件
* 使用 Tree Shaking & 教程 & Tree Shaking 优化
* 使用 babel-plugin-import 优化业务组件的引入，实现按需加载
* 使用 SplitChunksPlugin 拆分公共代码
* 优化 Webpack 中的库
* 分析 CSS 和 JS 代码覆盖率

## 代码逻辑优化

* 利用 shouldComponentUpdate 和 PureComponent 避免过多 render function
* 不要直接改变 setState 数据
* 虚拟化长列表 & 常用库 react-virtualized
* 合理组织 React Component：React Component Patterns
* 在希望发生重新渲染的 DOM 上设置同级唯一 key 以触发重新渲染
* 尽量将 props 和 state 摊平，只传递 component 需要的 props，慎将 component 当作 props 传入
  