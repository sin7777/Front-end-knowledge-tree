# Some Tips

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

## 如何实现物理 1px 效果

* 兼容问题
* transform:scale(0.5);
* 图片，border-image
  
```HTML
<div style="height:1px;overflow:hidden;background:red"></div>
```

## CSS用来隐藏元素

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
