# Header Compression and Decompression / 首部压缩与解压缩
> Just as in HTTP/1, a header field in HTTP/2 is a name with one or more associated values. Header fields are used within HTTP request and response messages as well as in server push operations (see [Section 8.2](https://httpwg.github.io/specs/rfc7540.html#PushResources)).

正如在HTTP/1里那样，HTTP/2里的首部字段也是一个键具有一个或多个值。这些首部字段用于HTTP请求和响应消息，也用于服务端推送操作( [8.2节](https://httpwg.github.io/specs/rfc7540.html#PushResources) )。

> Header lists are collections of zero or more header fields. When transmitted over a connection, a header list is serialized into a header block using [HTTP header compression](https://httpwg.github.io/specs/rfc7540.html#COMPRESSION) [COMPRESSION]. The serialized header block is then divided into one or more octet sequences, called header block fragments, and transmitted within the payload of HEADERS ([Section 6.2](https://httpwg.github.io/specs/rfc7540.html#HEADERS)), PUSH_PROMISE ([Section 6.6](https://httpwg.github.io/specs/rfc7540.html#PUSH_PROMISE)), or CONTINUATION ([Section 6.10](https://httpwg.github.io/specs/rfc7540.html#CONTINUATION)) frames.

首部列表是零个或多个首部字段的集合。当通过连接传送时，首部列表被 [HTTP首部压缩](https://httpwg.github.io/specs/rfc7540.html#COMPRESSION) *[COMPRESSION]*序列化成首部块。然后，序列化的首部块又被划分成一个或多个叫做首部块片段的字节序列，并通过HEADERS( [6.2节](https://httpwg.github.io/specs/rfc7540.html#HEADERS) )、PUSH\_PROMISE( [6.6节](https://httpwg.github.io/specs/rfc7540.html#PUSH_PROMISE) )，或者CONTINUATION( [6.10节](https://httpwg.github.io/specs/rfc7540.html#CONTINUATION) )帧的有效负载传送。

> The [Cookie header field](https://httpwg.github.io/specs/rfc7540.html#COOKIE) *[COOKIE]* is treated specially by the HTTP mapping (see [Section 8.1.2.5](https://httpwg.github.io/specs/rfc7540.html#CompressCookie)).

[Cookie首部字段](https://httpwg.github.io/specs/rfc7540.html#COOKIE) *[COOKIE]* 需要HTTP映射特殊对待(参见 [8.1.2.5节](https://httpwg.github.io/specs/rfc7540.html#CompressCookie) )。

> A receiving endpoint reassembles the header block by concatenating its fragments and then decompresses the block to reconstruct the header list.

接收端连接片段重组首部块，然后解压首部块重建首部列表。

> A complete header block consists of either:
> 
> * a single [HEADERS](https://httpwg.github.io/specs/rfc7540.html#HEADERS) or [PUSH\_PROMISE](https://httpwg.github.io/specs/rfc7540.html#PUSH_PROMISE) frame, with the END\_HEADERS flag set, or
> * a [HEADERS](https://httpwg.github.io/specs/rfc7540.html#HEADERS) or [PUSH\_PROMISE](https://httpwg.github.io/specs/rfc7540.html#PUSH_PROMISE) frame with the END\_HEADERS flag cleared and one or more [CONTINUATION](https://httpwg.github.io/specs/rfc7540.html#CONTINUATION) frames, where the last [CONTINUATION](https://httpwg.github.io/specs/rfc7540.html#CONTINUATION) frame has the END\_HEADERS flag set.

一个完整的首部块包括以下二者之一：

* 一个单独的、设置了END\_HEADERS标识的 [HEADERS](https://httpwg.github.io/specs/rfc7540.html#HEADERS) 或 [PUSH\_PROMISE](https://httpwg.github.io/specs/rfc7540.html#PUSH_PROMISE) 帧，或者
* 一个单独的、清除了END\_HEADERS标识的 [HEADERS](https://httpwg.github.io/specs/rfc7540.html#HEADERS) 或 [PUSH\_PROMISE](https://httpwg.github.io/specs/rfc7540.html#PUSH_PROMISE) 帧，并有一个或多个 [CONTINUATION](https://httpwg.github.io/specs/rfc7540.html#CONTINUATION) 帧，且最后一个 [CONTINUATION](https://httpwg.github.io/specs/rfc7540.html#CONTINUATION) 帧设置了END\_HEADERS标识。

> Header compression is stateful. One compression context and one decompression context are used for the entire connection. A decoding error in a header block MUST be treated as a connection error ([Section 5.4.1](https://httpwg.github.io/specs/rfc7540.html#ConnectionErrorHandler)) of type [COMPRESSION\_ERROR](https://httpwg.github.io/specs/rfc7540.html#COMPRESSION_ERROR).

首部压缩是有状态的。一个连接具有一个压缩上下文和一个解压缩上下文。必须将首部块解码错误当做类型为 [COMPRESSION\_ERROR](https://httpwg.github.io/specs/rfc7540.html#COMPRESSION_ERROR) 的连接错误( [5.4.1节](https://httpwg.github.io/specs/rfc7540.html#ConnectionErrorHandler) )处理。

> Each header block is processed as a discrete unit. Header blocks MUST be transmitted as a contiguous sequence of frames, with no interleaved frames of any other type or from any other stream. The last frame in a sequence of [HEADERS](https://httpwg.github.io/specs/rfc7540.html#HEADERS) or [CONTINUATION](https://httpwg.github.io/specs/rfc7540.html#CONTINUATION) frames has the END\_HEADERS flag set. The last frame in a sequence of PUSH_PROMISE or CONTINUATION frames has the END\_HEADERS flag set. This allows a header block to be logically equivalent to a single frame.

将每个首部块当做离散的单元处理。必须将首部块作为连续的帧序列传送，而没有交错任何其他类型或其他流的帧。[HEADERS](https://httpwg.github.io/specs/rfc7540.html#HEADERS) 或 [CONTINUATION](https://httpwg.github.io/specs/rfc7540.html#CONTINUATION) 帧序列的最后一个帧设置END\_HEADERS标识。[PUSH_PROMISE](https://httpwg.github.io/specs/rfc7540.html#PUSH_PROMISE) 或 [CONTINUATION](https://httpwg.github.io/specs/rfc7540.html#CONTINUATION) 帧序列的最后一个帧设置END\_HEADERS标识。这让首部块在逻辑上等价于一个单独的帧。

> Header block fragments can only be sent as the payload of [HEADERS](https://httpwg.github.io/specs/rfc7540.html%23HEADERS), [PUSH\_PROMISE](https://httpwg.github.io/specs/rfc7540.html#PUSH_PROMISE), or [CONTINUATION](https://httpwg.github.io/specs/rfc7540.html%23CONTINUATION) frames because these frames carry data that can modify the compression context maintained by a receiver. An endpoint receiving [HEADERS](https://httpwg.github.io/specs/rfc7540.html%23HEADERS), [PUSH\_PROMISE](https://httpwg.github.io/specs/rfc7540.html#PUSH_PROMISE), or [CONTINUATION](https://httpwg.github.io/specs/rfc7540.html%23CONTINUATION) frames needs to reassemble header blocks and perform decompression even if the frames are to be discarded. A receiver MUST terminate the connection with a connection error ([Section 5.4.1](https://httpwg.github.io/specs/rfc7540.html#ConnectionErrorHandler)) of type [COMPRESSION_ERROR](https://httpwg.github.io/specs/rfc7540.html#COMPRESSION_ERROR) if it does not decompress a header block.

首部块片段只能作为 [HEADERS](https://httpwg.github.io/specs/rfc7540.html%23HEADERS) 、[PUSH\_PROMISE](https://httpwg.github.io/specs/rfc7540.html#PUSH_PROMISE) ，或 [CONTINUATION](https://httpwg.github.io/specs/rfc7540.html%23CONTINUATION) 帧的有效载荷进行传送，因为这些帧携带的数据能修改接收端维护的压缩上下文。即使收到要被丢弃的 [HEADERS](https://httpwg.github.io/specs/rfc7540.html%23HEADERS) 、[PUSH\_PROMISE](https://httpwg.github.io/specs/rfc7540.html#PUSH_PROMISE) ，或 [CONTINUATION](https://httpwg.github.io/specs/rfc7540.html%23CONTINUATION) 帧，接收端点也需要重组首部块并解压。如果接收端不解压首部块，它必须用类型为 [COMPRESSION\_ERROR](https://httpwg.github.io/specs/rfc7540.html#COMPRESSION_ERROR) 的连接错误( [5.4.1节](https://httpwg.github.io/specs/rfc7540.html#ConnectionErrorHandler) )终止连接。