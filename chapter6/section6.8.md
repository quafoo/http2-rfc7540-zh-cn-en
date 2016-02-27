# GOAWAY / GOAWAY帧
> The GOAWAY frame (type=0x7) is used to initiate shutdown of a connection or to signal serious error conditions. GOAWAY allows an endpoint to gracefully stop accepting new streams while still finishing processing of previously established streams. This enables administrative actions, like server maintenance.

GOAWAY帧(type=0x7)用于发起关闭连接，或者警示严重错误。GOAWAY帧能让端点优雅地停止接受新流，同时仍然能处理完先前建立的流。这使类似于服务端维护的管理行为成为可能。


> There is an inherent race condition between an endpoint starting new streams and the remote sending a GOAWAY frame. To deal with this case, the GOAWAY contains the stream identifier of the last peer-initiated stream that was or might be processed on the sending endpoint in this connection. For instance, if the server sends a GOAWAY frame, the identified stream is the highest-numbered stream initiated by the client.

在发起新流的端点和发送GOAWAY帧的远端之间，内在地存在着竞争条件。为了解决这个问题，GOAWAY帧包含对端最后初始创建的流的标识符，该流被或者可能被连接的发送端处理。例如，如果服务端发送一个GOAWAY帧，被标识的流就是由客户端初始创建的最大标号的流。


> Once sent, the sender will ignore frames sent on streams initiated by the receiver if the stream has an identifier higher than the included last stream identifier. Receivers of a GOAWAY frame MUST NOT open additional streams on the connection, although a new connection can be established for new streams.

一旦GOAWAY帧被发送，如果接收端初始创建的流的标识符大于GOAWAY帧携带的最后的流标识符，发送端将会忽略在这些流上发送的帧。尽管可以在新的连接上创建新的流，但是GOAWAY帧的接收端不能在原有的连接上再打开其他的流。


> If the receiver of the GOAWAY has sent data on streams with a higher stream identifier than what is indicated in the GOAWAY frame, those streams are not or will not be processed. The receiver of the GOAWAY frame can treat the streams as though they had never been created at all, thereby allowing those streams to be retried later on a new connection.

如果GOAWAY帧的接收端在流上发送数据，这些流的流标识符比GOAWAY帧里携带的流标识符大，那么这些流将不会被处理。GOAWAY帧的接收端可以将这些流看做从来没有创建过一样，因此，这些流随后可以在一个新的连接上重新被创建。


> Endpoints SHOULD always send a GOAWAY frame before closing a connection so that the remote peer can know whether a stream has been partially processed or not. For example, if an HTTP client sends a POST at the same time that a server closes a connection, the client cannot know if the server started to process that POST request if the server does not send a GOAWAY frame to indicate what streams it might have acted on.

在关闭连接之前，端点应该总是发送一个GOAWAY帧，从而可以让远端知道流是否被部分地处理了。例如，假设在服务端关闭连接的同时，HTTP客户端发送了一个POST请求，如果服务端没有发送GOAWAY帧来指明关闭连接影响到了哪些流，客户端不可能知道服务端是否开始处理该POST请求。


> An endpoint might choose to close a connection without sending a GOAWAY for misbehaving peers.

如果对端出错了，一端可以不用发送一个GOAWAY帧而关闭连接。


> A GOAWAY frame might not immediately precede closing of the connection; a receiver of a GOAWAY that has no more use for the connection SHOULD still send a GOAWAY frame before terminating the connection.

> ```
> +-+-------------------------------------------------------------+
> |R|                  Last-Stream-ID (31)                        |
> +-+-------------------------------------------------------------+
> |                      Error Code (32)                          |
> +---------------------------------------------------------------+
> |                  Additional Debug Data (*)                    |
> +---------------------------------------------------------------+
> 					Figure 13: GOAWAY Payload Format
```

GOAWAY帧之后可能不会立即关闭连接。GOAWAY帧的接收端应该在关闭连接之前仍然发送一个GOAWAY帧。

