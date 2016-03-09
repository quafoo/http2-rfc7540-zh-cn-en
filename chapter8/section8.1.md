# HTTP Request/Response Exchange
> A client sends an HTTP request on a new stream, using a previously unused stream identifier ([Section 5.1.1](http://httpwg.org/specs/rfc7540.html#StreamIdentifiers)). A server sends an HTTP response on the same stream as the request.

客户端在一个新流上发送一个HTTP请求，该新流使用了先前未使用的流标识符( [5.1.1节](http://httpwg.org/specs/rfc7540.html#StreamIdentifiers) )。服务端在同样的流上发送一个HTTP响应。


> An HTTP message (request or response) consists of:
> 
> 1. for a response only, zero or more [HEADERS](http://httpwg.org/specs/rfc7540.html#HEADERS) frames (each followed by zero or more [CONTINUATION](http://httpwg.org/specs/rfc7540.html#CONTINUATION) frames) containing the message headers of informational (1xx) HTTP responses (see [*[RFC7230]*](http://httpwg.org/specs/rfc7540.html#RFC7230), [Section 3.2](http://httpwg.org/specs/rfc7230.html#header.fields) and [*[RFC7231]*](http://httpwg.org/specs/rfc7540.html#RFC7231), [Section 6.2](http://httpwg.org/specs/rfc7231.html#status.1xx)),
> 
> 2. one [HEADERS](http://httpwg.org/specs/rfc7540.html#HEADERS) frame (followed by zero or more [CONTINUATION](http://httpwg.org/specs/rfc7540.html#CONTINUATION) frames) containing the message headers (see [*[RFC7230]*](http://httpwg.org/specs/rfc7540.html#RFC7230), [Section 3.2](http://httpwg.org/specs/rfc7230.html#header.fields)),
> 
> 3. zero or more [DATA](http://httpwg.org/specs/rfc7540.html#DATA) frames containing the payload body (see [*[RFC7230]*](http://httpwg.org/specs/rfc7540.html#RFC7230), [Section 3.3](http://httpwg.org/specs/rfc7230.html#message.body)), and
> 
> 4. optionally, one [HEADERS](http://httpwg.org/specs/rfc7540.html#HEADERS) frame, followed by zero or more [CONTINUATION](http://httpwg.org/specs/rfc7540.html#CONTINUATION) frames containing the trailer-part, if present (see [*[RFC7230]*](http://httpwg.org/specs/rfc7540.html#RFC7230), [Section 4.1.2](http://httpwg.org/specs/rfc7230.html#chunked.trailer.part)).

一个HTTP消息(请求或响应)包括：

1. 仅仅对响应而言，零个或多个 [HEADERS](http://httpwg.org/specs/rfc7540.html#HEADERS) 帧(每个后面跟随零个或多个 [CONTINUATION](http://httpwg.org/specs/rfc7540.html#CONTINUATION) 帧)，这些帧包含信息性(1xx)HTTP响应的消息首部(参见 [*[RFC7230]*](http://httpwg.org/specs/rfc7540.html#RFC7230)，[3.2节](http://httpwg.org/specs/rfc7230.html#header.fields)，和 [*[RFC7231]*](http://httpwg.org/specs/rfc7540.html#RFC7231)，[6.2节](http://httpwg.org/specs/rfc7231.html#status.1xx))，
2. 一个包含消息首部(参见 [*[RFC7230]*](http://httpwg.org/specs/rfc7540.html#RFC7230), [3.2节](http://httpwg.org/specs/rfc7230.html#header.fields))的 [HEADERS](http://httpwg.org/specs/rfc7540.html#HEADERS) 帧(后面跟随零个或多个 [CONTINUATION](http://httpwg.org/specs/rfc7540.html#CONTINUATION) 帧)，
3. 包含负载体(参见 [*[RFC7230]*](http://httpwg.org/specs/rfc7540.html#RFC7230)，[3.3节](http://httpwg.org/specs/rfc7230.html#message.body))的零个或多个 [DATA](http://httpwg.org/specs/rfc7540.html#DATA) 帧，和
4. 可选地，一个 [HEADERS](http://httpwg.org/specs/rfc7540.html#HEADERS) 帧，后面跟随零个或多个 [CONTINUATION](http://httpwg.org/specs/rfc7540.html#CONTINUATION) 帧，如果有(参见 [*[RFC7230]*](http://httpwg.org/specs/rfc7540.html#RFC7230)，[4.1.2节](http://httpwg.org/specs/rfc7230.html#chunked.trailer.part))，也包括拖挂部分。


> The last frame in the sequence bears an END\_STREAM flag, noting that a [HEADERS](http://httpwg.org/specs/rfc7540.html#HEADERS) frame bearing the END\_STREAM flag can be followed by [CONTINUATION](http://httpwg.org/specs/rfc7540.html#CONTINUATION) frames that carry any remaining portions of the header block.

序列的最后一帧带有END\_STREAM标志，注意带有END\_STREAM标志的 [HEADERS](http://httpwg.org/specs/rfc7540.html#HEADERS) 帧后面可以跟装载首部块剩余部分的 [CONTINUATION](http://httpwg.org/specs/rfc7540.html#CONTINUATION) 帧。


> Other frames (from any stream) MUST NOT occur between the [HEADERS](http://httpwg.org/specs/rfc7540.html#HEADERS) frame and any [CONTINUATION](http://httpwg.org/specs/rfc7540.html#CONTINUATION) frames that might follow.

无论是来自哪个流的其它的帧都不能出现在 [HEADERS](http://httpwg.org/specs/rfc7540.html#HEADERS) 帧和后面可能跟随的 [CONTINUATION](http://httpwg.org/specs/rfc7540.html#CONTINUATION) 帧之间。


> HTTP/2 uses DATA frames to carry message payloads. The chunked transfer encoding defined in [Section 4.1](http://httpwg.org/specs/rfc7230.html#chunked.encoding) of [*[RFC7230]*](http://httpwg.org/specs/rfc7540.html#RFC7230) MUST NOT be used in HTTP/2.

HTTP/2使用DATA帧来传送消息负载。[*[RFC7230]*](http://httpwg.org/specs/rfc7540.html#RFC7230) [4.1节](http://httpwg.org/specs/rfc7230.html#chunked.encoding) 定义的分块传输编码不能在HTTP/2中使用。


> Trailing header fields are carried in a header block that also terminates the stream. Such a header block is a sequence starting with a [HEADERS](http://httpwg.org/specs/rfc7540.html#HEADERS) frame, followed by zero or more [CONTINUATION](http://httpwg.org/specs/rfc7540.html#CONTINUATION) frames, where the [HEADERS](http://httpwg.org/specs/rfc7540.html#HEADERS) frame bears an END_STREAM flag. Header blocks after the first that do not terminate the stream are not part of an HTTP request or response.

拖尾首部字段在终结流的首部块里传送。这样的首部块是一个序列，从一个 [HEADERS](http://httpwg.org/specs/rfc7540.html#HEADERS) 帧开始，后跟零个或多个 [CONTINUATION](http://httpwg.org/specs/rfc7540.html#CONTINUATION) 帧，其中 [HEADERS](http://httpwg.org/specs/rfc7540.html#HEADERS) 帧携带一个END\_STREAM标志。没有终结流的首部块之后的首部块不属于HTTP请求或响应的一部分。


> A [HEADERS](http://httpwg.org/specs/rfc7540.html#HEADERS) frame (and associated [CONTINUATION](http://httpwg.org/specs/rfc7540.html#CONTINUATION) frames) can only appear at the start or end of a stream. An endpoint that receives a [HEADERS](http://httpwg.org/specs/rfc7540.html#HEADERS) frame without the END_STREAM flag set after receiving a final (non-informational) status code MUST treat the corresponding request or response as malformed ([Section 8.1.2.6](http://httpwg.org/specs/rfc7540.html#malformed)).

[HEADERS](http://httpwg.org/specs/rfc7540.html#HEADERS) 帧(和相关联的 [CONTINUATION](http://httpwg.org/specs/rfc7540.html#CONTINUATION) 帧)只能出现在流的开始或结束。如果端点在收到一个最终的(非信息性的)状态码以后又收到一个没有设置END\_STREAM标志的 [HEADERS](http://httpwg.org/specs/rfc7540.html#HEADERS) 帧，必须将相应的请求或响应当做是有缺陷的( [8.1.2.6节](http://httpwg.org/specs/rfc7540.html#malformed) )。


> An HTTP request/response exchange fully consumes a single stream. A request starts with the [HEADERS](http://httpwg.org/specs/rfc7540.html#HEADERS) frame that puts the stream into an "open" state. The request ends with a frame bearing END\_STREAM, which causes the stream to become "half-closed (local)" for the client and "half-closed (remote)" for the server. A response starts with a [HEADERS](http://httpwg.org/specs/rfc7540.html#HEADERS) frame and ends with a frame bearing END\_STREAM, which places the stream in the "closed" state.

一个完整的HTTP请求/响应交换流程需要消耗单独的一个流。请求从 [HEADERS](http://httpwg.org/specs/rfc7540.html#HEADERS) 帧开始，将流置于 打开(open) 状态。请求以携带END\_STREAM标志的帧结束，从而对客户端来说，流进入 半关闭(本地)(half-closed (local)) 状态，对服务端来说，流进入 半关闭(远端)(half-closed (remote)) 状态。响应从一个 [HEADERS](http://httpwg.org/specs/rfc7540.html#HEADERS) 帧开始，以一个携带END\_STREAM标志的帧结束，然后流进入 关闭(closed) 状态。


> An HTTP response is complete after the server sends — or the client receives — a frame with the END\_STREAM flag set (including any [CONTINUATION](http://httpwg.org/specs/rfc7540.html#CONTINUATION) frames needed to complete a header block). A server can send a complete response prior to the client sending an entire request if the response does not depend on any portion of the request that has not been sent and received. When this is true, a server MAY request that the client abort transmission of a request without error by sending a [RST\_STREAM](http://httpwg.org/specs/rfc7540.html#RST_STREAM) with an error code of [NO\_ERROR](http://httpwg.org/specs/rfc7540.html#NO_ERROR) after sending a complete response (i.e., a frame with the END\_STREAM flag). Clients MUST NOT discard responses as a result of receiving such a [RST_STREAM](http://httpwg.org/specs/rfc7540.html#RST_STREAM), though clients can always discard responses at their discretion for other reasons.

服务端发送或者客户端接收一个设置了END\_STREAM标志的帧(包括任何传送完成首部块的 [CONTINUATION](http://httpwg.org/specs/rfc7540.html#CONTINUATION) 帧)以后，HTTP响应结束。如果响应完全不依赖于还未发送和接收的请求，那么在客户端发送整个请求之前，服务端可以发送一个完整的响应。这种情况下，发送完一个完整的响应(即，携带END\_STREAM标志的帧)以后，通过发送一个错误码为 [NO\_ERROR](http://httpwg.org/specs/rfc7540.html#NO_ERROR) 的 [RST\_STREAM](http://httpwg.org/specs/rfc7540.html#RST_STREAM) 帧，服务端可以要求客户端取消传送请求。尽管客户端总是可以因为其它原因而按自己的意愿丢弃响应，但客户端不能因为收到了这样一个 [RST\_STREAM](http://httpwg.org/specs/rfc7540.html#RST_STREAM) 帧就丢弃响应。