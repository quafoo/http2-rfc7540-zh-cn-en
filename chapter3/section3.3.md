# Starting HTTP/2 for "https" URIs / 为"https" URIs启用HTTP/2协议
> A client that makes a request to an "https" URI uses TLS [TLS12] with the application-layer protocol negotiation (ALPN) extension [TLS-ALPN].

客户端对"https" URI发起请求时使用TLS-ALPN(TLS [TLS12] with the application-layer protocol negotiation (ALPN) extension)。

> HTTP/2 over TLS uses the "h2" protocol identifier. The "h2c" protocol identifier MUST NOT be sent by a client or selected by a server; the "h2c" protocol identifier describes a protocol that does not use TLS.

运行在TLS之上的HTTP/2使用"h2"协议标识符。此时，客户端不能发送"h2c"协议标识符，服务端也不能选择"h2c"协议标识符；"h2c"协议标识符表示HTTP/2不使用TLS。

> Once TLS negotiation is complete, both the client and the server MUST send a connection preface (Section 3.5).

一旦TLS协商完成，客户端和服务端都必须发送一个连接前奏(3.5节)。