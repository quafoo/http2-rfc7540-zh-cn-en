# Starting HTTP/2 with Prior Knowledge / 先知情况下启用HTTP/2
> A client can learn that a particular server supports HTTP/2 by other means. For example, [*[ALT-SVC]*](https://httpwg.github.io/specs/rfc7540.html#ALT-SVC) describes a mechanism for advertising this capability.

客户端可以通过其他方式了解服务端是否支持HTTP/2。例如，[*[ALT-SVC]*](https://httpwg.github.io/specs/rfc7540.html#ALT-SVC) 描述了一种通知支持HTTP/2的机制。

> A client MUST send the connection preface ([Section 3.5](https://httpwg.github.io/specs/rfc7540.html#ConnectionHeader)) and then MAY immediately send HTTP/2 frames to such a server; servers can identify these connections by the presence of the connection preface. This only affects the establishment of HTTP/2 connections over cleartext TCP; implementations that support HTTP/2 over TLS MUST use protocol negotiation in TLS *[TLS-ALPN]*.

客户端必须先向这种服务端发送连接前奏( [3.5节](https://httpwg.github.io/specs/rfc7540.html#ConnectionHeader) )，然后可以立即发送HTTP/2帧。服务端能通过连接前奏识别出这种连接。这只影响基于明文TCP建立的HTTP/2连接。基于TLS的HTTP/2实现必须使用TLS中的协议协商*[TLS-ALPN]*。

> Likewise, the server MUST send a connection preface ([Section 3.5](https://httpwg.github.io/specs/rfc7540.html#ConnectionHeader)).

同样，服务端也必须发送一个连接前奏( [3.5节](https://httpwg.github.io/specs/rfc7540.html#ConnectionHeader) )。

> Without additional information, prior support for HTTP/2 is not a strong signal that a given server will support HTTP/2 for future connections. For example, it is possible for server configurations to change, for configurations to differ between instances in clustered servers, or for network conditions to change.

没有额外的参考信息，某个服务端先前支持HTTP/2并不能表明它在以后的连接中仍会支持HTTP/2。例如，可能服务端配置改变了，或者集群中不同服务器的配置有差异，或者网络状况改变了。