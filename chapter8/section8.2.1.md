# Push Requests / 推送请求
> Server push is semantically equivalent to a server responding to a request; however, in this case, that request is also sent by the server, as a [PUSH_PROMISE](http://httpwg.org/specs/rfc7540.html#PUSH_PROMISE) frame.

服务端推送在语义上等价于服务端对请求的响应。但在本小节，请求也被服务端以 [PUSH\_PROMISE](http://httpwg.org/specs/rfc7540.html#PUSH_PROMISE) 帧的形式发送。


> The [PUSH_PROMISE](http://httpwg.org/specs/rfc7540.html#PUSH_PROMISE) frame includes a header block that contains a complete set of request header fields that the server attributes to the request. It is not possible to push a response to a request that includes a request body.

[PUSH\_PROMISE](http://httpwg.org/specs/rfc7540.html#PUSH_PROMISE) 帧包含一个首部块，该首部块包含完整的一套请求首部字段，服务端将这些字段归因于请求。不可能向包含请求体的请求推送响应。


> Pushed responses are always associated with an explicit request from the client. The [PUSH\_PROMISE](http://httpwg.org/specs/rfc7540.html#PUSH_PROMISE) frames sent by the server are sent on that explicit request's stream. The [PUSH_PROMISE](http://httpwg.org/specs/rfc7540.html#PUSH_PROMISE) frame also includes a promised stream identifier, chosen from the stream identifiers available to the server (see [Section 5.1.1](http://httpwg.org/specs/rfc7540.html#StreamIdentifiers)).

被推送的响应总是和明显来自于客户端的请求有关联。服务端在该请求所在的流上发送 [PUSH\_PROMISE](http://httpwg.org/specs/rfc7540.html#PUSH_PROMISE) 帧。[PUSH\_PROMISE](http://httpwg.org/specs/rfc7540.html#PUSH_PROMISE) 帧也包含一个被允诺的流标识符，该流标识符是从服务端可用的流标识符里选出来的( [5.1.1节](http://httpwg.org/specs/rfc7540.html#StreamIdentifiers) )。


> The header fields in [PUSH\_PROMISE](http://httpwg.org/specs/rfc7540.html#StreamIdentifiers) and any subsequent [CONTINUATION](http://httpwg.org/specs/rfc7540.html#CONTINUATION) frames MUST be a valid and complete set of request header fields ([Section 8.1.2.3](http://httpwg.org/specs/rfc7540.html#HttpRequest)). The server MUST include a method in the :method pseudo-header field that is safe and cacheable. If a client receives a [PUSH\_PROMISE](http://httpwg.org/specs/rfc7540.html#PUSH_PROMISE) that does not include a complete and valid set of header fields or the :method pseudo-header field identifies a method that is not safe, it MUST respond with a stream error ([Section 5.4.2](http://httpwg.org/specs/rfc7540.html#StreamErrorHandler)) of type [PROTOCOL_ERROR](http://httpwg.org/specs/rfc7540.html#PROTOCOL_ERROR).

[PUSH\_PROMISE](http://httpwg.org/specs/rfc7540.html#StreamIdentifiers) 帧和任何随后的 [CONTINUATION](http://httpwg.org/specs/rfc7540.html#CONTINUATION) 帧里的首部字段必须是完整有效的一套请求首部字段( [8.1.2.3节](http://httpwg.org/specs/rfc7540.html#HttpRequest) )。服务端必须在`:method`伪首部字段里包含一个安全的可缓存的方法。如果客户端收到了一个 [PUSH\_PROMISE](http://httpwg.org/specs/rfc7540.html#PUSH_PROMISE) 帧，其没有包含完整有效的一套首部字段，或者`:method`伪首部字段标识了一个不安全的方法，就必须响应一个类型为 [PROTOCOL\_ERROR](http://httpwg.org/specs/rfc7540.html#PROTOCOL_ERROR) 的流错误( [5.4.2节](http://httpwg.org/specs/rfc7540.html#StreamErrorHandler) )。


> The server SHOULD send [PUSH\_PROMISE](http://httpwg.org/specs/rfc7540.html#PUSH_PROMISE) ([Section 6.6](http://httpwg.org/specs/rfc7540.html#PUSH_PROMISE)) frames prior to sending any frames that reference the promised responses. This avoids a race where clients issue requests prior to receiving any [PUSH_PROMISE](http://httpwg.org/specs/rfc7540.html#PUSH_PROMISE) frames.

在发送任何跟被允诺的响应有关联的帧之前，服务端应该优先发送 [PUSH\_PROMISE](http://httpwg.org/specs/rfc7540.html#PUSH_PROMISE) 帧( [6.6节](http://httpwg.org/specs/rfc7540.html#PUSH_PROMISE) )。这就避免了一种竞态条件，即客户端在收到任何 [PUSH\_PROMISE](http://httpwg.org/specs/rfc7540.html#PUSH_PROMISE) 帧之前就发送请求。


> For example, if the server receives a request for a document containing embedded links to multiple image files and the server chooses to push those additional images to the client, sending [PUSH\_PROMISE](http://httpwg.org/specs/rfc7540.html#PUSH_PROMISE) frames before the [DATA](http://httpwg.org/specs/rfc7540.html#DATA) frames that contain the image links ensures that the client is able to see that a resource will be pushed before discovering embedded links. Similarly, if the server pushes responses referenced by the header block (for instance, in Link header fields), sending a [PUSH_PROMISE](http://httpwg.org/specs/rfc7540.html#PUSH_PROMISE) before sending the header block ensures that clients do not request those resources.

例如，如果服务端收到了一个对文档的请求，该文档包含内嵌的指向多个图片文件的链接，且服务端选择向客户端推送那些额外的图片，那么在发送包含图片链接的 [DATA](http://httpwg.org/specs/rfc7540.html#DATA) 帧之前发送 [PUSH\_PROMISE](http://httpwg.org/specs/rfc7540.html#PUSH_PROMISE) 帧可以确保客户端在发现内嵌的链接之前，能够知道有一个资源将要被推送过来。同样地，如果服务端推送被首部块引用的响应(比如，在链接的首部字段里)，在发送首部块之前发送一个 [PUSH\_PROMISE](http://httpwg.org/specs/rfc7540.html#PUSH_PROMISE) 帧，可以确保客户端不再请求那些资源。


> [PUSH_PROMISE](http://httpwg.org/specs/rfc7540.html#PUSH_PROMISE) frames MUST NOT be sent by the client.

客户端不能发送 [PUSH\_PROMISE](http://httpwg.org/specs/rfc7540.html#PUSH_PROMISE) 帧。


> [PUSH\_PROMISE](http://httpwg.org/specs/rfc7540.html#PUSH_PROMISE) frames can be sent by the server in response to any client-initiated stream, but the stream MUST be in either the "open" or "half-closed (remote)" state with respect to the server. [PUSH_PROMISE](http://httpwg.org/specs/rfc7540.html#PUSH_PROMISE) frames are interspersed with the frames that comprise a response, though they cannot be interspersed with [HEADERS](http://httpwg.org/specs/rfc7540.html#HEADERS) and [CONTINUATION](http://httpwg.org/specs/rfc7540.html#CONTINUATION) frames that comprise a single header block.

服务端可以发送 [PUSH\_PROMISE](http://httpwg.org/specs/rfc7540.html#PUSH_PROMISE) 帧，以响应任何客户端发起的流，但是相对于服务端该流必须处于 打开(open) 或者 半关闭(远端)(half-closed(remote)) 状态。[PUSH\_PROMISE](http://httpwg.org/specs/rfc7540.html#PUSH_PROMISE) 帧之间夹杂着包含响应的帧，但不能夹杂着包含一个单独的首部块的 [HEADERS](http://httpwg.org/specs/rfc7540.html#HEADERS) 帧 和 [CONTINUATION](http://httpwg.org/specs/rfc7540.html#CONTINUATION) 帧。


> Sending a [PUSH_PROMISE](http://httpwg.org/specs/rfc7540.html#PUSH_PROMISE) frame creates a new stream and puts the stream into the “reserved (local)” state for the server and the “reserved (remote)” state for the client.

发送一个 [PUSH\_PROMISE](http://httpwg.org/specs/rfc7540.html#PUSH_PROMISE) 帧会创建一个新流。在服务端，该流会进入 被保留的(本地)(reserved (local)) 状态；在客户端，该流会进入 被保留的(远端)(reserved (remote)) 状态。