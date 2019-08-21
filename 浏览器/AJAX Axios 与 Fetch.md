# AJAX Axios 与 Fecth

axios,fetch请求后都支持 Promise 对象 API，ajax 只能用回调函数

```JS
//使用XHR发送请求：
var xhr = new XMLHttpRequest(); //创建XML
xhr.open('GET', url);
xhr.responseType = 'json';
xhr.onload = function() {  //通过回调获取响应信息
    console.log(xhr.response);
};
xhr.onerror = function() {
    console.log("Oops, error");
};
xhr.send(); //发送请求
```

## [AJAX](https://juejin.im/post/58c883ecb123db005311861a)

> [讲解](https://zhuanlan.zhihu.com/p/38067984)

Ajax（Asynchronous JavaScript and XML的缩写）是一种异步请求数据的web开发技术，对于改善用户的体验和页面性能很有帮助。

简单地说，在不需要重新刷新页面的情况下，Ajax 通过异步请求加载后台数据，并在网页上呈现出来。常见运用场景有表单验证是否登入成功、百度搜索下拉框提示和快递单号查询等等。

![AJAX](https://pic3.zhimg.com/80/v2-75bf034c1cb2e15781d809372646784a_hd.jpg)

通常, 一个xhr实例对象拥有 10 个普通属性和 9 个方法。

### readyState

**只读属性**, readyState属性记录了ajax调用过程中所有可能的状态. 它的取值简单明了, 如下:

* 0 (未初始化)，xhr.UNSENT，请求已建立, 但未初始化(此时未调用open方法)
* 1 (初始化)，xhr.OPENED，请求已建立, 但未发送 (已调用open方法, 但未调用send方法)
* 2 (发送数据)，xhr.HEADERS_RECEIVED，请求已发送 (send方法已调用, 已收到响应头)
* 3 (数据传送中)，xhr.LOADING，请求处理中, 因响应内容不全, 这时通过responseBody和responseText获取可能会出现错误
* 4 (完成)，xhr.DONE，数据接收完毕, 此时可以通过通过responseBody和responseText获取完整的响应数据

### onreadystatechange

onreadystatechange事件回调方法在readystate状态改变时触发, 在一个收到响应的ajax请求周期中, onreadystatechange 方法会被触发4次.

```JS
xhr.onreadystatechange = function(e){
  //TODO ...
}
```

### status

只读属性, status表示http请求的状态, 初始值为0. 如果服务器没有显式地指定状态码, 那么status将被设置为默认值, 即200.

### statusText

只读属性, statusText表示服务器的响应状态信息, 它是一个 UTF-16 的字符串, 请求成功且status==20X时, 返回大写的 OK . 请求失败时返回空字符串. 其他情况下返回相应的状态描述. 比如: 301的 Moved Permanently , 302的 Found , 303的 See Other , 307 的 Temporary Redirect , 400的 Bad Request , 401的 Unauthorized 等等.

### onload

onload事件回调方法在ajax请求成功后触发, 触发时机在 readyState==4 状态之后.

### abort

abort方法用于取消ajax请求, 取消后, readyState 状态将被设置为 0 (UNSENT). 如下, 调用abort 方法后, 请求将被取消.

### onerror

onerror方法用于在ajax请求出错后执行. 通常只在网络出现问题时或者ERR_CONNECTION_RESET时触发(如果请求返回的是407状态码, chrome下也会触发onerror).

## Axios

Axios本质上也是对原生 XHR 的封装，只不过它是 Promise 的实现版本，符合最新的ES规范

特点:

* 基于 promise 的 http 库
* 支持Promise所有 API
* 安全性更高, 客户端支持防御 CSRF（怎么防护？）
  * 让你的每个请求都带一个**从 cookie 中拿到的 key**, 根据浏览器同源策略，假冒的网站是拿不到你 cookie 中得 key 的，这样，后台就可以轻松辨别出这个请求是否是用户在假冒网站上的误导输入，从而采取正确的策略。
* 可以转换请求数据和响应数据,并对响应回来的内容自动转换成JSON类型的数据
* 可以拦截请求和响应

## Fetch

Fetch API优点:

* 语法简洁,更加语义化
* 基于标准 Promise 实现,支持 async,await
* 更加底层，提供的 API 丰富（request, response）
* 脱离了 XHR，是 ES 规范里新的实现方式

Fetch 的问题

1. Fetch接收到错误状态码『404, 500 ...』时候, 返回的 Promise 状态为 resolve『完成状态』，只有在网络故障或者请求被阻止『跨域』才是 reject『拒绝状态』
2. fetch 默认不会带 cookie，需要设置 credentials 才能从服务端发送或接收任何 cookies
3. fetch 不支持 abort，不支持超时控制，使用 setTimeout 及Promise.reject 的实现的超时控制并不能阻止请求过程继续在后台运行，造成了流量的浪费
4. fetch 没有办法原生监测请求的进度，而 XHR 可以

```JS
fetch(url).then(
  response => response.json() //请求成功信息
).then(
data => console.log(data) //获取请求失败信息
).catch(
e => console.log("Oops, error", e) //捕获异常
)
```

## GET 请求和POST请求的区别

用法上的区别

* 请求的数据放在什么地方？GET 参数通过 URL 传递，POST 放在 Request body 中。
* GET 产生的URL地址可以被 Bookmark，而 POST 不可以。
* GET 请求会被浏览器主动 cache，而POST不会，除非手动设置。
* GET 请求只能进行 url 编码，而 POST 支持多种编码方式。
* GET 请求参数会被完整保留在浏览器历史记录里，而POST中的参数不会被保留。
* GET 请求在 URL 中传送的参数是有长度限制的，而POST没有。（由浏览器规定）
* GET 比 POST 更不安全，因为参数直接暴露在URL上，所以不能用来传递敏感信息。

本质区别？？？

GET和POST最大的区别主要是GET请求是幂等性的，POST请求不是

HTTP 幂等方法，是指无论调用这个 url 多少次，都会**返回相同的结果**的HTTP 方法。也就是不管你调用 1 次还是调用 100 次，1000 次，结果都是一样的（前提是服务器端的数据没有被人为手动更改。比如说，你数据库中的数据被手动更改过，那两次调用的结果肯定是变化的）

GET、PUT 和 DELETE都是幂等的
