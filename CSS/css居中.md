# 如何实现元素居中

## margin：auto属性

元素水平居中：

```CSS
div{
    width:200px;
    margin:0 auto;
}
```

## 行内元素居中

* 水平居中：text-align:center
* 垂直居中：line-height = 父元素的height

## 通过绝对定位的偏移属性

> 下面会给详细例子

* 配合translate()位移函数
* 配合relative
* 配合负margin

## flex布局

* flex-direction   项目的排列方向
* flex-wrap   如果一条轴线排不下，如何换行
* flex-flow   flex-direction属性和flex-wrap属性的简写形式
* justify-content   项目在主轴上的对齐方式，center： 居中
* align-items
* align-content

## 栅格布局grid

> 不详细展开

## 固定宽高元素居中的方法

### absolute + 负margin

```CSS
.wp {
    position: relative;
    height: 400px;
}
.box {
    position: absolute;
    width: 100px;
    height: 100px;
    top: 50%;
    left: 50%;
    margin-left: -50px;
    margin-top: -50px;
}
```

### absolute + margin auto

```CSS
.wp {
    position: relative;
    height: 400px;
}
.box {
    position: absolute;
    width: 100px;
    height: 100px;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    margin: auto;
}
```

### absolute + calc

```CSS
.wp {
    position: relative;
    height: 400px;
}
.box {
    position: absolute;
    width: 100px;
    height: 100px;
    top: calc(50% - 50px);
    left: calc(50% - 50px);
}
```

## 不定宽高元素居中的方法

### absolute + transform

```CSS
.wp {
    position: relative;
}
.box {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
}
```

### flex

```CSS
.box{
    display: flex;
    justify-content: center;
    align-items: center;
}
```

### grid

```CSS
.wp {
    display: grid;
}
.box {
    align-self: center;
    justify-self: center;
}
```
