# HTTP2-Settings Header Field / HTTP2-Settings首部字段
> A request that upgrades from HTTP/1.1 to HTTP/2 MUST include exactly one HTTP2-Settings header field. The HTTP2-Settings header field is a connection-specific header field that includes parameters that govern the HTTP/2 connection, provided in anticipation of the server accepting the request to upgrade.
> 
> ```
> HTTP2-Settings    = token68
> ```

从HTTP/1.1升级到HTTP/2的请求必须确切地包含一个HTTP2-Settings首部字段。HTTP2-Settings首部字段是一个专用于连接的首部字段，它包含管理HTTP/2连接的参数，其前提是假设服务端会接受升级请求。

```
HTTP2-Settings    = token68
```

> A server MUST NOT upgrade the connection to HTTP/2 if this header field is not present or if more than one is present. A server MUST NOT send this header field.

如果该首部字段没有出现，或者出现了不止一个，那么服务端一定不要把连接升级到HTTP/2。服务端一定不要发送该首部字段。

> The content of the HTTP2-Settings header field is the payload of a [SETTINGS](https://httpwg.github.io/specs/rfc7540.html#SETTINGS) frame ([Section 6.5](https://httpwg.github.io/specs/rfc7540.html#SETTINGS)), encoded as a base64url string (that is, the URL- and filename-safe Base64 encoding described in [Section 5](https://tools.ietf.org/html/rfc4648#section-5) of [*[RFC4648]*](https://httpwg.github.io/specs/rfc7540.html#RFC4648), with any trailing '=' characters omitted). The [ABNF](https://httpwg.github.io/specs/rfc7540.html#RFC5234) *[RFC5234]* production for token68 is defined in [Section 2.1](https://httpwg.github.io/specs/rfc7235.html#challenge.and.response) of [*[RFC7235]*](https://httpwg.github.io/specs/rfc7540.html#RFC7235).

HTTP2-Settings首部字段的值是 [SETTINGS](https://httpwg.github.io/specs/rfc7540.html#SETTINGS) 帧 ([6.5节](https://httpwg.github.io/specs/rfc7540.html#SETTINGS)) 的有效载荷被编码成的base64url串(即，[*[RFC4648]*](https://httpwg.github.io/specs/rfc7540.html#RFC4648) 的 [第5节](https://tools.ietf.org/html/rfc4648#section-5) 描述的URL-和文件名安全Base64编码，忽略任何拖尾'='字符)。[ABNF](https://httpwg.github.io/specs/rfc7540.html#RFC5234) *[RFC5234]* 产生token68在 [*[RFC7235]*](https://httpwg.github.io/specs/rfc7540.html#RFC7235) 的 [2.1节](https://httpwg.github.io/specs/rfc7235.html#challenge.and.response)有定义。

> Since the upgrade is only intended to apply to the immediate connection, a client sending the HTTP2-Settings header field MUST also send HTTP2-Settings as a connection option in the Connection header field to prevent it from being forwarded (see [Section 6.1](https://httpwg.github.io/specs/rfc7230.html#header.connection) of [*[RFC7230]*](https://httpwg.github.io/specs/rfc7540.html#RFC7230)).

因为升级操作只适用于相邻端点的直连，发送HTTP2-Settings首部字段的客户端也必须在发送的Connection首部字段值里加上HTTP2-Settings选项，以阻止它被转发(参见 [*[RFC7230]*](https://httpwg.github.io/specs/rfc7540.html#RFC7230) 的 [6.1节](https://httpwg.github.io/specs/rfc7230.html%23header.connection) )。

> A server decodes and interprets these values as it would any other [SETTINGS](https://httpwg.github.io/specs/rfc7540.html#SETTINGS) frame. Explicit acknowledgement of these settings (Section 6.5.3) is not necessary, since a 101 response serves as implicit acknowledgement. Providing these values in the upgrade request gives a client an opportunity to provide parameters prior to receiving any frames from the server.

就像对其他的 [SETTINGS](https://httpwg.github.io/specs/rfc7540.html#SETTINGS) 帧那样，服务端对这些值进行解码和解释。没有必要对这些设置( [6.5.3节](https://httpwg.github.io/specs/rfc7540.html#SettingsSync) )进行显示的确认，因为101响应就相当于隐式的确认。在收到服务端发送的帧之前，客户端有机会在升级请求的这些值里提供一些参数。