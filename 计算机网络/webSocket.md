# WebSocket

WebSocket 是应用层的协议

WebSocket是基于TCP的一种新的网络协议，并在2011年被IETF定为标准的全双工通信协议，它实现了客户端与服务器**全双工通信**，多适用于服务器向客户端推送消息的场景，当然，客户端也可以像服务器推向消息。

WebSocket 也有 ws 和 wss（ws + 加密）的两种，分别的默认端口为80端口和443端口

## 协议过程

### 握手

ws 的握手使用 HTTP 协议已来实现，通过发送带有特殊的首部信息的GET请求来升级协议

```JS
GET /chat HTTP/1.1
Host: server.example.com
Upgrade: websocket           // Upgrade头
Connection: Upgrade          // 如果客户端和服务器之间是通过代理连接的，那么在发送这个握手消息之前首先要发送CONNECT消息来建立直接连接。
Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==    //发送给服务器使用（服务器会使用此字段组装成另一个key值放在握手返回信息里发送客户端）
Origin: http://example.com
Sec-WebSocket-Version: 13            //客户端支持的WS协议的版本列表

// 可能包含：
Sec-WebSocket-Protocol: chat, superchat     //客户端支持的子协议的列表
Sec-WebSocket-Extensions  // 客户端和服务器可以使用此header来进行一些功能的扩展。
```

如果服务器接受这个请求，就会返回以下信息。101状态码表示服务器收到了客户端切换协议的请求，并且同意切换到此协议。

```JS
HTTP/1.1 101 Switching Protocols //1
Upgrade: websocket. //2
Connection: Upgrade. //3
Sec-WebSocket-Accept: s3pPLMBiTxaQ9kYGzzhZRbK+xOo=  //4
Sec-WebSocket-Protocol: chat. //5
```

在客户端接收到Response握手消息之后要做的一些事情

* 如果返回的返回码不是101，则按照RFC2616进行处理。如果是101，进行下一步，开始解析header域，所有header域的值不区分大小写。
* 判断是否含有Upgrade头，且内容包含websocket。
* 判断是否含有Connection头，且内容包含Upgrade
* 判断是否含有Sec-WebSocket-Accept头
  * Sec-WebSocket-Accept头域，其内容的生成步骤：
    * 首先将Sec-WebSocket-Key的内容加上字符串258EAFA5-E914-47DA-95CA-C5AB0DC85B11（一个UUID）。  
    * 将#1中生成的字符串进行SHA1编码。
    * 将#2中生成的字符串进行Base64编码。
* 如果含有Sec-WebSocket-Extensions头，要判断是否之前的Request握手带有此内容，如果没有，则连接失败。
* 如果含有Sec-WebSocket-Protocol头，要判断是否之前的Request握手带有此协议，如果没有，则连接失败。

## 协议属性、事件和方法

属性

* Socket.readyState 表示连接状态
  * 0 - 表示连接尚未建立。
  * 1 - 表示连接已建立，可以进行通信。
  * 2 - 表示连接正在进行关闭。
  * 3 - 表示连接已经关闭或者连接不能打开。
* Socket.bufferedAmount  表示已被 send() 放入正在队列中等待传输，但是还没有发出的 UTF-8 文本字节数。

事件

* open：连接建立时触发，使用方式Socket.onopen。
* message：客户端接收服务端数据时触发，使用方式Socket.onmessage。
* error：通信发生错误时触发，使用方式Socket.onerror。
* close：连接关闭时触发，使用方式Socket.onclose。

方法

* Socket.send()：使用连接发送数据。
* Socket.close()：关闭连接。

## 协议的缺点

基于多线程或多进程的服务器无法适用于 WebSockets，因为它旨在打开连接，尽可能快地处理请求，然后关闭连接。任何实际的 WebSockets 服务器端实现都需要一个异步服务器。

如果你的 web 应用使用了代理服务器 Nginx，那么你还需要为 Nginx 做一些配置，使得它开启 WebSocket 代理功能。

长连接需要后端处理业务的代码更稳定（不要随便把进程和框架都crash掉），推送消息相对复杂一些。

## websoket和http1.1的长链接的区别

[详细讲解](https://www.zhihu.com/question/20215561/answer/40316953)

* 协议不同，是否持久化
* websoket一个request，可以有多个response，http1.1一个request一个response
* 在讲Websocket之前，我就顺带着讲下 long poll 和 ajax轮询 的原理。

> 从上面很容易看出来，不管怎么样，ajax 轮询和 long poll 这两种都是非常消耗资源的。
>
> ajax轮询 需要服务器有很快的处理速度和资源。（速度）
>
> long poll 需要有很高的并发，也就是说同时接待客户的能力。（场地大小）

* 缺点：浏览器兼容问题，安卓兼容问题，宽带和耗电量
* 断开：服务端发送 `Opcode == 8 关闭连接（控制帧）` 给客户端

## webSocket 与 Socket 的区别

* webSocket 是应用层协议，Socket 是端到端的一种通信，汉仪比较广泛
