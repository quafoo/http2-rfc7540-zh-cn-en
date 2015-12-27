# Starting HTTP/2 for "http" URIs / 为"http" URIs启用HTTP/2协议
> A client that makes a request for an "http" URI without prior knowledge about support for HTTP/2 on the next hop uses the HTTP Upgrade mechanism (Section 6.7 of [RFC7230]). The client does so by making an HTTP/1.1 request that includes an Upgrade header field with the "h2c" token. Such an HTTP/1.1 request MUST include exactly one HTTP2-Settings (Section 3.2.1) header field.
> 
> For example:
> 
> ```
> GET / HTTP/1.1
> Host: server.example.com
> Connection: Upgrade, HTTP2-Settings
> Upgrade: h2c
> HTTP2-Settings: <base64url encoding of HTTP/2 SETTINGS payload>
> ```

事先不知道下一跳是否支持HTTP/2的客户端，使用HTTP的Upgrade机制(*[RFC7230]*的6.7节)发起"http"URI请求。其做法是：客户端先发起HTTP/1.1请求，该请求包含值为"h2c"的Upgrade首部字段，还必须包含一个HTTP2-Settings(3.2.1节)首部字段。

例如：

```
GET / HTTP/1.1
Host: server.example.com
Connection: Upgrade, HTTP2-Settings
Upgrade: h2c
HTTP2-Settings: <base64url encoding of HTTP/2 SETTINGS payload>
```

> Requests that contain a payload body MUST be sent in their entirety before the client can send HTTP/2 frames. This means that a large request can block the use of the connection until it is completely sent.

在客户端能发送HTTP/2帧之前，包含负载的请求必须全部被发送。这意味着一个大的请求能阻塞连接的使用，直到其全部被发送完毕。

> If concurrency of an initial request with subsequent requests is important, an OPTIONS request can be used to perform the upgrade to HTTP/2, at the cost of an additional round trip.

如果一个初始请求的后续并发请求很重要，那么可以使用OPTIONS请求来执行升级到HTTP/2的操作，代价是一个额外的往返。

> A server that does not support HTTP/2 can respond to the request as though the Upgrade header field were absent:
> 
> ```
> HTTP/1.1 200 OK
> Content-Length: 243
> Content-Type: text/html
> ...
> ```

不支持HTTP/2的服务端对请求进行响应时可以忽略Upgrade首部字段：

```
HTTP/1.1 200 OK
Content-Length: 243
Content-Type: text/html
...
```

> A server MUST ignore an "h2" token in an Upgrade header field. Presence of a token with "h2" implies HTTP/2 over TLS, which is instead negotiated as described in Section 3.3.

服务端必须忽略值为"h2"的Upgrade首部字段。"h2"字段表示HTTP/2使用了TLS，其协商方法在3.3节中描述。

> A server that supports HTTP/2 accepts the upgrade with a 101 (Switching Protocols) response. After the empty line that terminates the 101 response, the server can begin sending HTTP/2 frames. These frames MUST include a response to the request that initiated the upgrade.
> 
> For example:
> 
> ```
> HTTP/1.1 101 Switching Protocols
> Connection: Upgrade
> Upgrade: h2c
> 
> [ HTTP/2 connection ...
> ```

支持HTTP/2的服务端返回101(Switching Protocols)响应，表示接受升级协议的请求。结束101响应的空行之后，服务端可以开始发送HTTP/2帧。这些帧必须包含一个对升级请求的响应。

例如：

```
HTTP/1.1 101 Switching Protocols
Connection: Upgrade
Upgrade: h2c

[ HTTP/2 connection ...
```

> The first HTTP/2 frame sent by the server MUST be a server connection preface (Section 3.5) consisting of a SETTINGS frame (Section 6.5). Upon receiving the 101 response, the client MUST send a connection preface (Section 3.5), which includes a SETTINGS frame.

服务端发送的第一个HTTP/2帧必须是由一个SETTINGS帧(6.5节)组成的服务端连接前奏(3.5节)。客户端一收到101响应，也必须发送一个包含SETTINGS帧的连接前奏。

> The HTTP/1.1 request that is sent prior to upgrade is assigned a stream identifier of 1 (see Section 5.1.1) with default priority values (Section 5.3.5). Stream 1 is implicitly "half-closed" from the client toward the server (see Section 5.1), since the request is completed as an HTTP/1.1 request. After commencing the HTTP/2 connection, stream 1 is used for the response.

升级之前发送的HTTP/1.1请求被分配一个流标识符1(5.1.1节)，并被赋予默认优先级值(5.3.5节)。流1暗示从客户端到服务端(5.1节)是半关闭的，因为该请求作为HTTP/1.1请求已经完成了。HTTP/2连接开始后，流1用于响应。