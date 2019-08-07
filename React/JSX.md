# JSX

## 什么是 JSX

React 官网中介绍：JSX 仅仅只是 React.createElement(component, props, ...children) 函数的语法糖

## 为什么用户定义的组件必须以大写字母开头

以小写字母开头的元素代表一个 HTML 内置组件，比如 `<div>` 或者 `<span>` 会生成相应的字符串 'div' 或者 'span' 传递给 React.createElement（作为参数）。大写字母开头的元素则对应着在 JavaScript 引入或自定义的组件，如 `<Foo />` 会编译为 React.createElement(Foo)

## 为什么甚至在我们的代码并没有使用 React 的情况下，也需要在文件顶部 import React from 'react'

由于 JSX 会编译为 React.createElement 调用形式，所以 React 库也必须包含在 JSX 代码作用域内。

## JSX 与 单入口

当使用 render 和 return 函数时，最终返回的是单个 createElement 调用。由于 createElement 的机制，只能指定一个返回一个根元素的。

## JSX 被转译成 HTML 和 JS 的两种方法

围绕 Node 以及一些构建工具（比如 Webpack）来设置开发环境。在这种环境中，每次执行构建时，所有 JSX 被自动转换为 JS，放在磁盘上，让我们可以像标准 JavaScript 文件一样引用。

让浏览器在运行时自动将 JSX 转换为 JavaScript。我们直接像 JavaScript 一样用 JSX，浏览器负责剩下的转换。——需要引入babel
