# Upgrading from HTTP/2 / 从HTTP/2升级
> HTTP/2 removes support for the 101 (Switching Protocols) informational status code ([*[RFC7231]*](http://httpwg.org/specs/rfc7540.html#RFC7231), [Section 6.2.2](http://httpwg.org/specs/rfc7231.html#status.101)).

HTTP/2移除了对101(Switching Protocols)信息性状态码( [*[RFC7231]*](http://httpwg.org/specs/rfc7540.html#RFC7231)，[6.2.2节](http://httpwg.org/specs/rfc7231.html#status.101) )的支持。


> The semantics of 101 (Switching Protocols) aren't applicable to a multiplexed protocol. Alternative protocols are able to use the same mechanisms that HTTP/2 uses to negotiate their use (see [Section 3](http://httpwg.org/specs/rfc7540.html#starting)).

101(Switching Protocols)的语义不适用于多路复用的协议。替代协议可以使用跟HTTP/2相同的协商机制(参见 [第3章](http://httpwg.org/specs/rfc7540.html#starting))。