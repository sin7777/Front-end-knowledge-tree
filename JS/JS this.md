# this 的指向问题

>[解释以及面试题](https://segmentfault.com/a/1190000011194676#articleHeader6) *没有看完*

this 指向函数**运行**时所在的环境对象

可以改变this指向：

* new（new不太好说，别回答这个）
* call()
* aplly()
* bind()

## call() aplly() bind() 三者区别

> [概念和常用地方](https://segmentfault.com/a/1190000011389726)

不同之处：

* 语法：call接受两个参数、aplly接受多个参数、bind一个参数
* 调用：call() aplly()立即调用，bind()只绑定，不调用

常用来：

* 处理伪数组（借用数组方法）
* 实现继承
* 合并数组
