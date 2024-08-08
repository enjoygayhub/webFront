# HTTP

HTTP 本质是无状态的，使用 Cookies 可以创建有状态的会话。

## 资源定位URI

协议：http://
主机: www.xx.com
端口: :80 (标准端口HTTP 为 80，HTTPS 为 443)
路径: /path/myfile.html
查询: ?key1=value1&key2=value2 
片段：#SomewhereInTheDocument（# 号后面的部分，称为片段标识符，永远不会与请求一起发送到服务器。）

## MIME类型

媒体类型（通常称为 Multipurpose Internet Mail Extensions 或 MIME 类型）是一种标准，用来表示文档、文件或字节流的性质和格式
text/plain，image/png，audio/ogg，video/mp4，application/javascript（二进制数据）

## 代理主要有如下几种作用：

缓存（可以是公开的也可以是私有的，像浏览器的缓存）
过滤（像反病毒扫描，家长控制...）
负载均衡（让多个服务器服务不同的请求）
认证（对不同资源进行权限管理）
日志记录（允许存储历史信息）

## HTTP的发展

### HTTP/1.1
1. 连接可以复用，节省了多次打开 TCP 连接加载网页文档资源的时间。
2. 增加管线化技术，允许在第一个应答被完全发送之前就发送第二个请求，以降低通信延迟。
3. 支持响应分块。
4. 引入额外的缓存控制机制。
5. 引入内容协商机制，包括语言，编码，类型等，并允许客户端和服务器之间约定以最合适的内容进行交换。
6. 凭借Host头，能够使不同域名配置在同一个 IP 地址的服务器上。

### HTTP/2.0

1. 首部压缩
2. 多路复用：一个tcp链接发多个请求
3. 二进制分帧
4. 服务端推送：静态资源预加载：提前推送 CSS、JS、图片等资源。页面预渲染

## HTTP 状态码

1. 1XX 信息性状态码
   - 100 Continue继续
   - 101 Switching Protocol切换协议 例如使用websocket时需要切换
2. 2XX 成功状态码
   - 200 OK 成功处理了请求
   - 204 No Content 请求处理成功，但没有资源可返回
   - 206 Partial Content 请求资源的某一部分
3. 3XX 重定向状态码
   - 301 Moved Permanently 永久性重定向，表示请求的资源已被分配了新的 URI，与308类似
   - 302 Found 临时性重定向，资源的 URL 已临时定位到其他位置，与307类似
   - 303 See Other，告诉客户端应该用另一个 URL 获取资源
   - 304 Not Modified 未改变，告诉客户端使用缓存 
4. 4XX 客户端错误状态码
   - 400 Bad Request 由于语法无效，服务器无法理解该请求
   - 401 Unauthorized 未授权
   - 403 Forbidden 服务器拒绝了请求
   - 404 Not Found 服务器无法找到所请求的资源
5. 5XX 服务器错误状态码
   - 500 Internal Server Error内部服务器错误
   - 502 Bad Gateway 错误网关
   - 503 Service Unavailable 服务器暂时处于超负载或正在进行停机维护，无法处理请求。
   - 504 Gateway Timeout 响应超时

## HTTP 与 HTTPS 的区别

1. HTTP 传输的数据是明文未加密的，HTTPS 协议是由 HTTP 和 SSL 协议构建的可进行加密传输和身份认证的网络协议，安全性更高。
2. HTTPS 协议需要 CA 证书 验证身份，HTTP 协议不需要。
3. 链接方式不同，HTTP 协议的端口为 80，HTTPS 的端口为 443；


## HTTPS 协议的工作流程

1. 客户使用 HTTPS URL 访问服务器，则要求 web 服务器建立 SSL 链接。
2. web 服务器接收到客户端的请求之后，会将网站的证书（证书中包含了公钥），返回给客户端。
3. 客户端和 web 服务器端开始协商 SSL 链接的安全等级。
4. 客户端浏览器通过双方协商一致的安全等级，建立会话密钥，然后通过网站的公钥来加密会话密钥，并传送给网站。
5. web 服务器通过自己的私钥解密出会话密钥。
6. web 服务器通过会话密钥加密与客户端之间进行通信。

