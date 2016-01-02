# HTTP/2 Version Identification / HTTP/2版本标识
> The protocol defined in this document has two identifiers.
> 
> * The string "h2" identifies the protocol where HTTP/2 uses [Transport Layer Security (TLS)](https://httpwg.github.io/specs/rfc7540.html#TLS12) *[TLS12]*. This identifier is used in the [TLS application-layer protocol negotiation (ALPN) extension](https://httpwg.github.io/specs/rfc7540.html#TLS-ALPN) *[TLS-ALPN]* field and in any place where HTTP/2 over TLS is identified.
> 
> 	The "h2" string is serialized into an ALPN protocol identifier as the two-octet sequence: 0x68, 0x32.
> 
> * The string "h2c" identifies the protocol where HTTP/2 is run over cleartext TCP. This identifier is used in the HTTP/1.1 Upgrade header field and in any place where HTTP/2 over TCP is identified.
> 
>  The "h2c" string is reserved from the ALPN identifier space but describes a protocol that does not use TLS.

本文档定义的协议有两个标识符：

* 字符串"h2"表示HTTP/2协议使用了 [安全传输层协议(TLS)](https://httpwg.github.io/specs/rfc7540.html#TLS12) *[TLS12]*。该标识符用在 [TLS应用层协议协商(ALPN)扩展](https://httpwg.github.io/specs/rfc7540.html#TLS-ALPN) *[TLS-ALPN]*字段，以及任何在TLS之上运行HTTP/2的场合。
	
  "h2"字符串被序列化成一个ALPN协议标识符，其形式是两个字节的序列：0x68，0x32。

* 字符串"h2c"表示HTTP/2协议运行在明文TCP上。该标识符用在HTTP/1.1的Upgrade首部字段，以及任何在TCP之上运行HTTP/2的场合。

  "h2c"字符串是ALPN标识符空间预留的，但是用来表示不使用TLS的协议。

> Negotiating "h2" or "h2c" implies the use of the transport, security, framing, and message semantics described in this document.

"h2"或"h2c"协商需要用到本文档里描述的传输层、安全、帧和消息语义等概念。