---
title: CSS 问题
---


## 目录
- [常用http请求。](#盒子模型)
- [get和post区别。](#get和post区别)
- [常用状态码。](#常用状态码)
- [tcp和udp区别。](#tcp和udp区别)
- [三次握手四次挥手。](#三次握手四次挥手)
- [聊一聊跨域。](#聊一聊跨域)
- [浏览器缓存机制，强制，协商缓存。](#浏览器缓存机制强制协商缓存)
- [cookie, session, localStorage。](#cookiesessionlocalStorage)
- [URL输入到渲染全过程。](#URL输入到渲染全过程)
- [ssr,spa,seo。](#ssrspaseo)


### 常用http请求。

- get，获取资源
- post，添加新的资源
- put，修改某个资源
- delete，删除某个资源
- connect，用代理进行传输，SSL
- options，询问可以执行哪些方法
- trace，用于远程诊断服务器

[[↑] 回到顶部](#目录)


### get和post区别。

***非ajax的HTTP请求***

- get
    - 指读取一个资源，比如get一个html文件，反复读取不应该对访问的数据有副作用（当然，服务器可以写成有副作用的），因为get是读取，所以缓存到浏览器本身。
- post
    - <form>表单中的，往往对访问的数据有副作用（服务器也可以写成无副作用的），所以无法在浏览器缓存，无法保存为书签。
- 区别
    - get可以缓存，post无法缓存。刷新页面，可以看到post请求返回始终是200，而get请求成304。
    - get可以保存书签，post无法保存书签。
    - get发送参数需要在url后进行拼接，post的参数在body中，但这是浏览器限制的，http协议本身并没做这个限制。

***接口中的HTTP请求***

这里是指通过Ajax，api，或者IOS/Android的App的http，client，java的commons-httpclient/okhttp或者curl，postman之类的工具发出的get和post请求。这时候不光能用在前后端交互，也可以用在后端之间的调用。不通过浏览器发送的请求，就没有浏览器那么多限制，例如可以在get请求中添加请求体，也可以在post请求中拼接querystring。如在后台之间互相请求，
// 请求服务器A
```js
request({
url: `http://127.0.0.1:3000/test`,
method: `get`,
json: true,
body: {msg: `来自服务器A的请求体`},
}, (err, response, body) => {
console.log( body);
});
```
// 接收服务器接口B
```js
app.get('/test', (req, res) => {
console.log(req.body); // {msg: `来自服务器A的请求体`},
res.send(`get`)
});
```
关于安全性：http都没有提供端到端的加密，以明文的形式才网络上进行传输，中间过程存在大量的节点，包括网关，代理，都可以获取信息并进行篡改。

###### 参考

- https://www.zhihu.com/question/28586791

[[↑] 回到顶部](#目录)


### 常用请求状态码。

***1xx临时相应***

- 100：服务已经收到请求，等待剩余部分
- 101：切换协议

***2xx成功状态***

- 200：请求成功
- 204：无内容，服务器成功处理请求，但没有返回内容

***3xx重定向***

- 301：永久移动
- 302：暂时移动
- 304：未修改
- 305：使用代理

***4xx客户端错误***

- 400：服务器不理解请求的语法
- 401：未授权，认证鉴权
- 403：禁止访问
- 404：未找到请求地址
- 405：方法禁用，禁用请求中指定的方法

***5xx服务器错误***

- 500：服务器内部错误
- 503：服务器不可用，服务器目前无法使用（超载或停机维护）。

###### 参考

- https://www.jianshu.com/p/f637f60e4c5a


[[↑] 回到顶部](#目录)


### 常用状态码。

tcp需要事先进行连接，udp无连接。tcp保证数据正确性，udp可能丢包，虽然 UDP 并没有 TCP 传输来的准确，但是也能在很多实时性要求高的地方有所作为。对数据准确性要求高，速度可以相对较慢的，可以选用TCP。

###### 参考

https://zhuanlan.zhihu.com/p/60017840


[[↑] 回到顶部](#目录)


### 三次握手四次挥手。

***三次握手***

（1）客户端向服务器端发送SYN，表示请求建立连接
（2）服务器端在收到后，发送SYN和ACK给客户端，表示服务器端可以正常接收到客户端数据，但此时服务器端还不确定自己能否将数据发送到客户端，为了安全可靠，所以有第三次握手
（3）客户端发送ACK给服务器端，表示自己可以接收来自服务器端的信息。服务器端第二握手发送完请求后就直接建立起连接的话，并开始发送数据，可能会造成客户端无法收到数据。
tcp是一种可靠的连接，根据谢希仁《计算机网络》中的解释，如果第一次握手请求时长较长，客户端因长时间滞留而导致释放连接，服务器端确认接受到这个滞留信息后，直接建立连接，等到客户端发送数据，而此时客户端已经断开，会造成服务器端资源浪费。

***四次挥手***

（1）客户端发送FIN给服务器端
（2）服务器端发送ACK给客户端，表示自己知道客户端要求断开连接，但此时可能还有数据没有传送完毕。
（3）当数据传送完毕时，服务器FIN和ACK给客户端。
（4）客户端收到后，发送ACK给服务器端，客户端等待一段事件后断开连接。服务器端在收到ACK之后，断开连接。

###### 参考

- https://www.zhihu.com/question/24853633
- https://www.jianshu.com/p/b64a1231af05


[[↑] 回到顶部](#目录)


### 聊一聊跨域。

由于浏览器同源策略限制的一类请求场景

***同源策略***

同源策略/SOP（Same origin policy），它是浏览器最核心的也是最基本的安全功能，同源是指“协议，域名，端口”三者相同。

***jsonp***

通过动态创建script标签，再请求一个带参网址实现跨域通信。后台直接返回一段js逻辑，在script标签中执行。一般场景下后台会返回一个执行函数。
```js
// 前台
var script = document.createElement('script');
script.type = 'text/javascript';
script.src = 'http://localhost:8080/getJsonp';
document.head.appendChild(script);
funciton handleCallback(res){
    console.log(res); // '{data: 1}'
}
// 后台返回
handleCallback({data: 1})
```
***cors***

普通跨域请求：只服务端设置Access-Control-Allow-Origin即可，类似一个白名单的方式，告诉浏览器，服务允许该域下获取请求的结果。

***完整样例***

- 浏览器端
```js
document.cookie = "tasd=2";
let xhr = new XMLHttpRequest();
// 发送cookie
xhr.withCredentials = true;
xhr.open(`post`, `http://127.0.0.1:3000/test`);
xhr.setRequestHeader('X-Custom-Header', 'value');
xhr.onload = function(){
    console.log(this.responseText);
};
xhr.send();
```
- 服务器端
```js
app.get('/test', (req, res) => {
    res.setHeader('Access-Control-Allow-Origin', 'http://localhost:8081'); //设置返回头
    res.setHeader('Access-Control-Allow-Methods', 'PUT, DELETE');
    res.setHeader('Access-Control-Allow-Headers', 'X-Custom-Header');
    res.setHeader('Access-Control-Allow-Credentials', 'true');
    res.send(`handleCallback({good: 1})`)
});
```

***Access-Control-Allow-Origin***

响应头指定了该响应的资源是否被允许与给定的origin共享.

***Access-Control-Allow-Methods***

对于简单请求（如GET,POST），并不涉及该字段，对于非简单请求（对服务器有特殊要求的请求，如PUT,DELETE或Content-Type是application/json），会在正式通信之前，增加一次HTTP查询请求，成为preflight。浏览器会先询问服务器，当前网页所在域名是否在服务器的许可名单之中，以及可以使用哪些HTTP动词和头信息字段，只有得到肯定答复，浏览器才会发出正式的XMLHttpRequest请求，否则报错。所以对于非简单请求，相应方需要添加头信息Access-Control-Allow-Methods。

***Access-Control-Allow-Headers***

当浏览器CORS请回发送额外的头信息字段时，需要返回该字段，如上述res填写的该字段是因为，浏览器发送了，res返回了
```js
xhr.setRequestHeader('X-Custom-Header', 'value');
```

***Access-Control-Allow-Credentials'***

可选值，boolean值，表示是否允许发送Cookie。默认情况下，Cookie不包括在CORS请求之中，该字段用于cors跨域发送和修改cookie，需要注意的是，这时候Access-Control-Allow-Origin不能填写'*'。
- 浏览器cors发送cookie（浏览器默认不发送）
```js
xhr.withCredentials = true;
```
- 服务器端允许接收cookie
```js
res.setHeader('Access-Control-Allow-Credentials', true);
```

###### 参考

- https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Access-Control-Allow-Origin
- https://segmentfault.com/a/1190000011145364


[[↑] 回到顶部](#目录)


### 浏览器缓存机制，强制，协商缓存。

***强制缓存***

当浏览器第一次发送请求时，服务器端返回cache-contorl或者expires时，下次请求便不会走到服务器端，直接从disk中获取。需要注意的是，如果客户端请求时将max-age=0传到服务器，将不进行强制缓存。
cache-control：
- public: 代理服务器和客户端都可以缓存
- private: 只有发起请求的浏览器可以进行缓存
- no-cahce: 不进行强制缓存
- no-store: 不进行任何缓存
- max-age: 缓存周期，单位是秒

expires：是一个日期值，`Tue Aug 25 2020 17:28:14 GMT`，如果同时设置cache-control和expires，已cache-control为的缓存时期为准。

***协商缓存***

- 发请求
    - 过期200，返回最新资源
    - 没过期304，客户端用缓存的劳资源
- 注意事项
    - 如果服务器返回304，那么即使服务器返回了新的数据，浏览器依旧会使用老数据。如果同时设置，两者必须同时满足，查看过期的头：
- 头字段
    - etag（if-none-match，客户端），文件hash，
    - last-modified（if-modified-since，客户端），日期值

###### 参考


- https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Cache-Control
- https://www.cnblogs.com/cckui/p/11506514.html
- https://www.jianshu.com/p/1a1536ab01f1

[[↑] 回到顶部](#目录)


### cookie, session, localStorage。

***cookie***

cookie包含名字，值，过期时间，路径和域，路径和域一起构成cookie的作用范围，如果不设置cookie的生命期，关闭浏览器，cookie就会消失。session是保存在服务器端的内存中。
```js
document.cookie = "d=4; expires=" + new Date(new Date().getTime() + 100000).toGMTString() + ";path=/page/;";
// 需要注意的是，设置时间也可以用max-age
document.cookie = "d=4; max-age=10; path=/page/;";
```
- 设置上：时间设置上，需要转换成GTM，domain相同的时候，cookie可以共享，比如在127.0.0.1:3000和127.0.0.1:8080同时启动了两个服务器，当在3000端口上设置cookie时，8080也会得到。
- 大小上：不同浏览器有所不同，根据网上的一些资料，总大小约为4KB左右，包括name和value合等号（仅供参考）.

***web Storage***

- 包含sessionStorage和localStorage，多数浏览器大小为5MB，发送请求时不会自动带到后台。并且提供了方便的原生api。
- sessionStorage：关闭浏览器消失，sessionstorage不在不同浏览器窗口中共享。
- localStorage：关闭浏览器不会消失，localStorage在所有同源窗口中共享。

***区别***

- cookie在不同端口下可以共享
- cookie在http请求中会自动带入，webStorage不会
- cookie一般大小为4KB，而webStorage一般可以存储5MB大小数据
- sessionStorage不在不同窗口共享，localStorage和cookie在同域的窗口下共享版
- webstorage提供更加方便的API接口


###### 参考

- https://www.cnblogs.com/jing-tian/p/10991431.html

[[↑] 回到顶部](#目录)


### URL输入到渲染全过程。

找寻服务器在网络上的地址->建立连接，获取html页面->浏览器渲染过程
- 通过dnf域名解析，将域名解析成对应的IP，并在在网络中找到对应地址
- 通过三次握手，建立TCP连接，发送HTTP请求，请求到页面。
- 浏览器得到页面后，开始进行渲染（边加载文件边渲染，后期添加该部分）

[[↑] 回到顶部](#目录)


### ssr,spa,seo。

- ssr:服务器端渲染，利于爬虫爬取到关键数据，利于seo。
- spa:单页面引用，首屏时间较长，因为会直接加载所有需要的文件，再通过JS动态渲染的方式，所以页面直接切换的速度非常快，减少了后端服务器的压力。
- seo:搜索引擎优化。


[[↑] 回到顶部](#目录)
