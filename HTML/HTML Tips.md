# 关于HTML的其他知识

## document.ready 和 window.onload的区别

页面加载完成有两种时间，ready比onload更先执行

* ready事件

ready事件在DOM结构绘制完成之后就会执行，这样能确保就算有大量的媒体文件没加载出来，JS代码一样可以执行。

* onload事件

load事件必须等到网页中所有内容全部加载完毕之后才被执行。如果一个网页中有大量的图片的话，则就会出现这种情况：网页文档已经呈现出来，但由于网页数据还没有完全加载完毕，导致load事件不能够即时被触发。

## Load 和 DOMContentLoaded 区别

Load 事件触发代表页面中的 DOM，CSS，JS，图片已经全部加载完毕。

DOMContentLoaded 事件触发代表初始的 HTML 被完全加载和解析，不需要等待 CSS，JS，图片加载。

## 如何实现浏览器内多个标签页之间的通信

WebSocket、SharedWorker；

也可以调用localstorge、cookies等本地存储方式；

localstorge另一个浏览上下文里被添加、修改或删除时，它都会触发一个事件，我们通过监听事件，控制它的值来进行页面信息通信；

## 页面引入 CSS 的方式

* 行内式：行内设置 CSS
* 内嵌式：通过 `<style>` 标签设置
* 导入式：@import 导入
* 链接式：link `<link href="mystyle.css" rel="stylesheet" type="text/css"/>`导入

## link & import的区别

* link 属于 XHTML 标签，除了加载 CSS 外，还能用于定义 RSS, 定义 rel 连接属性等作用；而 @import 是CSS提供的，只能用于加载 CSS;
* 页面被加载的时，link会同时被加载，而 @import 引用的 CSS 会等到页面被加载完再加载;
* link支持使用 js 控制 DOM 去改变样式，而 @import 不支持;

## HTML 语义化的作用是什么

* 保证即使在没有 CSS 的情况下仍然可以呈现页面
* 代码可读性更高
* 便于 SEO 搜索
* 为`无障碍辅助功能网络应用`提供基础

列举一些语义化的元素

* header 元素
* footer 元素
* nav 元素 代表页面的导航链接区域。用于定义页面的主要导航部分
* article 元素
* section 元素
* aside元素
