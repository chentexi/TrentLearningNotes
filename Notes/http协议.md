# http协议总结干货（请求、响应和状态码等）

HTTP：Hyper Text Transfer Protocol，超文本传输协议，设计HTTP最初的目的是为了提供一种发布和接收HTML页面的方法，目前广泛使用的是HTTP/1.1版本。

HTTP是一个基于TCP/IP通信协议来传递数据的协议，传输的数据类型为HTML 文件,、图片文件, 查询结果等。

http特点：

- http协议支持客户端/服务端模式，也是一种请求/响应模式的协议。
- 简单快速：客户向服务器请求服务时，只需传送请求方法和路径。请求方法常用的有GET、HEAD、POST。
- 灵活：HTTP允许传输任意类型的数据对象。传输的类型由Content-Type加以标记。
- 无连接：限制每次连接只处理一个请求。服务器处理完请求，并收到客户的应答后，即断开连接，但是却不利于客户端与服务器保持会话连接，为了弥补这种不足，产生了两项记录http状态的技术，一个叫做Cookie,一个叫做Session。
- 无状态：无状态是指协议对于事务处理没有记忆，后续处理需要前面的信息，则必须重传。

URI和URL

URI，统一资源标志符(Uniform Resource Identifier， URI)，表示的是web上每一种可用的资源。

URI通常由三部分组成：

- 访问资源的命名机制；
- 存放资源的主机名；
- 资源自身的名称。

URL是URI的一个子集。它是Uniform Resource Locator的缩写，统一资源定位符。

URL的一般格式为(带方括号[]的为可选项)：

```text
protocol :// hostname[:port] / path / [;parameters][?query]#fragment
```

URI和URL的区别：

URI和URL都定义了资源是什么，但URL还定义了该如何访问资源。URL是一种具体的URI，它是URI的一个子集，它不仅唯一标识资源，而且还提供了定位该资源的信息。URI 是一种语义上的抽象概念，可以是绝对的，也可以是相对的，而URL则必须提供足够的信息来定位，是绝对的。

请求报文构成

- 请求行：包括请求方法、URL、协议/版本
- 请求头(Request Header)：

```http
Accept: */*(客户端能接收的资源类型) 
Accept-Language: en-us(客户端接收的语言类型) 
Connection: Keep-Alive(维护客户端和服务端的连接关系) 
Host: localhost:8080(连接的目标主机和端口号) 
Referer: 告诉服务器我来自于哪里
User-Agent: Mozilla/4.0(客户端版本号的名字) 
Accept-Encoding: gzip, deflate(客户端能接收的压缩数据的类型) 
If-Modified-Since: Tue, 11 Jul 2000 18:23:51 GMT(缓存时间) 
Cookie(客户端暂存服务端的信息) 
Date: Tue, 11 Jul 2000 18:23:51 GMT(客户端请求服务端的时间)
```

- 请求正文

响应报文构成

- 状态行：HTTP/1.1(响应采用的协议和版本号) 200(状态码) OK(描述信息)
- 响应头：

```text
Location: 客户端访问的页面路径
Server:apache tomcat(服务端的Web服务端名)
Content-Encoding: gzip(服务端能够发送压缩编码类型) 
Content-Length: 80(服务端发送的压缩数据的长度) 
Content-Language: zh-cn(服务端发送的语言类型) 
Content-Type: text/html; charset=GB2312(服务端发送的类型及采用的编码方式)
Last-Modified: Tue, 11 Jul 2000 18:23:51 GMT(服务端对该资源最后修改的时间)
Refresh: 1;url=http://www.xxx.xxx(服务端要求客户端1秒钟后，刷新，然后访问指定的页面路径)
Content-Disposition: attachment; filename=aaa.zip(服务端要求客户端以下载文件的方式打开该文件)
Transfer-Encoding: chunked(分块传递数据到客户端） 
Set-Cookie:SS=Q0=5Lb_nQ; path=/search(服务端发送到客户端的暂存数据)
Expires: -1//3种(服务端禁止客户端缓存页面数据)
Cache-Control: no-***(服务端禁止客户端缓存页面数据) 
Pragma: no-***(服务端禁止客户端缓存页面数据) 
Connection: close(1.0)/(1.1)Keep-Alive(维护客户端和服务端的连接关系)
Date: Tue, 11 Jul 2000 18:23:51 GMT(服务端响应客户端的时间)
```

