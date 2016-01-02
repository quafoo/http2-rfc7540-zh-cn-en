# 1. Introduction / 前言
> The Hypertext Transfer Protocol (HTTP) is a wildly successful protocol. However, the way HTTP/1.1 uses the underlying transport ([RFC7230], Section 6) has several characteristics that have a negative overall effect on application performance today.

超文本传输协议(HTTP)是一个非常成功的协议。但是，HTTP/1.1使用下层传输层( *[[RFC7230]](https://httpwg.github.io/specs/rfc7540.html#RFC7230)* ， [第6章](https://httpwg.github.io/specs/rfc7230.html#connection.management) )的方式所具有的一些特性，对如今的应用层性能产生了一个全面的消极的影响。

> In particular, HTTP/1.0 allowed only one request to be outstanding at a time on a given TCP connection. HTTP/1.1 added request pipeling, but this only partially addressed request concurrency and still suffers from head-of-line blocking. Therefore, HTTP/1.0 and HTTP/1.1 clients that need to make many requests use multiple connections to a server in order to achieve concurrency and thereby reduce latency.

尤其是，在一个给定的TCP连接上，HTTP/1.0只允许发送一次，每次一个请求。HTTP/1.1管线了管道技术，但这只是部分地实现了并发请求，并且仍然存在着线头阻塞问题。因此，HTTP/1.0和HTTP/1.1的用户要发送大量请求时，就使用多个连接来达到并发和减少延迟的目的。

> Furthermore, HTTP header fields are often repetitive and verbose, causing unnecessary network traffic as well as causing the initial TCP [TCP] congestion window to quickly fill. This can result in excessive latency when multiple requests are made on a new TCP connection.

此外，HTTP首部字段常常是重复和冗余的，从而产生了不必要的网络流量，也导致初始TCP拥塞窗口被快速填满。当在一个新建立的TCP连接上发送多个请求时，这会产生极大的时延。

> HTTP/2 addresses these issues by defining an optimized mapping of HTTP's semantics to an underlying connection. Specifically, it allows interleaving of request and response messages on the same connection and uses an efficient coding for HTTP header fields. It also allows prioritization of requests, letting more important requests complete more quickly, further improving performance.

通过定义一个优化的HTTP语义到底层连接的映射，HTTP/2解决了这些问题。具体来说，它允许在同一个连接上请求和响应消息交错往来，并且使用了高效的HTTP首部字段编码。它还允许请求具有优先级，让比较重要的请求可以更快地完成，从而进一步改善了性能。

> The resulting protocol is more friendly to the network because fewer TCP connections can be used in comparison to HTTP/1.x. This means less competition with other flows and longer-lived connections, which in turn lead to better utilization of available network capacity.

相比HTTP/1.x，HTTP/2使用的TCP连接更少，对网络更友好。这意味着，网络流之间的竞争减少了，连接的生存期延长了，从而相应地提高了可用的网络资源的利用率。

> Finally, HTTP/2 also enables more efficient processing of messages through use of binary message framing.

最后，通过使用二进制消息帧，HTTP/2也提高了消息的处理效率。
