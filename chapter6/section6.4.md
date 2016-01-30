# RST\_STREAM / RST_STREAM帧
> The RST\_STREAM frame (type=0x3) allows for immediate termination of a stream. RST\_STREAM is sent to request cancellation of a stream or to indicate that an error condition has occurred.
> 
> ```
> +---------------------------------------------------------------+
> |                        Error Code (32)                        |
> +---------------------------------------------------------------+
> 				Figure 9: RST_STREAM Frame Payload
> ```

RST\_STREAM帧(type=0x3)可以立即终结一个流。RST\_STREAM用来请求取消一个流，或者表示发生了一个错误。

> ```
> +---------------------------------------------------------------+
> |                        Error Code (32)                        |
> +---------------------------------------------------------------+
> 					  图 9：RST_STREAM帧负载
> ```

> The RST_STREAM frame contains a single unsigned, 32-bit integer identifying the error code ( [Section 7](https://httpwg.github.io/specs/rfc7540.html#ErrorCodes) ). The error code indicates why the stream is being terminated.

RST\_STREAM帧包含一个32bit的无符号整数表示的错误码( [第7章](https://httpwg.github.io/specs/rfc7540.html#ErrorCodes) )。错误码指明了流为什么被终结。


> The RST_STREAM frame does not define any flags.

RST_STREAM帧没有定义任何特征位标记。


> The RST\_STREAM frame fully terminates the referenced stream and causes it to enter the "closed" state. After receiving a RST\_STREAM on a stream, the receiver MUST NOT send additional frames for that stream, with the exception of [PRIORITY](https://httpwg.github.io/specs/rfc7540.html#PRIORITY). However, after sending the RST\_STREAM, the sending endpoint MUST be prepared to receive and process additional frames sent on the stream that might have been sent by the peer prior to the arrival of the RST_STREAM.

RST\_STREAM帧完全终结了其所在的流，并且促使该流进入 关闭(closed) 状态。当在某一个流上收到一个RST\_STREAM帧以后，除了 [PRIORITY](https://httpwg.github.io/specs/rfc7540.html#PRIORITY) 帧，接收端不能在该流上发送任何其他的帧。但是，在发送完RST\_STREAM帧以后，发送端必须准备好接收并处理该流上的其他帧，这些帧可能是在RST\_STREAM帧到达前由对端所发送。


> RST\_STREAM frames MUST be associated with a stream. If a RST\_STREAM frame is received with a stream identifier of 0x0, the recipient MUST treat this as a connection error ([Section 5.4.1](https://httpwg.github.io/specs/rfc7540.html#ConnectionErrorHandler) of type [PROTOCOL_ERROR](https://httpwg.github.io/specs/rfc7540.html#PROTOCOL_ERROR).

RST\_STREAM帧必须和一个流相关联。如果收到一个流标识符为0x0的RST\_STREAM帧，接收端必须将其处理为类型为 [PROTOCOL_ERROR](https://httpwg.github.io/specs/rfc7540.html#PROTOCOL_ERROR) 的连接错误( [5.4.1节](https://httpwg.github.io/specs/rfc7540.html#ConnectionErrorHandler) )。


> RST\_STREAM frames MUST NOT be sent for a stream in the "idle" state. If a RST\_STREAM frame identifying an idle stream is received, the recipient MUST treat this as a connection error ([Section 5.4.1](https://httpwg.github.io/specs/rfc7540.html#ConnectionErrorHandler)) of type [PROTOCOL_ERROR](https://httpwg.github.io/specs/rfc7540.html#PROTOCOL_ERROR).

不能在处于 空闲(idle) 状态的流上发送RST\_STREAM帧。如果收到一个空闲流上的RST\_STREAM帧，接收方必须将其处理为类型为 [PROTOCOL_ERROR](https://httpwg.github.io/specs/rfc7540.html#PROTOCOL_ERROR) 的连接错误( [5.4.1节](https://httpwg.github.io/specs/rfc7540.html#ConnectionErrorHandler) )。


> A RST\_STREAM frame with a length other than 4 octets MUST be treated as a connection error ([Section 5.4.1](https://httpwg.github.io/specs/rfc7540.html#ConnectionErrorHandler)) of type [FRAME\_SIZE_ERROR](https://httpwg.github.io/specs/rfc7540.html#FRAME_SIZE_ERROR).

必须将不是4字节长度的RST\_STREAM帧当做一个类型为 [FRAME\_SIZE\_ERROR](https://httpwg.github.io/specs/rfc7540.html#FRAME_SIZE_ERROR) 的连接错误 [5.4.1节](https://httpwg.github.io/specs/rfc7540.html#ConnectionErrorHandler)。