- 响应正文

请求方法：

- GET:请求指定的页面信息并返回结果。
- POST:向指定资源提交数据进行处理请求（例如提交表单或者上传文件）。数据被包含在请求体中。POST请求可能会导致新的资源的建立和/或已有资源的修改。
- HEAD:类似于get请求，只不过返回的响应中没有具体的内容，用于获取报头
- PUT:从客户端向服务器传送的数据取代指定的文档的内容（修改）。
- DELETE:请求服务器删除指定的数据。

post和get的区别：

- 都包含请求头请求行，post多了请求body。
- get多用来查询，请求参数放在url中，不会对服务器上的内容产生作用。post用来提交，如把账号密码放入body中。
- GET是直接添加到URL后面的，直接就可以在URL中看到内容，而POST是放在报文内部的，用户无法直接看到。
- GET提交的数据长度是有限制的，因为URL长度有限制，具体的长度限制视浏览器而定。而POST没有。

状态码分类：

- 1XX- 信息型，服务器收到请求，需要请求者继续操作。
- 2XX- 成功型，请求成功收到，理解并处理。
- 3XX - 重定向，需要进一步的操作以完成请求。
- 4XX - 客户端错误，请求包含语法错误或无法完成请求。
- 5XX - 服务器错误，服务器在处理请求的过程中发生了错误。

常见状态码：

- 200 OK - 客户端请求成功
- 201 created 已创建
- 206 服务端成功处理了部分get请求，比如：迅雷的断点续传、在线视频的同时多段下载，每一段响应都返回206
- 301 - 资源（网页等）被永久转移到其它URL，是redirect，但原来的url不可访问
- 302 - 临时跳转，也是redirect，原来的url还可以访问
- 304 - 未修改，对于动态页面做缓存加速，首先要在 Response 的 HTTP Header 中增加 Last Modified 定义，其次根据 Request 中的 If Modified Since 和被请求内容的更新时间来返回 200 或者 304 。虽然在返回 304 的时候已经做了一次数据库查询，但是可以避免接下来更多的数据库查询，并且没有返回页面内容而只是一个 HTTP Header，从而大大的降低带宽的消耗，对于用户的感觉也是提高。
- 400 Bad Request - 客户端请求有语法错误，不能被服务器所理解
- 401 Unauthorized - 请求未经授权，请求要求用户的身份认证。
- 404 - 请求资源不存在，可能是输入了错误的URL
- 405 - 客户端请求的方法被禁止
- 408 - 等待超时，服务端等待客户端发送请求的时间过长。
- 413 - 请求实体过大
- 414 - 请求url过长
- 未避免机器人频繁发送造成413、414响应的请求，可以在返回状态码的同时，在响应头部添加一个Retry-After 字段，表示客户端可以在此时间之后重新尝试发送请求。
- 500 - 服务器内部发生了不可预期的错误，一般是通用的服务端错误状态码
- 501 - 服务器对于客户端请求的方法未实现，比如客户端发起一个PUT请求但服务端不支持
- 502 - Bad Gateway，当服务器是作为一个网关或者代理时，在处理客户端请求时从上游服务器收到了一个无效的响应，这时服务器可以返回一个502 BadGateway。出现这种情况可能的原因有网关或代理服务器过滤了所有请求（那么就收不到真正服务器的响应了），或者真正的服务器挂掉了，这些时候代理或网关都可以发送502 BadGateway。
- 503 Server Unavailable - 服务器当前不能处理客户端的请求，一段时间后可能恢复正常。一般是由于服务器暂时过载或者停机维护。

HTTPS 协议（HyperText Transfer Protocol over Secure Socket Layer）：一般理解为HTTP+SSL/TLS，通过SSL证书来验证服务器的身份，并为浏览器和服务器之间的通信进行加密。

SSL（Secure Socket Layer，安全套接字层）：SSL 协议位于 TCP/IP 协议与各种应用层协议之间，为数据通讯提供安全支持。

