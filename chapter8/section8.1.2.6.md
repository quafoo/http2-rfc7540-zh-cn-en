# Malformed Requests and Responses / 有缺陷的请求和响应
> A malformed request or response is one that is an otherwise valid sequence of HTTP/2 frames but is invalid due to the presence of extraneous frames, prohibited header fields, the absence of mandatory header fields, or the inclusion of uppercase header field names.

请求或响应本来是有效的HTTP/2帧序列，但由于出现了无关的帧，禁止的首部字段，缺少了强制的首部字段，或者包含了大写的首部字段名，有缺陷的请求或响应就变成了无效的HTTP/2帧序列。


> A request or response that includes a payload body can include a content-length header field. A request or response is also malformed if the value of a content-length header field does not equal the sum of the [DATA](http://httpwg.org/specs/rfc7540.html#DATA) frame payload lengths that form the body. A response that is defined to have no payload, as described in [*[RFC7230]*](http://httpwg.org/specs/rfc7540.html#RFC7230), [Section 3.3.2](http://httpwg.org/specs/rfc7230.html#header.content-length), can have a non-zero content-length header field, even though no content is included in [DATA](http://httpwg.org/specs/rfc7540.html#DATA) frames.

包含负载体的请求或响应可以包含一个`content-length`首部字段。如果`content-length`首部字段的值不等于组成负载体的 [DATA](http://httpwg.org/specs/rfc7540.html#DATA) 帧负载的长度之和，请求或响应也是有缺陷的。被定义为没有负载的响应(就像 [*[RFC7230]*](http://httpwg.org/specs/rfc7540.html#RFC7230)，[3.3.2节](http://httpwg.org/specs/rfc7230.html#header.content-length) 里描述的那样)，即使 [DATA](http://httpwg.org/specs/rfc7540.html#DATA) 帧里没有内容，也可以有一个值为非零的`content-length`首部字段。


> Intermediaries that process HTTP requests or responses (i.e., any intermediary not acting as a tunnel) MUST NOT forward a malformed request or response. Malformed requests or responses that are detected MUST be treated as a stream error ([Section 5.4.2](http://httpwg.org/specs/rfc7540.html#StreamErrorHandler)) of type [PROTOCOL_ERROR](http://httpwg.org/specs/rfc7540.html#PROTOCOL_ERROR).

处理HTTP请求或响应的中介(即，任何不充当隧道的中介)不能转发有缺陷的请求或响应。必须把探测到的有缺陷的请求或响应当做类型为 [PROTOCOL\_ERROR](http://httpwg.org/specs/rfc7540.html#PROTOCOL_ERROR) 的流错误( [5.4.2节](http://httpwg.org/specs/rfc7540.html#StreamErrorHandler) )。


> For malformed requests, a server MAY send an HTTP response prior to closing or resetting the stream. Clients MUST NOT accept a malformed response. Note that these requirements are intended to protect against several types of common attacks against HTTP; they are deliberately strict because being permissive can expose implementations to these vulnerabilities.

对于有缺陷的请求，服务端可以在关闭或重置流之前发送一个HTTP响应。客户端不能接受有缺陷的响应。注意，这些要求的目的是为了免受针对HTTP的几种常见的攻击；这些规则有意地严格，是因为可以避免暴露这些脆弱点的实现细节。