# Some Tips

## CSS优先级

!important > 行内样式>ID选择器 > 类选择器 > 标签 > 通配符 > 继承 > 浏览器默认属性

## 相对于元素自身的css属性

* border-radius 的百分比
* img标签的 max-heigth 与 max-width
* transform的translate

## 相当于包含块的宽度

padding的百分比是相对于其包含块的宽度，而不是高度

## 实现一个三角形

把上、左、右三条边隐藏掉（颜色设为 transparent）

```CSS
#demo {
  width: 0;
  height: 0;
  border-width: 20px;
  border-style: solid;
  border-color: transparent transparent red transparent;
}
```

## 如何修改chrome记住密码后自动填充表单的黄色背景

```CSS
input:-webkit-autofill, textarea:-webkit-autofill, select:-webkit-autofill {
  background-color: rgb(250, 255, 189); /* #FAFFBD; */
  background-image: none;
  color: rgb(0, 0, 0);
}
```

## style标签写在body后与body前有什么区别

head 内的 JavaScript 需要执行结束才开始渲染 body，所以尽量不要将 JS 文件放在 head 内。可以选择在 document complete 时，或者特定区块后引入和执行 JavaScript。

而 CSS 应当写在 head 中，以避免页面元素由于样式确实造成瞬间的白页或者给用户闪烁感。

## css全局命名冲突

* 人为地创建组件的作用域
* CSS Modules
* scoped

## 如何实现物理 1px 效果（0.5px）

> 怎么在高清屏上画一条0.5px的边呢？0.5px相当于高清屏物理像素的1px。

直接设置 0.5px 会有兼容性的问题，安卓会把 0.5px 处理成 0

Chrome把0.5px四舍五入变成了1px，而firefox/safari能够画出半个像素的边，并且Chrome会把小于0.5px的当成0，而Firefox会把不小于0.55px当成1px，Safari是把不小于0.75px当成1px

* 缩放: transform: scaleY(0.5)

```HTML
<style>
.hr.scale-half {
  height: 1px;
  transform: scaleY(0.5);
  transform-origin: 50% 100%; /*直接上面变化会使线变虚，设置缩放中点*/
}
</style>

<div class="hr scale-half"></div>
```

* 图片: border-image

## scale

scale 对元素进行缩放

* scale(2) 放大一倍
* scale(0.5) 缩小一倍
* scale(-1) 整个颠倒

scale 和 zoom 的区别

都是用来放大缩小，中心点不同。scale中心，zoom左上

## CSS 用来隐藏元素

diaplay:none （页面布局发生变化）

overflow:hidden

区别是什么？ 重排 与 重绘

## 包含块

每个元素的盒子都是相对于它的包含块来布局

根据定位元素的不同：position 包含块也不同

static（默认）与relative：由**最近的块级**、单元格（td,th）、行内块创建（tr，em不属于）

fixed：包含块是**当前可视窗口（**viewport）

absolute：绝对定位元素的包含块由离它最近的 'position' 属性为 'absolute'、'relative' 或者 'fixed' 的祖先元素创建，也就是非static祖先元素

但是如果当这个祖先元素为行内元素的时候，包含块就要取决于祖先元素的direction元素了

> 以下来自[微信公总号](https://mp.weixin.qq.com/s/iD8rinWJ_PEI3UZu4-PcMg)

## 负边距的效果

margin-left 的效果是整体往左移

margin-right 的效果是向左拉

## input 的宽度默认宽度取决于size特性的值

并不是给元素设置display:block就会自动填充父元素宽度。input 就是个例外，其默认宽度取决于size特性的值

## 行内元素与块级元素的不同

* 块状元素占据一行，行内元素和其他元素在一行
* 块状元素能容纳其他块元素或者内联元素；内联元素，只能容纳文本或其他内联元素
* 块级元素可以通过盒模型来控制，行内元素的宽高不可控制

### inline、block、inline-block的区别

inline是内联元素，block是块级元素，inline-block是内联块元素

* inline：一个内联元素不会开始新的一行，并且只占有必要的宽度。
* block：一个块级元素总是开始新的一行，并且占据可获得的全部宽度(左右都会尽可能的延伸到它能延伸的最远)
  * p是块级元素，但其只能包含行内元素（待考察）
* inline-block：它像内联元素可以显示在同一行，但可以设置宽度和高度
  * img、input标签等效于这种内联块元素标签

## inline-block和float的区别

1. 文档流（Document flow）:浮动元素会脱离文档流，并使得周围元素环绕这个元素。而inline-block元素仍在文档流内。因此设置inline-block不需要清除浮动。当然，周围元素不会环绕这个元素，你也不可能通过清除inline-block就让一个元素跑到下面去。
2. 水平位置（Horizontal position）：很明显你不能通过给父元素设置text-align:center让浮动元素居中。事实上定位类属性设置到父元素上，均不会影响父元素内浮动的元素。但是父元素内元素如果设置了display：inline-block，则对父元素设置一些定位属性会影响到子元素。（这还是因为浮动元素脱离文档流的关系）。
3. 垂直对齐（Vertical alignment）：inline-block元素沿着默认的基线对齐。浮动元素紧贴顶部。你可以通过vertical属性设置这个默认基线，但对浮动元素这种方法就不行了。这也是我倾向于inline-block的主要原因。
4. 空白（Whitespace）：inline-block包含html空白节点。如果你的html中一系列元素每个元素之间都换行了，当你对这些元素设置inline-block时，这些元素之间就会出现空白。而浮动元素会忽略空白节点，互相紧贴.

## 实现图形 hover 触发以及离开之后动画

transform 和 transition 是什么神仙！

```CSS
/* 实现Y轴收缩 */
transform: translate(0px, -130px);
/* 动画过渡，第二个是过渡时间，最后一个是延迟时间 */
transition: all .2s ease-in 0s;
```

```HTML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>离开时效果生硬</title>
    <style type="text/css">
        div{
            width: 100px;
            height: 100px;
            border:1px solid;

            margin:0px auto;
            margin-top: 200px;

            /* 在原本元素上再加一个transition */
            transition: all 1s linear 2s;
        }
        div:hover{
            transform: scale(2);
            transition: all 1s linear;
        }
    </style>
</head>
<body>
    <div></div>
</body>
</html>
```
