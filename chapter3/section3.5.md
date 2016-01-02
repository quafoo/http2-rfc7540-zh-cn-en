# HTTP/2 Connection Preface / HTTP/2连接前奏
> In HTTP/2, each endpoint is required to send a connection preface as a final confirmation of the protocol in use and to establish the initial settings for the HTTP/2 connection. The client and server each send a different connection preface.

在HTTP/2中，要求两端都要发送一个连接前奏，作为对所使用协议的最终确认，并确定HTTP/2连接的初始设置。客户端和服务端各自发送不同的连接前奏。

> The client connection preface starts with a sequence of 24 octets, which in hex notation is:
> 
> ```
> 0x505249202a20485454502f322e300d0a0d0a534d0d0a0d0a
> ```
> That is, the connection preface starts with the string PRI * HTTP/2.0\r\n\r\nSM\r\n\r\n). This sequence MUST be followed by a [SETTINGS](https://httpwg.github.io/specs/rfc7540.html#SETTINGS) frame ([Section 6.5](https://httpwg.github.io/specs/rfc7540.html#SETTINGS)), which MAY be empty. The client sends the client connection preface immediately upon receipt of a 101 (Switching Protocols) response (indicating a successful upgrade) or as the first application data octets of a TLS connection. If starting an HTTP/2 connection with prior knowledge of server support for the protocol, the client connection preface is sent upon connection establishment.

客户端连接前奏以一个24字节的序列开始，用十六进制表示为：

```
0x505249202a20485454502f322e300d0a0d0a534d0d0a0d0a
```

即，连接前奏以字符串"PRI * HTTP/2.0\r\n\r\nSM\r\n\r\n"开始。这个序列后面必须跟一个可以为空的 [SETTINGS](https://httpwg.github.io/specs/rfc7540.html#SETTINGS) 帧( [6.5节](https://httpwg.github.io/specs/rfc7540.html#SETTINGS) )。客户端一收到101(Switching Protocols)响应(表示成功升级)后，就立即发送客户端连接前奏，或者将其作为TLS连接的第一批应用程序数据字节。如果在预先知道服务端支持HTTP/2的情况下启用HTTP/2连接，客户端连接前奏在连接建立时发送。

> Note: The client connection preface is selected so that a large proportion of HTTP/1.1 or HTTP/1.0 servers and intermediaries do not attempt to process further frames. Note that this does not address the concerns raised in [*[TALKING]*](https://httpwg.github.io/specs/rfc7540.html#TALKING).

注意：客户端连接前奏是专门挑选的，目的是为了让大部分HTTP/1.1或HTTP/1.0服务器和中介不会试图处理后面的帧，但这并没有解决在 [*[TALKING]*](https://httpwg.github.io/specs/rfc7540.html#TALKING) 中提出的问题。

> The server connection preface consists of a potentially empty [SETTINGS](https://httpwg.github.io/specs/rfc7540.html#SETTINGS) frame ([Section 6.5](https://httpwg.github.io/specs/rfc7540.html#SETTINGS)) that MUST be the first frame the server sends in the HTTP/2 connection.

服务端连接前奏包含一个可能为空的 [SETTINGS](https://httpwg.github.io/specs/rfc7540.html#SETTINGS) 帧( [6.5节](https://httpwg.github.io/specs/rfc7540.html#SETTINGS) )，它必须由服务端在HTTP/2连接中首先发送。

> The [SETTINGS](https://httpwg.github.io/specs/rfc7540.html#SETTINGS) frames received from a peer as part of the connection preface MUST be acknowledged (see [Section 6.5.3](https://httpwg.github.io/specs/rfc7540.html#SettingsSync)) after sending the connection preface.

在发送完本端的连接前奏之后，必须对收到的作为对端连接前奏一部分的 [SETTINGS](https://httpwg.github.io/specs/rfc7540.html%23SETTINGS) 帧进行确认(参见 [6.5.3节](https://httpwg.github.io/specs/rfc7540.html#SettingsSync) )。

> To avoid unnecessary latency, clients are permitted to send additional frames to the server immediately after sending the client connection preface, without waiting to receive the server connection preface. It is important to note, however, that the server connection preface [SETTINGS](https://httpwg.github.io/specs/rfc7540.html%23SETTINGS) frame might include parameters that necessarily alter how a client is expected to communicate with the server. Upon receiving the [SETTINGS](https://httpwg.github.io/specs/rfc7540.html%23SETTINGS) frame, the client is expected to honor any parameters established. In some configurations, it is possible for the server to transmit [SETTINGS](https://httpwg.github.io/specs/rfc7540.html%23SETTINGS) before the client sends additional frames, providing an opportunity to avoid this issue.

为了避免不必要的延迟，允许客户端发送完连接前奏后就立即向服务端发送其他的帧，而不必等待服务端的连接前奏。不过需要注意的是，服务端连接前奏的 [SETTINGS](https://httpwg.github.io/specs/rfc7540.html%23SETTINGS) 帧中可能包含一些期望客户端如何与服务端进行通信所必须修改的参数。在收到这些 [SETTINGS](https://httpwg.github.io/specs/rfc7540.html%23SETTINGS) 帧以后，客户端应当遵守所有设置的参数。在某些配置中，服务端是可以在客户端发送额外的帧之前传送 [SETTINGS](https://httpwg.github.io/specs/rfc7540.html%23SETTINGS) 帧的，这样就避免前边所说的问题。

> Clients and servers MUST treat an invalid connection preface as a connection error ([Section 5.4.1](https://httpwg.github.io/specs/rfc7540.html#ConnectionErrorHandler)) of type [PROTOCOL_ERROR](https://httpwg.github.io/specs/rfc7540.html#PROTOCOL_ERROR). A [GOAWAY](https://httpwg.github.io/specs/rfc7540.html#GOAWAY) frame ([Section 6.8](https://httpwg.github.io/specs/rfc7540.html#GOAWAY)) MAY be omitted in this case, since an invalid preface indicates that the peer is not using HTTP/2.

客户端和服务端都必须将无效的连接前奏处理为连接错误( [5.4.1节](https://httpwg.github.io/specs/rfc7540.html#ConnectionErrorHandler) )，错误类型为 [PROTOCOL_ERROR](https://httpwg.github.io/specs/rfc7540.html#PROTOCOL_ERROR) 。在这种情况下，可以忽略 [GOAWAY](https://httpwg.github.io/specs/rfc7540.html#GOAWAY) 帧( [6.8节](https://httpwg.github.io/specs/rfc7540.html#GOAWAY) )，因为无效的连接前奏表示对端并没有使用HTTP/2。