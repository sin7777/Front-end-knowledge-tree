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

## link和a和import的区别

* a标签属于超链接,用来URL定向的;
* link标签是用来连接文件的,一般用于CSS文件的引入
* link引用CSS时，在页面载入时同时加载；@import需要页面网页完全载入以后加载

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

## getElementsBy 和 querySelectorAll 的区别

querySelectorAll 方法接收的参数是一个 CSS 选择符。而 getElementsBy 系列接收的参数只能是单一的className、tagName 和 name。

querySelectorAll 返回的是一个 **Static** Node List，而 getElementsBy 系列的返回的是一个 **Live** Node List。

## 严格模式和混杂模式

<!DOCTYPE> 声明位于文档中的最前面的位置，处于 `<html>` 标签之前。此标签可告知浏览器文档使用哪种 HTML 或 XHTML 规范。

1. 所谓的**标准模式**是指，浏览器按 W3C 标准解析执行代码；**怪异模式**则是使用浏览器自己的方式解析执行代码，因为不同浏览器解析执行的方式不一样，所以我们称之为怪异模式。
2. 浏览器解析时到底使用标准模式还是怪异模式，与你网页中的 DTD 声明直接相关， DTD 声明定义了标准文档的类型（标准模式解析）文档类型，会使浏览器使用相应的方式加载网页并显示，忽略 DTD 声明 , 将使网页进入怪异模式。

严格模式和混杂模式的差异

* 盒模型
  * 标准模式采用 `content-box`
  * 怪异模式采用 `border-box`
* 可以设置行内元素的高宽
  * 怪异模式的行内元素可以设置宽高
* 可设置百分比的高度
  * 在standards模式下，一个元素的高度是由其包含的内容来决定的，如果父元素没有设置高度，子元素设置一个百分比的高度是无效的
* 用margin:0 auto设置水平居中在IE下会失效
  * 使用margin:0 auto在standards模式下可以使元素水平居中，但在quirks模式下却会失效,quirk模式下的解决办法，用text-align属性
* quirk模式下设置图片的padding会失效
* quirk模式下Table中的字体属性不能继承上层的设置
* quirk模式下white-space:pre会失效
