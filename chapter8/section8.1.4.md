# Request Reliability Mechanisms in HTTP/2 / HTTP/2里的请求可靠机制
> In HTTP/1.1, an HTTP client is unable to retry a non-idempotent request when an error occurs because there is no means to determine the nature of the error. It is possible that some server processing occurred prior to the error, which could result in undesirable effects if the request were reattempted.

在HTTP/1.1里，发生错误时，HTTP客户端不能重试一个非幂等的请求，因为没有办法判定错误的性质。相比错误，一些服务端可能优先处理已经发生的请求，如果重试发生错误的请求，可能会导致不可预料的影响。


> HTTP/2 provides two mechanisms for providing a guarantee to a client that a request has not been processed:
> 
> * The [GOAWAY](http://httpwg.org/specs/rfc7540.html#GOAWAY) frame indicates the highest stream number that might have been processed. Requests on streams with higher numbers are therefore guaranteed to be safe to retry.
> 
> * The [REFUSED\_STREAM](http://httpwg.org/specs/rfc7540.html#REFUSED_STREAM) error code can be included in a [RST_STREAM](http://httpwg.org/specs/rfc7540.html#RST_STREAM) frame to indicate that the stream is being closed prior to any processing having occurred. Any request that was sent on the reset stream can be safely retried.

对于请求尚未被处理的客户端，HTTP/2提供了两种保障机制：

* [GOAWAY](http://httpwg.org/specs/rfc7540.html#GOAWAY) 帧指出了可能已被处理的最大流号。因此，在流号更大的流上发送请求时，可以保证重试安全。
* [RST\_STREAM](http://httpwg.org/specs/rfc7540.html#RST_STREAM) 帧可以包含 [REFUSED\_STREAM](http://httpwg.org/specs/rfc7540.html#REFUSED_STREAM) 错误码，表明未处理任何请求，流已被关闭。在已被重置的流上发送的任何请求都可以安全地重试。


> Requests that have not been processed have not failed; clients MAY automatically retry them, even those with non-idempotent methods.

未经处理的请求不算失败，客户端可以自动重试这些请求，即使其是非幂等的。

> A server MUST NOT indicate that a stream has not been processed unless it can guarantee that fact. If frames that are on a stream are passed to the application layer for any stream, then [REFUSED_STREAM](http://httpwg.org/specs/rfc7540.html#RST_STREAM) MUST NOT be used for that stream, and a [GOAWAY](http://httpwg.org/specs/rfc7540.html#GOAWAY) frame MUST include a stream identifier that is greater than or equal to the given stream identifier.

服务端不能说明某个流未被处理，除非它能保证这一事实。如果某个流上的帧被传给应用层的任意一个流，那么 [REFUSED\_STREAM](http://httpwg.org/specs/rfc7540.html#RST_STREAM) 错误码不能用于该流，且 [GOAWAY](http://httpwg.org/specs/rfc7540.html#GOAWAY) 帧包含的流标识符必须大于等于该给定流的标识符。


> In addition to these mechanisms, the [PING](http://httpwg.org/specs/rfc7540.html#PING) frame provides a way for a client to easily test a connection. Connections that remain idle can become broken as some middleboxes (for instance, network address translators or load balancers) silently discard connection bindings. The [PING](http://httpwg.org/specs/rfc7540.html#PING) frame allows a client to safely test whether a connection is still active without sending a request.

除了这些机制之外，[PING](http://httpwg.org/specs/rfc7540.html#PING) 帧还提供了让客户端方便地测试连接的方法。空闲连接会由于一些中间设备(例如，网络地址转换器或者负载均衡器)默默地丢弃连接绑定而断掉。[PING](http://httpwg.org/specs/rfc7540.html#PING) 帧让客户端可以不用发送请求就能安全地测试连接是否仍然有效。

