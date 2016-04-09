# Push Responses / 推送响应
> After sending the [PUSH_PROMISE](http://httpwg.org/specs/rfc7540.html#PUSH_PROMISE) frame, the server can begin delivering the pushed response as a response ([Section 8.1.2.4](http://httpwg.org/specs/rfc7540.html#HttpResponse)) on a server-initiated stream that uses the promised stream identifier. The server uses this stream to transmit an HTTP response, using the same sequence of frames as defined in [Section 8.1](http://httpwg.org/specs/rfc7540.html#HttpSequence). This stream becomes "half-closed" to the client ([Section 5.1](http://httpwg.org/specs/rfc7540.html#StreamStates)) after the initial [HEADERS](http://httpwg.org/specs/rfc7540.html#HEADERS) frame is sent.

在发送 [PUSH\_PROMISE](http://httpwg.org/specs/rfc7540.html#PUSH_PROMISE) 帧以后，在一个服务端发起的、使用被允诺的流标识符的流上，服务端可以开始将被推送的响应( [8.1.2.4节](http://httpwg.org/specs/rfc7540.html#HttpResponse) )当做响应传送。服务端使用该流传送HTTP响应，帧顺序跟 [8.1节](http://httpwg.org/specs/rfc7540.html#HttpSequence) 定义的相同。在初始 [HEADERS](http://httpwg.org/specs/rfc7540.html#HEADERS) 帧被发送以后，对客户端( [5.1节](http://httpwg.org/specs/rfc7540.html#StreamStates) )来说，该流变为 半关闭(half-closed) 状态。


> Once a client receives a [PUSH_PROMISE](http://httpwg.org/specs/rfc7540.html#PUSH_PROMISE) frame and chooses to accept the pushed response, the client SHOULD NOT issue any requests for the promised response until after the promised stream has closed.

一旦客户端收到了 [PUSH\_PROMISE](http://httpwg.org/specs/rfc7540.html#PUSH_PROMISE) 帧，并选择接收被推送的响应，客户端就不应该为被允诺的响应发送任何请求，直到被允诺的流被关闭以后。


> If the client determines, for any reason, that it does not wish to receive the pushed response from the server or if the server takes too long to begin sending the promised response, the client can send a [RST_STREAM](http://httpwg.org/specs/rfc7540.html#RST_STREAM) frame, using either the [CANCEL](http://httpwg.org/specs/rfc7540.html#CANCEL) or [REFUSED\_STREAM](http://httpwg.org/specs/rfc7540.html#REFUSED_STREAM) code and referencing the pushed stream's identifier.

不管出于什么原因，如果客户端决定不再从服务端接收被推送的响应，或者如果服务端花费了太长时间准备发送被允诺的响应，客户端可以发送一个 [RST\_STREAM](http://httpwg.org/specs/rfc7540.html#RST_STREAM) 帧，该帧可以使用 [CANCEL](http://httpwg.org/specs/rfc7540.html#CANCEL) 或者 [REFUSED\_STEAM](http://httpwg.org/specs/rfc7540.html#REFUSED_STREAM) 码，并引用被推送的流标识符。


> A client can use the [SETTINGS\_MAX\_CONCURRENT\_STREAMS](http://httpwg.org/specs/rfc7540.html#SETTINGS_MAX_CONCURRENT_STREAMS) setting to limit the number of responses that can be concurrently pushed by a server. Advertising a [SETTINGS\_MAX\_CONCURRENT\_STREAMS](http://httpwg.org/specs/rfc7540.html#SETTINGS_MAX_CONCURRENT_STREAMS) value of zero disables server push by preventing the server from creating the necessary streams. This does not prohibit a server from sending [PUSH_PROMISE](http://httpwg.org/specs/rfc7540.html#PUSH_PROMISE) frames; clients need to reset any promised streams that are not wanted.

客户端可以使用 [SETTINGS\_MAX\_CONCURRENT\_STREAMS](http://httpwg.org/specs/rfc7540.html#SETTINGS_MAX_CONCURRENT_STREAMS) 设置项来限制服务端并行推送响应的数量。通告 [SETTINGS\_MAX\_CONCURRENT\_STREAMS](http://httpwg.org/specs/rfc7540.html#SETTINGS_MAX_CONCURRENT_STREAMS) 的值为零，会通过阻止服务端创建必要的流的方式来关闭服务端推送功能。这不会阻止服务端发送 [PUSH\_PROMISE](http://httpwg.org/specs/rfc7540.html#PUSH_PROMISE) 帧；客户端需要重置任何不想要的被允诺的流。


> Clients receiving a pushed response MUST validate that either the server is authoritative (see [Section 10.1](http://httpwg.org/specs/rfc7540.html#authority)) or the proxy that provided the pushed response is configured for the corresponding request. For example, a server that offers a certificate for only the example.com DNS-ID or Common Name is not permitted to push a response for https://www.example.org/doc .

收到被推送响应的客户端必须确认，要么服务端是可信的(参见 [10.1节](http://httpwg.org/specs/rfc7540.html#authority) )，要么提供被推送响应的代理为相应的请求做了配置。例如，只为 example.com 的DNS-ID或者域名提供证书的服务端不准向 https://www.example.org/doc 推送响应。


> The response for a [PUSH\_PROMISE](http://httpwg.org/specs/rfc7540.html#PUSH_PROMISE) stream begins with a [HEADERS](http://httpwg.org/specs/rfc7540.html#HEADERS) frame, which immediately puts the stream into the "half-closed (remote)" state for the server and "half-closed (local)" state for the client, and ends with a frame bearing END_STREAM, which places the stream in the "closed" state.

[PUSH\_PROMISE](http://httpwg.org/specs/rfc7540.html#PUSH_PROMISE) 流的响应以 [HEADERS](http://httpwg.org/specs/rfc7540.html#HEADERS) 帧开始，这会立即将流在服务端置于 半关闭(远端)(half-closed(remote)) 状态，在客户端置于 半关闭(本地)(half-closed(local)) 状态，最后以携带 END\_STREAM 的帧结束，这会将流置于 关闭(closed) 状态。


> Note: The client never sends a frame with the END_STREAM flag for a server push.

注意：客户端从来不会为服务端推送发送带有 END\_STREAM 标志的帧。