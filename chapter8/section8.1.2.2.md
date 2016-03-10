# Connection-Specific Header Fields / 连接专用的首部字段
> HTTP/2 does not use the Connection header field to indicate connection-specific header fields; in this protocol, connection-specific metadata is conveyed by other means. An endpoint MUST NOT generate an HTTP/2 message containing connection-specific header fields; any message containing connection-specific header fields MUST be treated as malformed ([Section 8.1.2.6](http://httpwg.org/specs/rfc7540.html#malformed)).

HTTP/2不使用Connection首部字段来表示连接专用的首部字段；在本协议中，连接专用的元数据通过其他方式来表达。端点不能生成一个包含连接专用首部字段的HTTP/2消息；必须将任何包含连接专用首部字段的消息看做是有缺陷的( [8.1.2.6节](http://httpwg.org/specs/rfc7540.html#malformed) )。

> The only exception to this is the TE header field, which MAY be present in an HTTP/2 request; when it is, it MUST NOT contain any value other than "trailers".

唯一的例外是TE首部字段，它可以出现在HTTP/2请求里，但它的值只能是"trailers"。

> This means that an intermediary transforming an HTTP/1.x message to HTTP/2 will need to remove any header fields nominated by the Connection header field, along with the Connection header field itself. Such intermediaries SHOULD also remove other connection-specific header fields, such as Keep-Alive, Proxy-Connection, Transfer-Encoding, and Upgrade, even if they are not nominated by the Connection header field.

这意味着中介将一个HTTP/1.x消息转换成HTTP/2消息时，需要将所有的连接首部字段随同Connection首部字段一起移除。这样的中介也应该移除其他的连接专用首部字段，例如`Keep-Alive`、`Proxy-Connection`、`Transfer-Encoding`和`Upgrade`，即使这些首部字段不是以Connection命名的字段。


> Note: HTTP/2 purposefully does not support upgrade to another protocol. The handshake methods described in [Section 3](http://httpwg.org/specs/rfc7540.html#starting) are believed sufficient to negotiate the use of alternative protocols.

注意：HTTP/2有意地不支持通过upgrade首部字段转换到其它协议。[第3章](http://httpwg.org/specs/rfc7540.html#starting) 描述的握手方法协商使用其它替代协议时足够用了。