```
+-+-------------------------------------------------------------+
|R|                  Last-Stream-ID (31)                        |
+-+-------------------------------------------------------------+
|                      Error Code (32)                          |
+---------------------------------------------------------------+
|                  Additional Debug Data (*)                    |
+---------------------------------------------------------------+
					  图 13: GOAWAY帧负载格式
```


> The GOAWAY frame does not define any flags.

GOAWAY帧没有定义任何标志。


> The GOAWAY frame applies to the connection, not a specific stream. An endpoint MUST treat a GOAWAY frame with a stream identifier other than 0x0 as a connection error ([Section 5.4.1](http://httpwg.org/specs/rfc7540.html#ConnectionErrorHandler)) of type [PROTOCOL_ERROR](http://httpwg.org/specs/rfc7540.html#PROTOCOL_ERROR).

GOAWAY帧作用于连接，而不是流。如果GOAWAY帧的流标识符不是0x0，一端必须将其当做类型为 [PROTOCOL_ERROR](http://httpwg.org/specs/rfc7540.html#PROTOCOL_ERROR) 的连接错误( [5.4.1节](http://httpwg.org/specs/rfc7540.html#ConnectionErrorHandler) )。


> The last stream identifier in the GOAWAY frame contains the highest-numbered stream identifier for which the sender of the GOAWAY frame might have taken some action on or might yet take action on. All streams up to and including the identified stream might have been processed in some way. The last stream identifier can be set to 0 if no streams were processed.

GOAWAY帧里最后的流标识符包含最大数字的流标识符，GOAWAY帧的发送者可能已经、或者仍然在作用于该流。小于等于该标识符的流以某种方式被处理。如果没有流被处理，最后的流标识符可以被设置为0。


> Note: In this context, "processed" means that some data from the stream was passed to some higher layer of software that might have taken some action as a result.

注意：在这个上下文里，『被处理』表示该流传送的一些数据被传递给软件的更高层，结果导致了采取了一些行动。


> If a connection terminates without a GOAWAY frame, the last stream identifier is effectively the highest possible stream identifier.

如果没有GOAWAY帧就关闭连接，最后的流标识符实际上就是最大可能的流标识符。


> On streams with lower- or equal-numbered identifiers that were not closed completely prior to the connection being closed, reattempting requests, transactions, or any protocol activity is not possible, with the exception of idempotent actions like HTTP GET, PUT, or DELETE. Any protocol activity that uses higher-numbered streams can be safely retried using a new connection.

连接关闭之前，在没有完全关闭的、流标识符小于等于最大值的流上，重新尝试请求、事务，或者任何协议行为是不可能的，但诸如HTTP GET、PUT、或者DELETE等幂等动作除外。在流标识符大于最大值的流上的任何协议动作都可以在一个新连接上安全地重试。


> Activity on streams numbered lower or equal to the last stream identifier might still complete successfully. The sender of a GOAWAY frame might gracefully shut down a connection by sending a GOAWAY frame, maintaining the connection in an "open" state until all in-progress streams complete.

在小于等于最后的流标识符的流上的行为可能仍然能成功地完成。GOAWAY帧的发送端可能会通过发送一个GOAWAY帧来优雅地关闭连接，同时保持连接在 打开(open) 状态，直到所有的流都完成。


> An endpoint MAY send multiple GOAWAY frames if circumstances change. For instance, an endpoint that sends GOAWAY with [NO_ERROR](http://httpwg.org/specs/rfc7540.html#NO_ERROR) during graceful shutdown could subsequently encounter a condition that requires immediate termination of the connection. The last stream identifier from the last GOAWAY frame received indicates which streams could have been acted upon. Endpoints MUST NOT increase the value they send in the last stream identifier, since the peers might already have retried unprocessed requests on another connection.

如果环境变化了，一端可能会发送多个GOAWAY帧。例如，在优雅地关闭期间，发送带有 [NO_ERROR](http://httpwg.org/specs/rfc7540.html#NO_ERROR) 的GOAWAY帧的端点可能随后就会遭遇需要立即关闭连接的情况。收到的最后的GOAWAY帧上最后的流标识符指明哪些流受影响了。端点不能增加它们发送的最后的流标识符的值，因为对端可能已经在另外一个连接上重试未处理的请求了。


> A client that is unable to retry requests loses all requests that are in flight when the server closes the connection. This is especially true for intermediaries that might not be serving clients using HTTP/2. A server that is attempting to gracefully shut down a connection SHOULD send an initial GOAWAY frame with the last stream identifier set to 2^31 - 1 and a [NO_ERROR](http://httpwg.org/specs/rfc7540.html#NO_ERROR) code. This signals to the client that a shutdown is imminent and that initiating further requests is prohibited. After allowing time for any in-flight stream creation (at least one round-trip time), the server can send another GOAWAY frame with an updated last stream identifier. This ensures that a connection can be cleanly shut down without losing requests.

当服务端关闭连接时，不能重试请求的客户端会丢失所有传输途中的请求。这对不向客户端提供HTTP/2服务的中介来说尤其正确。试图优雅地关闭连接的服务端应该发送一个初始GOAWAY帧，其 最后的流标识符(last stream identifier) 设置为2^31 - 1，并且带有一个 [NO_ERROR](http://httpwg.org/specs/rfc7540.html#NO_ERROR) 码。这会通知客户端，连接即将关闭，禁止再发起新的请求。一段可以让传输途中的流创建完成的时间(至少一个往返时间)过后，服务端可以发送其它的GOAWAY帧，携带更新的 最后流标识符(last time identifier) 。这样可以确保干净地关闭连接，不会丢失请求。


> After sending a GOAWAY frame, the sender can discard frames for streams initiated by the receiver with identifiers higher than the identified last stream. However, any frames that alter connection state cannot be completely ignored. For instance, [HEADERS](http://httpwg.org/specs/rfc7540.html#HEADERS), [PUSH_PROMISE](http://httpwg.org/specs/rfc7540.html#PUSH_PROMISE), and [CONTINUATION](http://httpwg.org/specs/rfc7540.html#CONTINUATION) frames MUST be minimally processed to ensure the state maintained for header compression is consistent (see [Section 4.3](http://httpwg.org/specs/rfc7540.html#HeaderBlock)); similarly, DATA frames MUST be counted toward the connection flow-control window. Failure to process these frames can cause flow control or header compression state to become unsynchronized.

发送了GOAWAY帧以后，发送端可以丢弃接收端初始创建的、其标识符大于最后的流标识符的流上的帧。但是，改变连接状态的帧不能被完全忽略。例如，[HEADERS](http://httpwg.org/specs/rfc7540.html#HEADERS) 帧，[PUSH_PROMISE](http://httpwg.org/specs/rfc7540.html#PUSH_PROMISE) 帧，和 [CONTINUATION](http://httpwg.org/specs/rfc7540.html#CONTINUATION) 帧，必须至少被处理，以确保为首部压缩维护的状态是一致(参见 [4.3节](http://httpwg.org/specs/rfc7540.html#HeaderBlock) )。类似地，必须将DATA帧计入连接的流量控制窗口。对这些帧的处理失败会导致流量控制或者首部压缩状态变得不同步。


> The GOAWAY frame also contains a 32-bit error code ([Section 7](http://httpwg.org/specs/rfc7540.html#ErrorCodes)) that contains the reason for closing the connection.

GOAWAY帧还包含一个32bit的错误码( [第7章](http://httpwg.org/specs/rfc7540.html#ErrorCodes) )，该错误码包含了关闭连接的原因。


> Endpoints MAY append opaque data to the payload of any GOAWAY frame. Additional debug data is intended for diagnostic purposes only and carries no semantic value. Debug information could contain security- or privacy-sensitive data. Logged or otherwise persistently stored debug data MUST have adequate safeguards to prevent unauthorized access.

端点可以在任何GOAWAY帧的负载上附加不透明的数据。额外的调试数据只用于诊断目的，并且不携带语义值。调试信息可以包含安全相关的、或者隐私敏感的数据。登陆相关的、或者其他持久存储的调试数据必须具备足够的安全措施，以阻止未授权访问。