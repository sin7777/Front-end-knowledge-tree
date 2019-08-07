# webpack

* Entry：入口，Webpack 执行构建的第一步将从 Entry 开始，可抽象成输入。
* Module：模块，在 Webpack 里一切皆模块，一个模块对应着一个文件
* Webpack 会从配置的 Entry 开始递归找出所有依赖的模块。
* Chunk：代码块，一个 Chunk 由多个模块组合而成，用于代码合并与分割。
* Loader：模块转换器，用于把模块原内容按照需求转换成新内容。
* Plugin：扩展插件，在 Webpack 构建流程中的特定时机会广播出对应的事件，插件可以监听这些事件的发生，在特定时机做对应的事情

## webpack是什么

webpack是一个**模块打包工具**，可以使用它管理项目中的模块依赖，并编译输出模块所需的静态文件。它可以很好地管理、打包开发中所用到的HTML,CSS,JavaScript和静态文件（图片，字体）等，让开发更高效。对于不同类型的依赖，webpack有对应的模块加载器，而且会分析模块间的依赖关系，最后合并生成优化的静态资源。

## webpack的构建流程是什么

Webpack 的运行流程是一个串行的过程，从启动到结束会依次执行以下流程：

* 初始化参数：从配置文件和 Shell 语句中读取与合并参数，得出最终的参数；
* 开始编译：用上一步得到的参数初始化 Compiler 对象，加载所有配置的插件，执行对象的 run 方法开始执行编译；
* 确定入口：根据配置中的 entry 找出所有的入口文件；
* 编译模块：从入口文件出发，调用所有配置的 Loader 对模块进行翻译，再找出该模块依赖的模块，再递归本步骤直到所有入口依赖的文件都经过了本步骤的处理；
* 完成模块编译：在经过第4步使用 Loader 翻译完所有模块后，得到了每个模块被翻译后的最终内容以及它们之间的依赖关系；
* 输出资源：根据入口和模块之间的依赖关系，组装成一个个包含多个模块的 Chunk，再把每个 Chunk 转换成一个单独的文件加入到输出列表，这步是可以修改输出内容的最后机会；
* 输出完成：在确定好输出内容后，根据配置确定输出的路径和文件名，把文件内容写入到文件系统。

在整个流程中 webpack 会在合适的时机执行plugin里定义的逻辑

## webpack打包原理

将所有依赖打包成一个bundle.js，通过代码分割成单元片段按需加载？？？

## webpack的基本功能和工作原理

* 代码转换：TypeScript 编译成 JavaScript、SCSS 编译成 CSS 等等
* 文件优化：压缩 JavaScript、CSS、HTML 代码，压缩合并图片等
* 代码分割：提取多个页面的公共代码、提取首屏不需要执行部分的代码让其异步加载
* 模块合并：在采用模块化的项目有很多模块和文件，需要构建功能把模块分类合并成一个文件
* 自动刷新：监听本地源代码的变化，自动构建，刷新浏览器
* 代码校验：在代码被提交到仓库前需要检测代码是否符合规范，以及单元测试是否通过
* 自动发布：更新完代码后，自动构建出线上发布代码并传输给发布系统。

## Loader和Plugin的不同

* loader是用来告诉webpack如何转换某一类型的文件，并且引入到打包出的文件中。
* plugins(插件)作用更大，可以打包优化，资源管理和注入环境变量

不同的作用

* Loader直译为"加载器"。Webpack将一切**文件**视为模块，但是webpack原生是只能解析js文件，如果想将其他文件也打包的话，就会用到loader。 所以Loader的作用是让webpack拥有了加载和解析非JavaScript文件的能力。
* Plugin直译为"插件"。Plugin可以扩展webpack的功能，让webpack具有更多的灵活性。 在 Webpack 运行的生命周期中会广播出许多事件，Plugin 可以监听这些事件，在合适的时机通过 Webpack 提供的 API 改变输出结果。

不同的用法

* Loader在module.rules中配置，也就是说他作为模块的解析规则而存在。 类型为数组，每一项都是一个Object，里面描述了对于什么类型的文件（test），使用什么加载(loader)和使用的参数（options）
* Plugin在plugins中单独配置。 类型为数组，每一项是一个plugin的实例，参数都通过构造函数传入。

## 常见的Loader

* babel-loader：把 ES6/JSX 转换成 ES5
* css-loader：加载 CSS，支持模块化、压缩、文件导入等特性，解释@import url()
* style-loader：把 CSS 代码注入到 JavaScript 中，通过 DOM 操作去加载 CSS。
* eslint-loader：通过 ESLint 检查 JavaScript 代码
* file-loader：把文件输出到一个文件夹中，在代码中通过相对 URL 去引用输出的文件
* url-loader：和 file-loader 类似，但是能在文件很小的情况下以 base64 的方式把文件内容注入到代码中去
* source-map-loader：加载额外的 Source Map 文件，以方便断点调试
* image-loader：加载并且压缩图片文件

## 常见的plugin

* define-plugin：定义环境变量
* commons-chunk-plugin：提取公共代码
* uglifyjs-webpack-plugin：通过UglifyES压缩ES6代码（tree shaking)
* html-webpack-plugin 为html文件中引入的外部资源，可以生成创建html入口文件
* mini-css-extract-plugin：分离css文件
* clean-webpack-plugin：删除打包文件
* happypack：实现多线程加速编译

## webpack的热更新是如何做到的？说明其原理

webpack的热更新又称热替换（Hot Module Replacement），缩写为HMR。 这个机制可以做到不用刷新浏览器而将新变更的模块替换掉旧的模块。

webpack-dev-server 使用内存来存储webpack开发环境下的打包文件，并且可以使用模块热更新，相比传统http服务器开发更加简单高效

## webpack 怎样配置单页应用于多页应用

入口文件

* 单页应用 ——> 单个入口文件

```JS
module.exports = {
    entry: './path/to/my/entry/file.js'
}
```

* 多页应用 ——> 多个入口文件

```JS
module.entrys = {
    entry: {
        pageOne: './src/pageOne/index.js',
        pageTwo: './src/pageTwo/index.js'
    }
}
```

## 如何利用webpack来优化前端性能

* 压缩代码
  * 删除多余的代码、注释、简化代码的写法等等方式。
  * 可以利用webpack的UglifyJsPlugin和ParallelUglifyPlugin来压缩JS文件
  * 利用cssnano（css-loader?minimize）来压缩css
* 提取公共代码
* 利用CDN加速
  * 在构建过程中，将引用的静态资源路径修改为CDN上对应的路径。可以利用webpack对于output参数和各loader的publicPath参数来修改资源路径
* 删除死代码（Tree Shaking）
  * 将代码中永远不会走到的片段删除掉。可以通过在启动webpack时追加参数--optimize-minimize来实现

## 如何提高webpack的构建速度

* 使用Happypack 实现多线程加速编译
* 使用webpack-uglify-parallel来提升uglifyPlugin的压缩速度。 原理上webpack-uglify-parallel采用了多核并行压缩来提升压缩速度
* 使用Tree-shaking和Scope Hoisting来剔除多余代码
