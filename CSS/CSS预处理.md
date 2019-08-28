# CSS 的预处理

常见的 CSS 预处理器有

* Sass
* LESS
* Stylus
* PostCSS

sass和less主要区别：在于实现方式 less是基于JavaScript的在客户端处理 所以安装的时候用npm，sass是基于ruby所以在服务器处理。

## Sass

Sass有两种语法，分别以「 *.sass 」和「 *.scss 」为扩展名。

* 变量
* 嵌套计算
* 继承
* 重用代码块
* 条件语句、循环语句
* 自定义函数

## Less

Less 受 Sass 的影响非常大，以「 *.less 」为扩展名，是 sass 之后的又一款优秀的 css 预处理器。其特点包括：

* 变量：就像写其他语言一样，免于多处修改。
* 混合：class 之间的轻松引入和继承。
* 嵌套：选择器之间的嵌套使你的 less 非常简洁。
* 函数&运算：就像 js 一样，对 less 变量的操控更灵活。

```less
@base: #f938ab;

.box-shadow(@style, @c) when (iscolor(@c)) {
  box-shadow:         @style @c;
  -webkit-box-shadow: @style @c;
  -moz-box-shadow:    @style @c;
}
.box-shadow(@style, @alpha: 50%) when (isnumber(@alpha)) {
  .box-shadow(@style, rgba(0, 0, 0, @alpha));
}
.box {
  color: saturate(@base, 5%);
  border-color: lighten(@base, 30%);
  div { .box-shadow(0 0 5px, 30%) }
}
```

## Stylus

## PostCSS（插件系统）

PostCSS是一款使用插件去转换CSS的工具。

PostCSS 提供了一个解析器，它能够将 CSS 解析成抽象语法树（AST）。

常见插件

* Autoprefixer 是一个流行的 PostCSS 插件，其作用是为 CSS 中的属性添加浏览器特定的前缀
* postcss-stylelint 用来检查 CSS 中可能存在的格式问题
* cssnano 用来压缩 CSS 代码
* postCss-moudels 模块化 CSS
