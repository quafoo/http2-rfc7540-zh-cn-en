# 2. HTTP/2 Protocol Overview / HTTP/2概述
> HTTP/2 provides an optimized transport for HTTP semantics. HTTP/2 supports all of the core features of HTTP/1.1 but aims to be more efficient in several ways.

针对HTTP语义，HTTP/2提供了一种优化的传输机制。HTTP/2支持HTTP/1.1所有的核心特性，但在一下几个方面更有效率。

> The basic protocol unit in HTTP/2 is a frame ([Section 4.1](https://httpwg.github.io/specs/rfc7540.html#FrameHeader)). Each frame type serves a different purpose. For example, [HEADERS](https://httpwg.github.io/specs/rfc7540.html#HEADERS) and [DATA](https://httpwg.github.io/specs/rfc7540.html#DATA) frames form the basis of HTTP requests and responses ([Section 8.1](https://httpwg.github.io/specs/rfc7540.html#HttpSequence)); other frame types like [SETTINGS](https://httpwg.github.io/specs/rfc7540.html#SETTINGS), [WINDOW_UPDATE](https://httpwg.github.io/specs/rfc7540.html#WINDOW_UPDATE), and [PUSH_PROMISE](https://httpwg.github.io/specs/rfc7540.html#PUSH_PROMISE) are used in support of other HTTP/2 features.

帧([4.1节](https://httpwg.github.io/specs/rfc7540.html#FrameHeader))是HTTP/2里基本的协议单元。每个类型的帧都服务于不同的目的。例如：[HEADERS](https://httpwg.github.io/specs/rfc7540.html#HEADERS)帧和[DATA](https://httpwg.github.io/specs/rfc7540.html#DATA)帧形成了HTTP请求和响应([8.1节](https://httpwg.github.io/specs/rfc7540.html#HttpSequence))的主体；其他的诸如[SETTINGS](https://httpwg.github.io/specs/rfc7540.html#SETTINGS)、[WINDOW_UPDATE](https://httpwg.github.io/specs/rfc7540.html#WINDOW_UPDATE)和[PUSH_PROMISE](https://httpwg.github.io/specs/rfc7540.html#PUSH_PROMISE)等帧类型，用来支持HTTP/2的其他特性。

> Multiplexing of requests is achieved by having each HTTP request/response exchange associated with its own stream ([Section 5](https://httpwg.github.io/specs/rfc7540.html#StreamsLayer)). Streams are largely independent of each other, so a blocked or stalled request or response does not prevent progress on other streams.

每对HTTP请求/响应的交换都和自己的流([第5章](https://httpwg.github.io/specs/rfc7540.html#StreamsLayer))相关联，从而实现了请求的多路复用。流之间是相互独立的，所以阻塞或停止请求或响应不会中断其他的流。

> ==Flow control== and ==prioritization== ensure that it is possible to efficiently use multiplexed streams. Flow control ([Section 5.2](https://httpwg.github.io/specs/rfc7540.html#FlowControl)) helps to ensure that only data that can be used by a ==receiver== is transmitted. Prioritization ([Section 5.3](https://httpwg.github.io/specs/rfc7540.html#StreamPriority)) ensures that limited resources can be directed to the most important streams first.

流量控制和优先级机制确保能够高效地利用多路复用的流。流量控制([5.2节](https://httpwg.github.io/specs/rfc7540.html#FlowControl))有助于确保只传输接收方能够使用的数据。优先级机制([5.3节](https://httpwg.github.io/specs/rfc7540.html#StreamPriority))确保有限的资源首先被用于最重要的流。

> HTTP/2 adds a new interaction mode whereby a server can push responses to a client ([Section 8.2](https://httpwg.github.io/specs/rfc7540.html#PushResources)). Server push allows a server to speculatively send data to a client that the server anticipates the client will need, trading off some network usage against a potential latency gain. ==The server does this by synthesizing a request, which it sends as a PUSH_PROMISE frame. The server is then able to send a response to the synthetic request on a separate stream.==

HTTP/2增加了一种新的交互模式，即服务器能向客户端推送响应([8.2节](https://httpwg.github.io/specs/rfc7540.html#PushResources))。服务器推送功能允许服务器自主地向客户端发送它预期客户端会需要的数据，从而消耗一些网络带宽来取代潜在的网络延迟。服务器先综合一个PUSH\_PROMISE帧携带的请求，然后在一个独立的流上发送响应。

> Because HTTP header fields used in a connection can contain large amounts of redundant data, frames that contain them are compressed ([Section 4.3](https://httpwg.github.io/specs/rfc7540.html#HeaderBlock)). This has especially advantageous impact upon request sizes in the common case, allowing many requests to be compressed into one packet.

因为连接中使用的HTTP首部字段包含了大量的冗余数据，所以包含这些首部的帧都被压缩了([4.3节](https://httpwg.github.io/specs/rfc7540.html#HeaderBlock))。一般情况下，请求能显著地被压缩，从而允许多个请求被压缩到一个包里。