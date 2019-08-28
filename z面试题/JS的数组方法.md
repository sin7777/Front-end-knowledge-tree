# JS 常见数组方法

## sort

sort的实现原理

sort不传入函数的时候，先调用每个数组项的toString()转型方法，然后按照字符串Unicode编码顺序来对字符串进行排序。

不同浏览器使用的排序算法不同

* Google Chrome V8  插入排序和快速排序
* Mozilla Firefox  SpiderMonkey 归并排序
* Safari  Nitro（JavaScriptCore ）  归并排序和桶排序
* Microsoft Edge 和 IE(9+)  Chakra  快速排序

传入函数的时候，sort表现为一个高阶函数

sort()方法会直接对Array进行修改，它返回的结果仍是当前Array
