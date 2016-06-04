# Connection Management / 连接管理
> HTTP/2 connections are persistent. For best performance, it is expected that clients will not close connections until it is determined that no further communication with a server is necessary (for example, when a user navigates away from a particular web page) or until the server closes the connection.

HTTP/2连接是持久连接。为了获得最佳性能，人们期望客户端不要关闭连接，直到确定不需要再继续和服务端进行通信(例如，当用户离开一个特定的web页面时)，或者直到服务端关闭连接。


> Clients SHOULD NOT open more than one HTTP/2 connection to a given host and port pair, where the host is derived from a URI, a selected [alternative service](http://httpwg.org/specs/rfc7540.html#ALT-SVC) *[ALT-SVC]*, or a configured proxy.

对于给定的一对主机和端口，客户端应该只打开一个 HTTP/2 连接，其中主机或者来自于一个 URI ，一种选定的 [可替换的服务](http://httpwg.org/specs/rfc7540.html#ALT-SVC) *[ALT-SVC]* ，或者来自于一个已配置的代理。


> A client can create additional connections as replacements, either to replace connections that are near to exhausting the available stream identifier space ([Section 5.1.1](http://httpwg.org/specs/rfc7540.html#StreamIdentifiers)), to refresh the keying material for a TLS connection, or to replace connections that have encountered errors ([Section 5.4.1](http://httpwg.org/specs/rfc7540.html#ConnectionErrorHandler)).

客户端可以创建额外的连接作为替补，或者用来替换将要耗尽可用流标识符空间( [5.1.1节](http://httpwg.org/specs/rfc7540.html#StreamIdentifiers) )的连接，为一个 TLS 连接刷新关键资料，或者用来替换遇到错误( [5.4.1节](http://httpwg.org/specs/rfc7540.html#ConnectionErrorHandler) )的连接。


> A client MAY open multiple connections to the same IP address and TCP port using different [Server Name Indication](http://httpwg.org/specs/rfc7540.html#TLS-EXT) *[TLS-EXT]* values or to provide different TLS client certificates but SHOULD avoid creating multiple connections with the same configuration.

客户端可以和相同的 IP 地址和 TCP 端口建立多个连接，这些地址和端口使用不同的[服务器名称指示](http://httpwg.org/specs/rfc7540.html#TLS-EXT) *[TLS-EXT]* 值，或者提供不同的 TLS 客户端证书。但是应该避免使用相同的配置创建多个连接。


> Servers are encouraged to maintain open connections for as long as possible but are permitted to terminate idle connections if necessary. When either endpoint chooses to close the transport-layer TCP connection, the terminating endpoint SHOULD first send a [GOAWAY](http://httpwg.org/specs/rfc7540.html#GOAWAY) ([Section 6.8](http://httpwg.org/specs/rfc7540.html#GOAWAY)) frame so that both endpoints can reliably determine whether previously sent frames have been processed and gracefully complete or terminate any necessary remaining tasks.

鼓励服务端尽可能长地维持打开的连接，但是准许服务端在必要时可以关闭空闲的连接。当任何一端选择关闭传输层 TCP 连接时，主动关闭的一端应该先发送一个 [GOAWAY](http://httpwg.org/specs/rfc7540.html#GOAWAY) 帧( [6.8节](http://httpwg.org/specs/rfc7540.html#GOAWAY) )，这样两端都可以确定地知道先前发送的帧是否已被处理，并且优雅地完成或者终结任何剩下的必要任务。