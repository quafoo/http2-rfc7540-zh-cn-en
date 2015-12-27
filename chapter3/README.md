# Starting HTTP/2 / 开始HTTP/2
> An HTTP/2 connection is an application-layer protocol running on top of a TCP connection ([*TCP*](https://httpwg.github.io/specs/rfc7540.html#TCP)). The client is the TCP connection initiator.

HTTP/2是运行于TCP连接([*[TCP]*](https://httpwg.github.io/specs/rfc7540.html#TCP))之上的应用层协议。客户端是TCP连接的发起者。

> HTTP/2 uses the same "http" and "https" URI schemes used by HTTP/1.1. HTTP/2 shares the same default port numbers: 80 for "http" URIs and 443 for "https" URIs. As a result, implementations processing requests for target resource URIs like http://example.org/foo or https://example.com/bar are required to first discover whether the upstream server (the immediate peer to which the client wishes to establish a connection) supports HTTP/2.

HTTP/2使用与HTTP/1.1相同的URI方案"http"和"https"，共享同样的默认端口号："http"的80端口和"https"的443端口。因此，在处理对例如http://example.org/foo 或 https://example.com/bar 目标资源URIs的请求前，需要首先确定上游服务端(当前客户端希望直接与之建立连接的对端)是否支持HTTP/2。

> The means by which support for HTTP/2 is determined is different for "http" and "https" URIs. Discovery for "http" URIs is described in Section 3.2. Discovery for "https" URIs is described in Section 3.3.

检测"http"和"https"的URIs是否支持HTTP/2的方法是不一样的。检测"http"的URIs在3.2节中描述。检测"https"的URIs在3.3节中描述。