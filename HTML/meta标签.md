# meta标签

> [详细](https://zhuanlan.zhihu.com/p/56233041)
>
> [MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/meta)

meta标签分两大部分：

## HTTP-EQUIV变量

expires/Pragma(cach模式)/Refresh(刷新)/Set-Cookie(cookie设定)/Window-target(显示窗口的设定)/Content-Type(显示字符集的设定)

## NAME变量

Keywords(关键字)/description(简介)/robots(机器人向导)/author(作者)
常用的meta头

```HTML
<!-- 字体编码 --> <meta charset="utf-8" />
<!-- 关键字 --> <meta name="keywords" content="" />
<!-- 说明 --> <meta name="description" conten="" />
<!-- 作者 --> <meta name="author" content="" />
<!-- 设置文档宽度、是否缩放 --> <meta name="viewport" content="width=device-width,initial-scale=1.0,user-scalable=no" />
<!-- 优先使用IE最新版本或chrome --> <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />
<!-- 360读取到这个标签立即换到极速模式 --> <meta name="renderer" content="webkit" />
<!-- UC强制竖屏 --> <meta name="screen-orientation" content="portrait" />
```
