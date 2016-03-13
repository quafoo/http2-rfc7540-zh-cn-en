# Response Pseudo-Header Fields / 响应的伪首部字段
> For HTTP/2 responses, a single :status pseudo-header field is defined that carries the HTTP status code field (see [*[RFC7231]*](http://httpwg.org/specs/rfc7540.html#RFC7231), [Section 6](http://httpwg.org/specs/rfc7231.html#status.codes)). This pseudo-header field MUST be included in all responses; otherwise, the response is malformed ([Section 8.1.2.6](http://httpwg.org/specs/rfc7540.html#malformed)).

对于HTTP/2响应，只定义了一个`:status`伪首部字段，其携带了HTTP状态码(参见 [*[RFC7231]*](http://httpwg.org/specs/rfc7540.html#RFC7231)，[第6章](http://httpwg.org/specs/rfc7231.html#status.codes) )。所有的响应都必须包含该伪首部字段，否则，响应就是有缺陷的( [8.1.2.6节](http://httpwg.org/specs/rfc7540.html#malformed) )。


> HTTP/2 does not define a way to carry the version or reason phrase that is included in an HTTP/1.1 status line.

HTTP/2没有定义携带HTTP/1.1状态行里包含的版本或者原因短语的方法。