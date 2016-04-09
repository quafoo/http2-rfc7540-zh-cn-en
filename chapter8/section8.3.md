# The CONNECT Method / CONNECT 方法
> In HTTP/1.x, the pseudo-method CONNECT ([*[RFC7231]*](http://httpwg.org/specs/rfc7540.html#RFC7231), [Section 4.3.6](http://httpwg.org/specs/rfc7231.html#CONNECT)) is used to convert an HTTP connection into a tunnel to a remote host. CONNECT is primarily used with HTTP proxies to establish a TLS session with an origin server for the purposes of interacting with https resources.

在HTTP/1.x里，CONNECT( [*[RFC7231]*](http://httpwg.org/specs/rfc7540.html#RFC7231), [4.3.6节](http://httpwg.org/specs/rfc7231.html#CONNECT) ) 伪方法用于将和远端主机的HTTP连接转换为隧道。CONNECT 主要用于HTTP代理和源服务器建立TLS会话，其目的是和https资源交互。


> In HTTP/2, the CONNECT method is used to establish a tunnel over a single HTTP/2 stream to a remote host for similar purposes. The HTTP header field mapping works as defined in [Section 8.1.2.3](http://httpwg.org/specs/rfc7540.html#HttpRequest) ("[Request Pseudo-Header Fields](http://httpwg.org/specs/rfc7540.html#HttpRequest)"), with a few differences. Specifically:
> 
> * The :method pseudo-header field is set to CONNECT.
> 
> * The :scheme and :path pseudo-header fields MUST be omitted.
> 
> * The :authority pseudo-header field contains the host and port to connect to (equivalent to the authority-form of the request-target of CONNECT requests (see [*[RFC7230]*](http://httpwg.org/specs/rfc7540.html#RFC7230), [Section 5.3](http://httpwg.org/specs/rfc7230.html#request-target))).

在HTTP/2中, CONNECT 方法用于在一个到远端主机的单独的HTTP/2流之上建立隧道。HTTP首部字段映射以像在 [8.1.2.3节](http://httpwg.org/specs/rfc7540.html#HttpRequest)( [请求的伪首部字段](http://httpwg.org/specs/rfc7540.html#HttpRequest) ) 定义的那样起作用，但有一些不同，即：

* `:method` 伪首部字段设置为 CONNECT。
* 必须忽略`:scheme` 和 `:path`伪首部字段。
* `:authority`伪首部字段包含要连接的主机和端口(等价于 CONNECT 请求目标的 authority形式( [*[RFC7230]*](http://httpwg.org/specs/rfc7540.html#RFC7230), [5.3节](http://httpwg.org/specs/rfc7230.html#request-target) ))。


> A CONNECT request that does not conform to these restrictions is malformed ([Section 8.1.2.6](http://httpwg.org/specs/rfc7540.html#malformed)).

不符合这些限制的 CONNECT 请求是有缺陷的( [8.1.2.6节](http://httpwg.org/specs/rfc7540.html#malformed) )。


> A proxy that supports CONNECT establishes a [TCP connection](http://httpwg.org/specs/rfc7540.html#TCP) *[TCP]* to the server identified in the :authority pseudo-header field. Once this connection is successfully established, the proxy sends a [HEADERS](http://httpwg.org/specs/rfc7540.html#HEADERS) frame containing a 2xx series status code to the client, as defined in [*[RFC7231]*](http://httpwg.org/specs/rfc7540.html#RFC7231), [Section 4.3.6](http://httpwg.org/specs/rfc7231.html#CONNECT).

支持 CONNECT 的代理建立到服务端的 [TCP连接](http://httpwg.org/specs/rfc7540.html#TCP) *[TCP]*，连接靠`：authority`伪首部字段进行区分。一旦连接成功建立，代理就向客户端发送一个包含2xx系列状态码的 [HEADERS](http://httpwg.org/specs/rfc7540.html#HEADERS) 帧，正如 [*[RFC7231]*](http://httpwg.org/specs/rfc7540.html#RFC7231) 的 [4.3.6节](http://httpwg.org/specs/rfc7231.html#CONNECT) 定义的那样。


> After the initial [HEADERS](http://httpwg.org/specs/rfc7540.html#HEADERS) frame sent by each peer, all subsequent [DATA](http://httpwg.org/specs/rfc7540.html#DATA) frames correspond to data sent on the TCP connection. The payload of any [DATA](http://httpwg.org/specs/rfc7540.html#DATA) frames sent by the client is transmitted by the proxy to the TCP server; data received from the TCP server is assembled into [DATA](http://httpwg.org/specs/rfc7540.html#DATA) frames by the proxy. Frame types other than [DATA](http://httpwg.org/specs/rfc7540.html#DATA) or stream management frames ([RST\_STREAM](http://httpwg.org/specs/rfc7540.html#RST_STREAM), [WINDOW_UPDATE](http://httpwg.org/specs/rfc7540.html#WINDOW_UPDATE), and [PRIORITY](http://httpwg.org/specs/rfc7540.html#PRIORITY)) MUST NOT be sent on a connected stream and MUST be treated as a stream error ([Section 5.4.2](http://httpwg.org/specs/rfc7540.html#StreamErrorHandler)) if received.

两端发送完初始的 [HEADERS](http://httpwg.org/specs/rfc7540.html#HEADERS) 帧以后，所有随后的 [DATA](http://httpwg.org/specs/rfc7540.html#DATA) 帧就对应于在TCP连接上发送的数据。客户端发送的任意 [DATA](http://httpwg.org/specs/rfc7540.html#DATA) 帧的负载通过代理传送给TCP服务器；从TCP服务器收到的数据被代理组装成 [DATA](http://httpwg.org/specs/rfc7540.html#DATA) 帧。不能在处于连接状态的流上发送除 [DATA](http://httpwg.org/specs/rfc7540.html#DATA) 帧或流管理帧([RST\_STREAM](http://httpwg.org/specs/rfc7540.html#RST_STREAM), [WINDOW_UPDATE](http://httpwg.org/specs/rfc7540.html#WINDOW_UPDATE), 和 [PRIORITY](http://httpwg.org/specs/rfc7540.html#PRIORITY))之外的帧类型，如果收到了这样的帧，必须将其当做流错误( [5.4.2节](http://httpwg.org/specs/rfc7540.html#StreamErrorHandler) )。


> The TCP connection can be closed by either peer. The END\_STREAM flag on a [DATA](http://httpwg.org/specs/rfc7540.html#DATA) frame is treated as being equivalent to the TCP FIN bit. A client is expected to send a [DATA](http://httpwg.org/specs/rfc7540.html#DATA) frame with the END\_STREAM flag set after receiving a frame bearing the END\_STREAM flag. A proxy that receives a [DATA](http://httpwg.org/specs/rfc7540.html#DATA) frame with the END_STREAM flag set sends the attached data with the FIN bit set on the last TCP segment. A proxy that receives a TCP segment with the FIN bit set sends a [DATA](http://httpwg.org/specs/rfc7540.html#DATA) frame with the END\_STREAM flag set. Note that the final TCP segment or [DATA](http://httpwg.org/specs/rfc7540.html#DATA) frame could be empty.

TCP连接可以由任意一端关闭。[DATA](http://httpwg.org/specs/rfc7540.html#DATA) 帧上的 END\_STREAM 标志被当做等价于TCP的FIN比特。收到一个带有 END\_STREAM 标志的帧以后，客户端应该发送一个设置了 END\_STREAM 标志的 [DATA](http://httpwg.org/specs/rfc7540.html#DATA) 帧。如果代理收到了设置有 END\_STREAM 标志的 [DATA](http://httpwg.org/specs/rfc7540.html#DATA) 帧，就发送附加的数据，并在最后一个TCP片段上设置了 FIN 标志位。如果代理收到了一个设置了 FIN 标志位的TCP片段，就发送一个设置了 END\_STREAM 标志的 [DATA](http://httpwg.org/specs/rfc7540.html#DATA) 帧。注意，最后的TCP片段或者 [DATA](http://httpwg.org/specs/rfc7540.html#DATA) 帧可以为空。


> A TCP connection error is signaled with [RST\_STREAM](http://httpwg.org/specs/rfc7540.html#RST_STREAM). A proxy treats any error in the TCP connection, which includes receiving a TCP segment with the RST bit set, as a stream error ([Section 5.4.2](http://httpwg.org/specs/rfc7540.html#StreamErrorHandler)) of type [CONNECT_ERROR](http://httpwg.org/specs/rfc7540.html#CONNECT_ERROR). Correspondingly, a proxy MUST send a TCP segment with the RST bit set if it detects an error with the stream or the HTTP/2 connection.

TCP连接错误用 [RST\_STREAM](http://httpwg.org/specs/rfc7540.html#RST_STREAM) 帧表示。代理将TCP连接中的任意错误，包括收到设置了RST标志位的TCP片段，都当做类型为 [CONNECT_ERROR](http://httpwg.org/specs/rfc7540.html#CONNECT_ERROR) 的流错误( [5.4.2节](http://httpwg.org/specs/rfc7540.html#StreamErrorHandler) )。相应地，如果代理探测到了流错误或者HTTP/2连接错误，就必须发送一个设置了RST标志位的TCP片段。