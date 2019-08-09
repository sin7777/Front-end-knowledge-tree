# React项目优化

## 代码体积优化

* 使用 生产版本（Production Build）：会移除脚本中不必要的警告和报错，减少文件体积
* 使用 webpack-bundle-analyzer 可视化 webpack 输出文件的大小
* 使用动态 import，懒加载 React 组件
* 使用 Tree Shaking 移除没有使用的代码
* 使用 babel-plugin-import 优化业务组件的引入，实现按需加载
* 使用 SplitChunksPlugin 拆分公共代码
* 优化 Webpack 中的库
* 分析 CSS 和 JS 代码覆盖率

### 使用动态 import，懒加载 React 组件

```JSX
//使用之前
import OtherComponent from './OtherComponent';
//使用之后
const OtherComponent = React.lazy(() => import('./OtherComponent'));
```

React.lazy 接受一个函数，这个函数需要动态调用 import()。它必须返回一个 Promise，该 Promise 需要 resolve 一个 defalut export 的 React 组件。

### 基于路由的代码分割

使用 React.lazy 和 React Router 这类的第三方库，来配置基于路由的代码分割。

```JSX
const Home = lazy(() => import('./routes/Home'));
const About = lazy(() => import('./routes/About'));
const App = () => (
  <Router>
    <Suspense fallback={<div>Loading...</div>}>
      <Switch>
        <Route exact path="/" component={Home}/>
        <Route path="/about" component={About}/>
      </Switch>
    </Suspense>
  </Router>
);
```

## 代码逻辑优化

* 利用 shouldComponentUpdate 和 PureComponent 避免过多 render function
  * 但存在浅层比较的问题，对复杂对象会不能比较差异，导致组件不渲染，可以用Object.assign 以及对象扩展运算符改变数据，或者Immutable.js 使用不可变数据结构
* 不要直接改变 setState 数据
* 虚拟化长列表 & 常用库 react-virtualized
* 合理组织 React Component：React Component Patterns
* 在希望发生重新渲染的 DOM 上设置同级唯一 key 以触发重新渲染
* 尽量将 props 和 state 摊平，只传递 component 需要的 props，慎将 component 当作 props 传入
  