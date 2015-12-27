# HTTP2-Settings Header Field / HTTP2-Settings首部字段
> A request that upgrades from HTTP/1.1 to HTTP/2 MUST include exactly one HTTP2-Settings header field. The HTTP2-Settings header field is a connection-specific header field that includes parameters that govern the HTTP/2 connection, provided in anticipation of the server accepting the request to upgrade.
> 
> ```
> HTTP2-Settings    = token68
> ```

从HTTP/1.1升级到HTTP/2的请求必须确切地包含一个HTTP2-Settings首部字段。HTTP2-Settings首部字段是一个连接专用的首部字段，它包含管理HTTP/2连接的参数，前提是假设服务端会接受升级请求。

```
HTTP2-Settings    = token68
```

> A server MUST NOT upgrade the connection to HTTP/2 if this header field is not present or if more than one is present. A server MUST NOT send this header field.

如果该首部字段没有出现，或者出现了不止一个，那么服务端一定不要把连接升级到HTTP/2。服务端一定不要发送该首部字段。

> The content of the HTTP2-Settings header field is the payload of a SETTINGS frame (Section 6.5), encoded as a base64url string (that is, the URL- and filename-safe Base64 encoding described in Section 5 of [RFC4648], with any trailing '=' characters omitted). The ABNF [RFC5234] production for token68 is defined in Section 2.1 of [RFC7235].

HTTP2-Settings首部字段的值是SETTINGS帧(6.5节)的有效载荷被编码成的base64url串(即，*[RFC4648]*第5节描述的URL和文件名安全Base64编码，忽略任何'='字符)。ABNF*[RFC5234]*产生token68在*[RFC7235]*2.1节有定义。

> Since the upgrade is only intended to apply to the immediate connection, a client sending the HTTP2-Settings header field MUST also send HTTP2-Settings as a connection option in the Connection header field to prevent it from being forwarded (see Section 6.1 of [RFC7230]).

因为升级操作只适用于相邻端点的直连，发送HTTP2-Settings首部字段的客户端也必须在发送的Connection首部字段值里加上HTTP2-Settings，以阻止它被转发(参见*[RFC7230]*的6.1节)。

> A server decodes and interprets these values as it would any other SETTINGS frame. Explicit acknowledgement of these settings (Section 6.5.3) is not necessary, since a 101 response serves as implicit acknowledgement. Providing these values in the upgrade request gives a client an opportunity to provide parameters prior to receiving any frames from the server.

就像对其他的SETTINGS帧那样，服务端对这些值进行解码和解释。没有必要对这些设置(6.5.3节)进行显示的确认，因为101响应就相当于隐式的确认。在收到服务端发送的帧之前，客户端有机会在升级请求的这些值里提供一些参数。