## HTTP 请求方法

1. GET：请求指定的页面信息，并返回实体主体。
2. HEAD：类似于 GET 请求，只不过返回的响应中没有具体的内容，用于获取报头
3. POST：向指定资源提交数据进行处理请求（例如提交表单或者上传文件）。数据被包含在请求body中。
4. PUT：从客户端向服务器传送的数据取代指定的文档的内容。
5. DELETE：请求服务器删除指定的页面。
6. CONNECT：HTTP/1.1 协议中预留给能够将连接改为管道方式的代理服务器。
7. OPTIONS：允许客户端查看服务器的性能。
8. TRACE：回显服务器收到的请求，主要用于测试或诊断。

## GET 和 POST 的区别

两者本质上都是 TCP 链接
1. get 参数通过 url 传递，post 放在请求体 (request body) 中。
2. get 请求在 url 中传递的参数是有长度限制的，而 post 没有。
3. get 比 post 更不安全，因为参数直接暴露在 url 中，所以不能用来传递敏感信息。
4. get 请求有幂等性，而 post没有。
5. get 请求参数会被完整保留在浏览历史记录里，而 post 中的参数不会被保留。

## 浏览器的同源政策

协议、域名、端口号必须相同，否则则不属于同一个域。

同源政策主要限制了三个方面
第一当前域下的 js 脚本不能够访问其他域下的localStorage 和 indexDB。
第二当前域下的 js 脚本不能够操作访问其他域下的 DOM。
第三当前域下 ajax 无法发送跨域请求。

## 跨源资源共享（CORS）需要的场景

1.  XMLHttpRequest 或 Fetch API 发起的跨域 HTTP 请求。
2. Web 字体。
3. WebGL 贴图。
4. 使用 drawImage() 将图片或视频画面绘制到 canvas。
5. 来自图像的 CSS 图形。

## 网络请求跨域

> 当一个请求url的 协议、域名、端口三者之间任意一个与当前页面url不同即为跨域。

1. JSONP (JSON with Padding) 通过动态创建 script，再请求一个带参网址实现跨域通信，目前已过时。

2. CORS (跨域资源共享)
    CORS的基本思想就是使用自定义的HTTP头部让浏览器与服务器进行沟通，从而决定请求或响应是应该成功还是失败。
    简单请求：只需服务端设置 `Access-Control-Allow-Origin：*` 即可，若要带 cookie 前端设置`withCredentials`为true,后端设置`Access-Control-Allow-Credentials`为true,同时`Access-Control-Allow-Origin`不能设置为`*`
    复杂请求：使用 OPTIONS 方法发起一个预检请求到服务器，告知服务器实际请求使用方法和首部自定义字段，服务端响应返回实际请求接受的域和方法，头部。如果要携带cookie，Access-Control-Allow-Origin，Access-Control-Allow-Headers，Access-Control-Allow-Methods 均不能设置为*。
3. nginx反向代理,服务端请求不受浏览器限制，既访问同源服务端，服务端再去寻找正在的地址拿到数据再返回
4. webpack正向代理， 客户端配置正向代理，替换表面访问地址。
5. WebSocket协议跨域

## 强缓存与协商缓存

### Expires 与 Cache-Control：max-age
HTTP/1.0 中的Expires 
Expires 的值是一个绝对时间的 GMT 格式的时间字符串。比如 Expires 值是：`expires:Fri, 14 Apr 2017 10:47:02 GMT`。这个时间代表这这个资源的失效时间，只要发送请求时间是在 Expires 之前，那么本地缓存始终有效，则在缓存中读取数据。

HTTP/1.1 中，Cache-Control指定 max-age——代表经过的时间后缓存过期。
同时启用的时候 Cache-Control 优先级高。

### 缓存重新验证响应 

`Etag/If-None-Match`返回的是一个校验码。`Etag`可以保证每一个资源是唯一的，资源变化都会导致`Etag`变化。服务器根据浏览器发送的`If-None-Match`值来判断是否命中缓存。

