# 2.1 Document Organization / 文档结构
> The HTTP/2 specification is split into four parts:
> 
> * Starting HTTP/2 ([Section 3](https://httpwg.github.io/specs/rfc7540.html#starting)) covers how an HTTP/2 connection is initiated.
> * The frame ([Section 4](https://httpwg.github.io/specs/rfc7540.html#FramingLayer))and stream ([Section 5](https://httpwg.github.io/specs/rfc7540.html#StreamsLayer))layers describe the way HTTP/2 frames are structured and formed into multiplexed streams.
> * Frame ([Section 6](https://httpwg.github.io/specs/rfc7540.html#FrameTypes))and error ([Section 7](https://httpwg.github.io/specs/rfc7540.html#ErrorCodes)) definitions include details of the frame and error types used in HTTP/2.
> * HTTP mappings ([Section 8](https://httpwg.github.io/specs/rfc7540.html#HTTPLayer))and additional requirements ([Section 9](https://httpwg.github.io/specs/rfc7540.html#HttpExtra))describe how HTTP semantics are expressed using frames and streams.

HTTP/2规范分为四个部分：

* 开始HTTP/2一节([第3章](https://httpwg.github.io/specs/rfc7540.html#starting))涵盖了怎样初始化一个HTTP/2连接。
* 帧层([第4章](https://httpwg.github.io/specs/rfc7540.html#FramingLayer))和流层([第5章](https://httpwg.github.io/specs/rfc7540.html#StreamsLayer))两章描述了HTTP/2帧怎样被构造并形成多路复用的流。
* 帧定义([第6章](https://httpwg.github.io/specs/rfc7540.html#FrameTypes))和错误定义([第7章](https://httpwg.github.io/specs/rfc7540.html#ErrorCodes))包含了HTTP/2的帧和错误类型的细节。
* HTTP映射([第8章](https://httpwg.github.io/specs/rfc7540.html#HTTPLayer))与附加要求([第9章](https://httpwg.github.io/specs/rfc7540.html#HttpExtra))描述了HTTP语义是怎样用帧和流来表达的。

> While some of the frame and stream layer concepts are isolated from HTTP, this specification does not define a completely generic frame layer. The frame and stream layers are tailored to the needs of the HTTP protocol and server push.

尽管一些帧和流层的概念与HTTP是相脱离的，但本规范没有定义一个完全通用的帧层。这些帧和流层是根据HTTP协议和服务端推送的需求量身定制的。