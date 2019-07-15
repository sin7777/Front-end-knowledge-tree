# JSX

## JSX被转译成 HTML 和 JS 的两种方法

围绕 Node 以及一些构建工具（比如 Webpack）来设置开发环境。在这种环境中，每次执行构建时，所有 JSX 被自动转换为 JS，放在磁盘上，让我们可以像标准 JavaScript 文件一样引用。

让浏览器在运行时自动将 JSX 转换为 JavaScript。我们直接像 JavaScript 一样用 JSX，浏览器负责剩下的转换。——需要引入babel