Last-Modify是一个时间标识该资源的最后修改时间，例如Last-Modify: Thu,31 Dec 2037 23:59:59 GMT。当浏览器再次请求该资源时，request的请求头中会包含If-Modify-Since，该值为缓存之前返回Last-Modify。服务器收到If-Modify-Since后，根据资源的最后修改时间判断是否命中缓存。如果命中缓存，则返回304，并且不会返回资源内容，并且不会返回Last-Modify。

`Last-Modified` 与 `ETag` 是可以一起使用的，服务器会优先验证`ETag`，一致的情况下，才会继续比对 `Last-Modified`，最后才决定是否返回 304。

### 不使用缓存 Cache-Control

no-cache 指令不会阻止响应的存储，而是阻止在没有重新验证的情况下重用响应。
no-store 响应不存储在任何缓存中
private 私有缓存

## 网络安全

1. XSS攻击(cross-site script)

   有两种主要的方法，注入恶意的脚本，返回到浏览器运行 ：
  + 反射型：如果客户端将用户输入信息带入代码中，比如在输入的搜索的关键词、图片的链接后附带脚本，被编码进html代码中，浏览器会执行附带的脚本代码 
  + 持久型 ：服务端保存了用户的携带恶意代码的评论，其他的用户浏览到该评论时就可能会执行。

  防范 XSS 攻击的最好方式就是删除或禁用任何可能包含可运行代码指令的标记。对 HTML 来说，这些包括类似 <script>, <object>, <embed>,和 <link> 的标签。对用户的输入过滤。
  Content-Security-Policy 允许站点管理者控制用户代理能够为指定的页面加载哪些资源。除了少数例外情况，设置的政策主要涉及指定服务器的源和脚本结束点。

2. CSRF攻击(cross site request forgery,跨站请求伪造)

在其他站点伪造post请求，如果用户登录过，浏览器可能会记录cookie，在请求时便携带上了。便可以以用户的名义进行非法操作。
不过现代浏览器跨域的网络请求不会携带cookie。
服务端一般都会与token身份验证，token可以保证用户不同设备登录互踢，后端可以确认请求中没有token便会返回未授权。
如何防止CSRF攻击？
CSRF Token: 在表单中添加一个随机生成的 Token，服务器端验证该 Token 的有效性。
SameSite Cookie: 设置 Cookie 的 SameSite 属性为 Strict 或 Lax，可以有效防止 CSRF 攻击。
Referer Header检查: 检查请求的 Referer Header 是否为本站点。
验证码: 在关键操作前增加验证码验证。

3. SQL注入

当传递给底层 SQL 语句的用户输入可以修改该语句的语义，那么就能执行恶意的sql操作。
所以需要将用户输入中任何在 SQL 语句中有特殊含义的字符进行转义。

4. 劫持

将站点链到隐藏的iframe里，劫持上层的操作，转发给隐藏的iframe。从而执行恶意操作。
可以使用 HTTP X-Frame-Options 响应头以及 CSP: frame-ancestors 指令，避免自己网站被当做 iframe 嵌入到非法网站引导用户点击

5. 拒绝服务 DoS
Dos 通常通过使用伪造的请求淹没站点，合法用户的访问就会被中断。这些请求可能仅仅是数量巨大或者是单独消耗了大量资源 (如 延缓读，上传大文件等) 。

## Blob/File 对象如何上传

Blob： 表示一个原始的二进制数据。它可以包含文件的内容、文本或其他类型的二进制数据。Blob 对象是不可变的，一旦创建，其内容就不能被修改。
File： 继承自 Blob，表示一个用户系统中的文件。除了 Blob 的特性外，File 对象还包含了一些额外的属性，比如文件名、文件类型等。

上传过程：
创建 FormData 对象：将 File 对象添加到 FormData 对象中，指定一个表单字段名。
创建一个 XMLHttpRequest 对象，发送 HTTP 请求。设置请求方法为 POST，指定请求的 URL。
设置请求头中的 Content-Type，通常为 multipart/form-data，表示这是一个多部分表单数据。
发送请求：将 FormData 对象作为请求体，发送到服务器。
