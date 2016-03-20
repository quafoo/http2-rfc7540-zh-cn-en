# Server Push / 服务端推送
> HTTP/2 allows a server to pre-emptively send (or "push") responses (along with corresponding "promised" requests) to a client in association with a previous client-initiated request. This can be useful when the server knows the client will need to have those responses available in order to fully process the response to the original request.

HTTP/2允许服务端抢先向客户端发送(或『推送』)响应(以及相应的『被允诺的』请求)，这些响应跟先前客户端发起的请求有关。为了完整地处理对最初请求的响应，客户端将需要服务端推送的响应，当服务端了解到这一点时，服务端推送功能就会是有用的。

> A client can request that server push be disabled, though this is negotiated for each hop independently. The [SETTINGS\_ENABLE\_PUSH](http://httpwg.org/specs/rfc7540.html#SETTINGS_ENABLE_PUSH) setting can be set to 0 to indicate that server push is disabled.

客户端可以要求关闭服务端推送功能，但这是每一跳独立协商地。[SETTINGS\_ENABLE\_PUSH](http://httpwg.org/specs/rfc7540.html#SETTINGS_ENABLE_PUSH) 设置为0，表示关闭服务端推送功能。

> Promised requests MUST be cacheable (see [*[RFC7231]*](http://httpwg.org/specs/rfc7540.html#RFC7231), [Section 4.2.3](http://httpwg.org/specs/rfc7231.html#cacheable.methods)), MUST be safe (see [*[RFC7231]*](http://httpwg.org/specs/rfc7540.html#RFC7231), [Section 4.2.1](http://httpwg.org/specs/rfc7231.html#safe.methods)), and MUST NOT include a request body. Clients that receive a promised request that is not cacheable, that is not known to be safe, or that indicates the presence of a request body MUST reset the promised stream with a stream error ([Section 5.4.2](http://httpwg.org/specs/rfc7540.html#StreamErrorHandler)) of type [PROTOCOL_ERROR](http://httpwg.org/specs/rfc7540.html#PROTOCOL_ERROR). Note this could result in the promised stream being reset if the client does not recognize a newly defined method as being safe.

被允诺的请求必须是可缓存的(参见 [*[RFC7231]*](http://httpwg.org/specs/rfc7540.html#RFC7231)，[4.2.3节](http://httpwg.org/specs/rfc7231.html#cacheable.methods) )，必须是安全的(参见 [*[RFC7231]*](http://httpwg.org/specs/rfc7540.html#RFC7231)，[4.2.1节](http://httpwg.org/specs/rfc7231.html#safe.methods) )，而且不能包含请求体。如果客户端收到的被允诺的请求是不可缓存的，不安全的，或者含有请求体，那么客户端必须使用类型为 [PROTOCOL\_ERROR](http://httpwg.org/specs/rfc7540.html#PROTOCOL_ERROR) 的流错误( [5.4.2节](http://httpwg.org/specs/rfc7540.html#StreamErrorHandler) )来重置该被允诺的流。注意，如果客户端认为一个新定义的方法是不安全的，这将会导致被允诺的流被重置。

> Pushed responses that are cacheable (see [*[RFC7234]*](http://httpwg.org/specs/rfc7540.html#RFC7234), [Section 3](http://httpwg.org/specs/rfc7234.html#response.cacheability)) can be stored by the client, if it implements an HTTP cache. Pushed responses are considered successfully validated on the origin server (e.g., if the "no-cache" cache response directive is present ([*[RFC7234]*](http://httpwg.org/specs/rfc7540.html#RFC7234), [Section 5.2.2](http://httpwg.org/specs/rfc7234.html#cache-response-directive))) while the stream identified by the promised stream ID is still open.

如果客户端实现了HTTP缓存功能，就可以存储被推送的、可缓存的(参见 [*[RFC7234]*](http://httpwg.org/specs/rfc7540.html#RFC7234)，[第3章](http://httpwg.org/specs/rfc7234.html#response.cacheability) )响应。被推送的响应被认为在源服务器上已被成功地验证过(例如，如果存在"no-cache"缓存响应指令( [*[RFC7234]*](http://httpwg.org/specs/rfc7540.html#RFC7234)，[5.2.2节](http://httpwg.org/specs/rfc7234.html#cache-response-directive) ))，而被允诺的流ID标识的流仍处于打开状态。

> Pushed responses that are not cacheable MUST NOT be stored by any HTTP cache. They MAY be made available to the application separately.

任何HTTP缓存都不能存储不可缓存的被推送响应。这些响应可以单独提供给应用程序。

> The server MUST include a value in the :authority pseudo-header field for which the server is authoritative (see [Section 10.1](http://httpwg.org/specs/rfc7540.html#authority)). A client MUST treat a [PUSH_PROMISE](http://httpwg.org/specs/rfc7540.html#PUSH_PROMISE) for which the server is not authoritative as a stream error ([Section 5.4.2](http://httpwg.org/specs/rfc7540.html#StreamErrorHandler)) of type [PROTOCOL\_ERROR](http://httpwg.org/specs/rfc7540.html#PROTOCOL_ERROR).

服务端必须在`:authority`伪首部字段中包含一个值，以表明服务端是权威可信的(参见 [10.1节](http://httpwg.org/specs/rfc7540.html#authority) )。客户端必须将不可信的服务端的 [PUSH\_PROMISE](http://httpwg.org/specs/rfc7540.html#PUSH_PROMISE) 帧当做类型为 [PROTOCOL\_ERROR](http://httpwg.org/specs/rfc7540.html#PROTOCOL_ERROR) 的流错误( [5.4.2节](http://httpwg.org/specs/rfc7540.html#StreamErrorHandler) )。

> An intermediary can receive pushes from the server and choose not to forward them on to the client. In other words, how to make use of the pushed information is up to that intermediary. Equally, the intermediary might choose to make additional pushes to the client, without any action taken by the server.

中介可以接收来自服务端的推送，并选择不向客户端转发这些推送。换句话说，怎样利用被推送来的信息取决于该中介。同样，中介可以选择向客户端做额外的推送，不需要服务端采取任何行动。

> A client cannot push. Thus, servers MUST treat the receipt of a [PUSH\_PROMISE](http://httpwg.org/specs/rfc7540.html#PUSH_PROMISE) frame as a connection error ([Section 5.4.1](http://httpwg.org/specs/rfc7540.html#ConnectionErrorHandler)) of type [PROTOCOL\_ERROR](http://httpwg.org/specs/rfc7540.html#PROTOCOL_ERROR). Clients MUST reject any attempt to change the [SETTINGS\_ENABLE\_PUSH](http://httpwg.org/specs/rfc7540.html#SETTINGS_ENABLE_PUSH) setting to a value other than 0 by treating the message as a connection error ([Section 5.4.1](http://httpwg.org/specs/rfc7540.html#ConnectionErrorHandler)) of type [PROTOCOL_ERROR](http://httpwg.org/specs/rfc7540.html#PROTOCOL_ERROR).

客户端不能推送。因此，如果服务端收到了 [PUSH\_PROMISE](http://httpwg.org/specs/rfc7540.html#PUSH_PROMISE) 帧，必须将其当做类型为 [PROTOCOL\_ERROR](http://httpwg.org/specs/rfc7540.html#PROTOCOL_ERROR) 的连接错误( [5.4.1节](http://httpwg.org/specs/rfc7540.html#ConnectionErrorHandler) )。客户端必须拒绝任何将 [SETTING\_ENABLE\_PUSH](http://httpwg.org/specs/rfc7540.html#SETTINGS_ENABLE_PUSH) 改为非0值的行为，否则就将其当做类型为 [PROTOCOL\_ERROR](http://httpwg.org/specs/rfc7540.html#PROTOCOL_ERROR) 的连接错误( [5.4.1节](http://httpwg.org/specs/rfc7540.html#ConnectionErrorHandler) )。