TLS（Transport Layer Security，传输层安全）：其前身是 SSL，SSL3.0和TLS1.0由于存在安全漏洞，目前使用最广泛的是TLS 1.1、TLS 1.2。

https传输流程：

1. 首先客户端通过URL访问服务器建立SSL连接。
2. 服务端收到客户端请求后，会将网站支持的证书信息（证书中包含公钥）传送一份给客户端。
3. 客户端的服务器开始协商SSL连接的安全等级，也就是信息加密的等级。
4. 客户端的浏览器根据双方同意的安全等级，建立会话密钥，然后利用网站的公钥将会话密钥加密，并传送给网站。
5. 服务器利用自己的私钥解密出会话密钥。
6. 服务器利用会话密钥加密与客户端之间的通信。

http协议的缺点：

- 请求信息明文传输，容易被窃听截取。
- 数据的完整性未校验，容易被篡改
- 没有验证对方身份，存在冒充危险

HTTP协议不适合传输一些敏感信息，比如：各种账号、密码等信息，使用http协议传输隐私信息非常不安全。

HTTPS的缺点：

- HTTPS协议多次握手，导致页面的加载时间延长近50%；
- HTTPS连接缓存不如HTTP高效，会增加数据开销和功耗；
- 申请SSL证书需要钱，功能越强大的证书费用越高。
- SSL涉及到的安全算法会消耗 CPU 资源，对服务器资源消耗较大。

HTTPS和HTTP的区别：

- https是http协议的安全版本，http协议的数据传输是明文的，是不安全的，https使用了SSL/TLS协议进行了加密处理。
- http和https使用连接方式不同，默认端口也不一样，http是80，https是443



# **request中获取GET和POST请求参数**

一 获取请求方式
request.getMethod(); get和post都可用，

二 获取请求类型
request.getContentType(); get和post都可用，示例值：application/json ，multipart/form-data， application/xml等

三 获取所有参数key
request.getParameterNames(); get和post都可用，注：不适用contentType为multipart/form-data

四 获取参数值value
request.getParameter("test"); get和post都可用，注：不适用contentType为multipart/form-data

五 获取取参数请求集合
request.getParameterMap(); get和post都可用，注： 不适用contentType为multipart/form-data

六 获取文本流
request.getInputStream() 适用于如：application/json，xml,multipart/form-data文本流或者大文件形式提交的请求或者xml等形式的报文

七 获取URL
getRequestURL()

## 获取参数列表:

### getQueryString()

只适用于GET,比如客户端发送http://localhost/testServlet?a=b&c=d&e=f,通过request.getQueryString()得到的是a=b&c=d&e=f.

### getParameter()

GET和POST都可以使用

但如果是POST请求要根据

表单提交数据的编码方式来确定能否使用.当编码方式是(application/x- www-form-urlencoded)时才能使用.

这种编码方式(application/x-www-form-urlencoded)虽然简单，但对于传输大块的二进制数据显得力不从心.

对于传输大块的二进制数这类数据，浏览器采用了另一种编码方式("multipart/form-data"),这时就需要使用下面的两种方法.

### getInputStream()

### getReader()

上面两种方法获取的是Http请求包的包体,因为GET方式请求一般不包含包体.所以上面两种方法一般用于POST请求获取参数.

需要注意的是：

request.getParameter()、 request.getInputStream()、request.getReader()这三种方法是有冲突的，因为流只能被读一次。

比如：

当form表单内容采用 enctype=application/x-www-form-urlencoded编码时，先通过调用request.getParameter()方法得到参数后,

再调用request.getInputStream()或request.getReader()已经得不到流中的内容，

因为在调用 request.getParameter()时系统可能对表单中提交的数据以流的形式读了一次,反之亦然。

当form表单内容采用 enctype=multipart/form-data编码时，即使先调用request.getParameter()也得不到数据，

所以这时调用request.getParameter()方法对 request.getInputStream()或request.getReader()没有冲突，

即使已经调用了 request.getParameter()方法也可以通过调用request.getInputStream()或request.getReader()得到表单中的数据,

而request.getInputStream()和request.getReader()在同一个响应中是不能混合使用的,如果混合使用就会抛异常

request中获取GET和POST请求参数
https://blog.51cto.com/u_15127500/3815725