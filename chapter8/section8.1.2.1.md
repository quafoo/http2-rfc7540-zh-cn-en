# Pseudo-Header Fields / 伪首部字段
> While HTTP/1.x used the message start-line (see [*[RFC7230]*](http://httpwg.org/specs/rfc7540.html#RFC7230), [Section 3.1](http://httpwg.org/specs/rfc7230.html#start.line)) to convey the target URI, the method of the request, and the status code for the response, HTTP/2 uses special pseudo-header fields beginning with ':' character (ASCII 0x3a) for this purpose.

HTTP/1.x使用消息起始行( [*[RFC7230]*](http://httpwg.org/specs/rfc7540.html#RFC7230)，[3.1节](http://httpwg.org/specs/rfc7230.html#start.line) )表达目标URI。对于同样的目的，HTTP/2使用以':'字符(ASCII 0x3a)开始的特殊的伪首部字段来表示请求的方法和响应的状态码。


> Pseudo-header fields are not HTTP header fields. Endpoints MUST NOT generate pseudo-header fields other than those defined in this document.

伪首部字段不是HTTP首部字段。端点不能生成本文档定义之外的伪首部字段。


> Pseudo-header fields are only valid in the context in which they are defined. Pseudo-header fields defined for requests MUST NOT appear in responses; pseudo-header fields defined for responses MUST NOT appear in requests. Pseudo-header fields MUST NOT appear in trailers. Endpoints MUST treat a request or response that contains undefined or invalid pseudo-header fields as malformed ([Section 8.1.2.6](http://httpwg.org/specs/rfc7540.html#malformed)).

伪首部字段只在它们被定义的上下文中才有效。为请求定义的伪首部字段不能出现在响应中；为响应定义的伪首部字段不能出现在请求中。伪首部字段不能出现在拖尾中。如果请求或响应包含未定义的或无效的伪首部字段，端点必须将该请求或响应看做是有缺陷的( [8.1.2.6节](http://httpwg.org/specs/rfc7540.html#malformed) )。


> All pseudo-header fields MUST appear in the header block before regular header fields. Any request or response that contains a pseudo-header field that appears in a header block after a regular header field MUST be treated as malformed ([Section 8.1.2.6](http://httpwg.org/specs/rfc7540.html#malformed)).

在首部块中，所有的伪首部字段都必须出现在正常的首部字段之前。如果请求或响应包含的伪首部字段在首部块中位于正常的首部字段之后，必须将该请求或响应看作是有缺陷的( [8.1.2.6节](http://httpwg.org/specs/rfc7540.html#malformed) )。