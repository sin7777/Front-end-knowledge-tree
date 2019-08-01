# 客户端存储

* Cookie——服务器通信
* localstorage
* sessionStorage

特性 | Cookie | localStorage | sessionStorage
---|---|---|---
数据生命周期 | 可设置失效时间，默认是关闭浏览器后失效 | 除非被清除，否则永久保存 | 仅在当前会话下有效，关闭页面或浏览器后被清除
存放数据大小 | 4k | 5MB | 5MB
与服务器端的通信| 每次都会携带在HTTP头中，如果使用cookie保存过多数据会带来性能问题 | 仅在客户端（即浏览器）中保存，不参与和服务器的通信 | 仅在客户端（即浏览器）中保存，不参与和服务器的通信
易用性| 需要程序员自己封装，源生的Cookie接口不友好 | 源生接口可以接受，亦可再次封装来对Object和Array有更好的支持 | 源生接口可以接受，亦可再次封装来对Object和Array有更好的支持
使用场景| 判断用户是否登录 | 管理购物车 | 内容特别多的表单，表单页面拆分成多个子页面，然后按步骤引导用户填写

使用它们的时候，需要时刻注意是否有代码存在 XSS 注入的风险。因为只要打开控制台，你就随意修改它们的值，也就是说如果你的网站中有 XSS 的风险，它们就能对你的 localStorage 肆意妄为。所以千万不要用它们存储你系统中的敏感数据

```JS
// localStorage和sessionStorage所使用的方法是一样的，下面以sessionStorage为栗子：
var name='sessionData';
var num=120;
//存储数据 sessionStorage.setItem(name,num);
sessionStorage.setItem('value2',119);
let dataAll=sessionStorage.valueOf();  //获取全部数据
var dataSession=sessionStorage.getItem(name); //获取指定键名数据
var dataSession2=sessionStorage.sessionData;//sessionStorage是js对象，也可以使用key的方式来获取值
sessionStorage.removeItem(name); //删除指定键名数据
sessionStorage.clear();//清空缓存数据：localStorage.clear();
```

## Cookie，Session，Token

> [详解 Cookie，Session，Token](https://juejin.im/post/5d01f82cf265da1b67210869)

Cookie 相关设置

属性|作用
---|---
value|如果用于保存用户登录态，应该将该值加密，不能使用明文的用户标识
http-only|不能通过 JS 访问 Cookie，减少 XSS 攻击
secure|只能在协议为 HTTPS 的请求中携带
same-site|规定浏览器不能在跨域请求中携带 Cookie，减少 CSRF 攻击

### Cookie 和 session

cookie 是一种客户端机制，打开网站时，服务器会先判断是否有上次留下的 cookie，如果有，则根据 cookie 来识别客户端。

cookie**由服务器生成，发送给浏览器**，浏览器把cookie以KV形式存储到某个目录下的文本文件中，下一次请求同一网站时会把该cookie发送给服务器。由于cookie是存在客户端上的，所以浏览器加入了一些限制确保cookie不会被恶意使用，同时不会占据太多磁盘空间。所以每个域的cookie数量是有限制的

cookie 的主要内容包括：名字、值、国企时间、路径和域

不设置过期时间，则 cookie 的有效期 = 浏览器的会话时间，关闭浏览器，cookie 失效

session是一种服务器端机制，通过散列表的结构保存信息。

1. 用户向服务器发送用户名和密码
2. 服务器验证通过后,在当前对话(session)里面保存相关数据,比如用户角色, 登陆时间等;
3. 服务器向用户返回一个session_id, 写入用户的cookie
4. 用户随后的每一次请求, 都会通过cookie, 将session_id传回服务器
5. 服务端收到 session_id, 找到前期保存的数据, 由此得知用户的身份

### Cookies 与 Token

HTTP 协议是无状态的协议，但是服务器需要区分不同的用户及其权限

需要机制为HTTP提供状态

* Cookie
* Token

Token(JWT)

1. 用户通过用户名和密码发送请求
2. 程序验证
3. 程序返回一个签名的token给客户端
4. 客户端储存token, 并且每次用每次发送请求
5. 服务端验证Token并返回数据
