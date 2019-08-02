# Img标签

动态创建

```js
//图像 ping
let img = new Image();
img.src =""
img.onload = function(){}
img.onerrer = function(){}
img.onabsort = function(){}

//jsonp
let scriptElement = document.createElement("script")
scriptElement.src = ""
document.body.insertBefore(scriptElement,document.body.firstChild)
```

## img标签可实现跨域

通过src可以指定域名，crossorigin的属性来制定是否带上证书等，Jsonp的实现原理类似

## height与width属性

img 标签是可替换元素，类似于inline-block

行内元素 + 不可替换元素  --> 不可设置宽高
行内元素 + 可替换元素   --> 可设置宽高

常见的可替换元素

* img
* imput
* textarea
* select
