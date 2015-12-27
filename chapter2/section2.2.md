# 2.2 Conventions and Terminology /  约定和术语
> The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in RFC 2119 [*[RFC2119]*](https://httpwg.github.io/specs/rfc7540.html#RFC2119).

本文档中出现的关键词"MUST","MUST NOT","REQUIRED","SHALL","SHALL NOT","SHOULD","SHOULD NOT","RECOMMENDED","MAY",和"OPTIONAL"在RFC 2119 [*[RFC2119]*](https://httpwg.github.io/specs/rfc7540.html#RFC2119)中有解释。

> All numeric values are in network byte order. Values are unsigned unless otherwise indicated. ==Literal values== are provided in decimal or hexadecimal as appropriate. Hexadecimal literals are prefixed with 0x to distinguish them from decimal literals.

所有数值都以网络字节序表示。除非另有说明，否则数值都是无符号的。视情况以十进制或十六进制表示字面值。十六进制值带有0x前缀，以和十进制值区分开。

> The following terms are used:
> 
> * **client:** The endpoint that initiates an HTTP/2 connection. Clients send 	HTTP requests and receive HTTP responses.
> * **connection:** A transport-layer connection between two endpoints.
> * **connection error:** An error that affects the entire HTTP/2 connection.
> * **endpoint:** Either the client or server of the connection.
> * **frame:** The smallest unit of communication within an HTTP/2 connection, consisting of a header and a variable-length sequence of octets structured according to the frame type.
> * **peer:** An endpoint. When discussing a particular endpoint, "peer" refers to the endpoint that is remote to the primary subject of discussion.
> * **receiver:** An endpoint that is receiving frames.
> * **sender:** An endpoint that is transmitting frames.
> * **server:** The endpoint that accepts an HTTP/2 connection. Servers receive HTTP requests and send HTTP responses.
> * **stream:** A bidirectional flow of frames within the HTTP/2 connection.
> 
> * **stream error:** An error on the individual HTTP/2 stream.

文中使用的术语包括：

* **客户端：** 发起HTTP/2连接的端点。客户端发送HTTP请求，并接收HTTP响应。
* **连接：** 两个端点之间的传输层连接。
* **连接错误：** 影响整个HTTP/2连接的错误。
* **端点：** 连接中的客户端或服务器。
* **帧：** HTTP/2连接中的最小通信单元，由首部和可变长度的字节序列组成，其结构取决于帧类型。
* **对端：** 一个端点。当讨论一个特定的端点时，『对端』指的是其远程端点。
* **接收端：** 接收帧的端点。
* **发送端：** 发送帧的端点。
* **服务端：** 接受HTTP/2连接请求的端点。服务端接收HTTP请求，并发送HTTP响应。
* **流：** HTTP/2连接中的双向帧传输流。
* **流错误：** 发生在单独的HTTP/2流上的错误。

> Finally, the terms "gateway", "==intermediary==", "proxy", and "tunnel" are defined in [Section 2.3](https://httpwg.github.io/specs/rfc7230.html#intermediaries) of [*[RFC 7230]*](https://httpwg.github.io/specs/rfc7540.html#RFC7230). Intermediaries act as both client and server at different times.

最后，『网关』、『中介』、『代理』和『隧道』等术语都在[*[RFC 7230]*](https://httpwg.github.io/specs/rfc7540.html#RFC7230)的[2.3节](https://httpwg.github.io/specs/rfc7230.html#intermediaries)中有定义。在不同的时候，中介既可以是客户端，又可以是服务端。

> The term "==payload body==" is defined in [Section 3.3](https://httpwg.github.io/specs/rfc7230.html#message.body) of [*[RFC 7230]*](https://httpwg.github.io/specs/rfc7540.html#RFC7230).

术语『有效载荷体』在[*[RFC 7230]*](https://httpwg.github.io/specs/rfc7540.html#RFC7230)的[3.3节](https://httpwg.github.io/specs/rfc7230.html#message.body)中有定义。