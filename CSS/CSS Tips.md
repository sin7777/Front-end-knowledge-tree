# Some Tips

## 相对于元素自身的css属性

* border-radius 的百分比
* img标签的 max-heigth 与 max-width
* transform的translate

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
