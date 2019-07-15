# websoket和http1.1的长链接的区别

[详细讲解](https://www.zhihu.com/question/20215561/answer/40316953)

* 协议不同，是否持久化
* websoket一个request，可以有多个response，http1.1一个request一个response
* 在讲Websocket之前，我就顺带着讲下 long poll 和 ajax轮询 的原理。

> 从上面很容易看出来，不管怎么样，上面这两种都是非常消耗资源的。
>
> ajax轮询 需要服务器有很快的处理速度和资源。（速度）
>
> long poll 需要有很高的并发，也就是说同时接待客户的能力。（场地大小）

* 缺点：浏览器兼容问题，安卓兼容问题，宽带和耗电量
* 断开：服务端发送0x8的控制帧给客户端
