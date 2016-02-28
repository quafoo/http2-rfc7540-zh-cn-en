# WINDOW\_UPDATE / WINDOW_UPDATE帧
> The WINDOW_UPDATE frame (type=0x8) is used to implement flow control; see [Section 5.2](http://httpwg.org/specs/rfc7540.html#FlowControl) for an overview.

WINDOW_UPDATE帧(type=0x8)用于执行流量控制功能；参见 [5.2节](http://httpwg.org/specs/rfc7540.html#FlowControl) 的概述。


> Flow control operates at two levels: on each individual stream and on the entire connection.

流量控制操作有两个层次：在每一个单独的流上和在整个连接上。


> Both types of flow control are hop by hop, that is, only between the two endpoints. Intermediaries do not forward WINDOW_UPDATE frames between dependent connections. However, throttling of data transfer by any receiver can indirectly cause the propagation of flow-control information toward the original sender.

这两种流量控制都是逐跳的，即，只在两个端点之间。中介不会两个独立的连接之间转发WINDOW_UPDATE帧。但是，任何接收端对数据传输的遏制都会间接地导致流量控制信息向源发送端传播。


> Flow control only applies to frames that are identified as being subject to flow control. Of the frame types defined in this document, this includes only [DATA](http://httpwg.org/specs/rfc7540.html#DATA) frames. Frames that are exempt from flow control MUST be accepted and processed, unless the receiver is unable to assign resources to handling the frame. A receiver MAY respond with a stream error ([Section 5.4.2](http://httpwg.org/specs/rfc7540.html#StreamErrorHandler)) or connection error ([Section 5.4.1](http://httpwg.org/specs/rfc7540.html#ConnectionErrorHandler)) of type [FLOW\_CONTROL\_ERROR](http://httpwg.org/specs/rfc7540.html#FLOW_CONTROL_ERROR) if it is unable to accept a frame.
> 
> ```
> +-+-------------------------------------------------------------+
> |R|              Window Size Increment (31)                     |
> +-+-------------------------------------------------------------+
> 				Figure 14: WINDOW_UPDATE Payload Format
> ```

流量控制功能只适用于被标识的、受流量控制影响的帧。本文档定义的帧类型里，只有 [DATA](http://httpwg.org/specs/rfc7540.html#DATA)帧 受流量控制影响。除非接收端不能再分配资源去处理这些帧，否则不受流量控制影响的帧必须被接收并处理。如果接收端不能再接收帧了，它可以响应一个 [FLOW\_CONTROL\_ERROR](http://httpwg.org/specs/rfc7540.html#FLOW_CONTROL_ERROR) 类型的 流错误( [5.4.2节](http://httpwg.org/specs/rfc7540.html#StreamErrorHandler) ) 或者 连接错误( [5.4.1节](http://httpwg.org/specs/rfc7540.html#ConnectionErrorHandler) )。

```
+-+-------------------------------------------------------------+
|R|              Window Size Increment (31)                     |
+-+-------------------------------------------------------------+
				  图 14: WINDOW_UPDATE帧负载格式
```


> The payload of a WINDOW_UPDATE frame is one reserved bit plus an unsigned 31-bit integer indicating the number of octets that the sender can transmit in addition to the existing flow-control window. The legal range for the increment to the flow-control window is 1 to 2^31 - 1 (2,147,483,647) octets.

WINDOW_UPDTAE帧的负载由保留的1bit位，加上一个31bit的无符号整数组成，该整数表示除了现有的流量控制窗口之外，发送端还可以传送的字节数。流量控制窗口增长的合法范围是1到2^31 - 1(2,147,483,647)字节。


> The WINDOW_UPDATE frame does not define any flags.

WINDOW_UPDATE帧没有定义任何标志。


> The WINDOW_UPDATE frame can be specific to a stream or to the entire connection. In the former case, the frame's stream identifier indicates the affected stream; in the latter, the value "0" indicates that the entire connection is the subject of the frame.

WINDOW\_UPDATE帧可以是针对一个流的，或者是针对整个连接的。如果是前者，WINDOW\_UPDATE帧的流标识符指明了受影响的流；如果是后者，值为0表示整个连接都受WINDOW\_UPDATE帧的影响。


> A receiver MUST treat the receipt of a WINDOW_UPDATE frame with an flow-control window increment of 0 as a stream error ([Section 5.4.2](http://httpwg.org/specs/rfc7540.html#StreamErrorHandler)) of type [PROTOCOL\_ERROR](http://httpwg.org/specs/rfc7540.html#PROTOCOL_ERROR); errors on the connection flow-control window MUST be treated as a connection error ([Section 5.4.1](http://httpwg.org/specs/rfc7540.html#ConnectionErrorHandler)).

如果收到了 流量控制窗口增量(flow-control window increment) 为0的WINDOW\_UPDATE帧，接收端必须将其当做类型为 [PROTOCOL_ERROR](http://httpwg.org/specs/rfc7540.html#PROTOCOL_ERROR) 的流错误( [5.4.2节](http://httpwg.org/specs/rfc7540.html#StreamErrorHandler) )；发生在连接上的流量控制窗口错误必须被当做连接错误( [5.4.1节](http://httpwg.org/specs/rfc7540.html#ConnectionErrorHandler) )。


> WINDOW\_UPDATE can be sent by a peer that has sent a frame bearing the END\_STREAM flag. This means that a receiver could receive a WINDOW_UPDATE frame on a "half-closed (remote)" or "closed" stream. A receiver MUST NOT treat this as an error (see Section 5.1).

WINDOW\_UPDATE可以由发送过携带有END\_STREAM标志的帧的对端发送。这意味着接收端可能会在 半关闭(远端)(half-closed (remote)) 或者 关闭(closed) 状态的流上收到WINDOW_UPDATE帧。接收端不能将其当做错误(参加 [5.1节](http://httpwg.org/specs/rfc7540.html#StreamStates) )。


> A receiver that receives a flow-controlled frame MUST always account for its contribution against the connection flow-control window, unless the receiver treats this as a connection error ([Section 5.4.1](http://httpwg.org/specs/rfc7540.html#ConnectionErrorHandler)). This is necessary even if the frame is in error. The sender counts the frame toward the flow-control window, but if the receiver does not, the flow-control window at the sender and receiver can become different.

除非将其当做连接错误( [5.4.1节](http://httpwg.org/specs/rfc7540.html#ConnectionErrorHandler) )，否则当接收端收到被流量控制影响的帧时，必须总是将其从流量控制窗口中减掉。即使帧有错误，这也是有必要的。发送端将该帧计入流量控制窗口，但是如果接收端没有这样做，发送端和接收端的流量控制窗口就会变得不同。


> A WINDOW\_UPDATE frame with a length other than 4 octets MUST be treated as a connection error ([Section 5.4.1](http://httpwg.org/specs/rfc7540.html#ConnectionErrorHandler)) of type [FRAME\_SIZE_ERROR](http://httpwg.org/specs/rfc7540.html#FRAME_SIZE_ERROR).

如果WINDOW\_UPDATE帧的长度不是4字节，必须将其当做类型为 [FRAME\_SIZE\_ERROR](http://httpwg.org/specs/rfc7540.html#FRAME_SIZE_ERROR) 的连接错误( [5.4.1节](http://httpwg.org/specs/rfc7540.html#ConnectionErrorHandler) )。