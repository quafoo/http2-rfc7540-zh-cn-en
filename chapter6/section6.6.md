# PUSH\_PROMISE / PUSH_PROMISE帧
> The PUSH\_PROMISE frame (type=0x5) is used to notify the peer endpoint in advance of streams the sender intends to initiate. The PUSH\_PROMISE frame includes the unsigned 31-bit identifier of the stream the endpoint plans to create along with a set of headers that provide additional context for the stream. [Section 8.2](http://httpwg.org/specs/rfc7540.html#PushResources) contains a thorough description of the use of PUSH_PROMISE frames.
> 
> ```
> +---------------+
> |Pad Length? (8)|
> +-+-------------+-----------------------------------------------+
> |R|                  Promised Stream ID (31)                    |
> +-+-----------------------------+-------------------------------+
> |                   Header Block Fragment (*)                 ...
> +---------------------------------------------------------------+
> |                           Padding (*)                       ...
> +---------------------------------------------------------------+
> 					Figure 11: PUSH_PROMISE Payload Format
> ```

在发送端准备初始化流之前，要发送PUSH\_PROMISE(type=0x5)帧来通知对端。PUSH\_PROMISE帧包含31位的无符号流标识符，该流标识符和为流提供额外的上下文的首部集一起由端点创建的。[8.2节](http://httpwg.org/specs/rfc7540.html#PushResources) 详细描述了PUSH_PROMISE帧的使用。

``` 
+---------------+
|Pad Length? (8)|
+-+-------------+-----------------------------------------------+
|R|                  Promised Stream ID (31)                    |
+-+-----------------------------+-------------------------------+
|                   Header Block Fragment (*)                 ...
+---------------------------------------------------------------+
|                           Padding (*)                       ...
+---------------------------------------------------------------+
					图 11: PUSH_PROMISE帧负载的格式
```


> The PUSH_PROMISE frame payload has the following fields:
> 
> * **Pad Length:** An 8-bit field containing the length of the frame padding in units of octets. This field is only present if the PADDED flag is set.
> * **R:** A single reserved bit.
> * **Promised Stream ID:** An unsigned 31-bit integer that identifies the stream that is reserved by the PUSH_PROMISE. The promised stream identifier MUST be a valid choice for the next stream sent by the sender (see "new stream identifier" in Section 5.1.1).
> * **Header Block Fragment:** A header block fragment (Section 4.3) containing request header fields.
> * **Padding:**	Padding octets.

PUSH\_PROMISE帧负载具有以下域：

* **Pad Length:** 一个8bit的域，包含帧填充数据的字节长度。只有当设置了PADDED标识时，该域才会出现。
* **R:** 保留的1bit位。
* **Promised Stream ID:** 31bit的无符号整数，标记PUSH\_PROMISE帧保留的流。对于发送端来说，该标识符必须是可用于下一个流的有效值(参见 [5.1.1节](http://httpwg.org/specs/rfc7540.html#StreamIdentifiers) 的创建流标识符)。
* **Header Block Fragment:** 包含请求首部域的首部块片段( [4.3节](http://httpwg.org/specs/rfc7540.html#HeaderBlock) )。
* **Padding:** 填充字节。


> The PUSH\_PROMISE frame defines the following flags:
> 
> * **END_HEADERS (0x4):** When set, bit 2 indicates that this frame contains an entire header block ([Section 4.3](http://httpwg.org/specs/rfc7540.html#HeaderBlock)) and is not followed by any [CONTINUATION](http://httpwg.org/specs/rfc7540.html#CONTINUATION) frames.
> 
> 	A PUSH\_PROMISE frame without the END\_HEADERS flag set MUST be followed by a CONTINUATION frame for the same stream. A receiver MUST treat the receipt of any other type of frame or a frame on a different stream as a connection error (Section 5.4.1) of type PROTOCOL_ERROR.
> 
> * **PADDED (0x8):** When set, bit 3 indicates that the Pad Length field and any padding that it describes are present.

PUSH\_PROMISE帧定义了如下标识符：

* **END_HEADERS (0x4):** 当设置了该标识符，第二个bit位表示这个帧包含整个首部块( [4.3节](http://httpwg.org/specs/rfc7540.html#HeaderBlock) )，并且后面没有跟随任何 [CONTINUATION](http://httpwg.org/specs/rfc7540.html#CONTINUATION) 帧。

  对于同一个流，没有设置END\_HEADERS标识的PUSH\_PROMISE帧后面必须跟随有CONTINUATION帧。如果接收端收到了其他类型的帧，或者是其他流上的帧，接收端必须将其当做类型为 [PROTOCOL\_ERROR](http://httpwg.org/specs/rfc7540.html#PROTOCOL_ERROR) 的流错误( [5.4.1节](http://httpwg.org/specs/rfc7540.html#ConnectionErrorHandler) )。

* **PADDED (0x8):** 当设置了该标识符，第3个bit位表示出现了填充长度(Pad Length)域和它所描述的填充字节。


> PUSH\_PROMISE frames MUST only be sent on a peer-initiated stream that is in either the "open" or "half-closed (remote)" state. The stream identifier of a PUSH\_PROMISE frame indicates the stream it is associated with. If the stream identifier field specifies the value 0x0, a recipient MUST respond with a connection error (Section 5.4.1) of type [PROTOCOL_ERROR](http://httpwg.org/specs/rfc7540.html#PROTOCOL_ERROR).

只能在处于 打开(open) 或者 半关闭(远端)(half-closed (remote)) 状态的对等初始化的流上发送PUSH\_PROMISE帧。PUSH\_PROMISE帧的流标识符指明其所关联的流。如果流标识符域的值是0x0，接收方必须响应一个类型为 [PROTOCOL\_ERROR](http://httpwg.org/specs/rfc7540.html#PROTOCOL_ERROR) 的连接错误( [5.4.1节](http://httpwg.org/specs/rfc7540.html#ConnectionErrorHandler) )。


> Promised streams are not required to be used in the order they are promised. The PUSH_PROMISE only reserves stream identifiers for later use.

不要求被允诺的流以他们被允诺的顺序来被使用。PUSH\_PROMISE帧只保留用于以后的流标识符。


> PUSH\_PROMISE MUST NOT be sent if the SETTINGS\_ENABLE\_PUSH setting of the peer endpoint is set to 0. An endpoint that has set this setting and has received acknowledgement MUST treat the receipt of a PUSH\_PROMISE frame as a connection error ([Section 5.4.1](http://httpwg.org/specs/rfc7540.html#ConnectionErrorHandler)) of type [PROTOCOL_ERROR](http://httpwg.org/specs/rfc7540.html#PROTOCOL_ERROR).

如果对端的SETTINGS\_ENABLE\_PUSH被设置为0，不能发送PUSH\_PROMISE帧。如果一端已经设置了该设置项，并且收到了确认，当它收到了PUSH\_PROMISE帧时，必须将其当做类型为 [PROTOCOL\_ERROR](http://httpwg.org/specs/rfc7540.html#PROTOCOL_ERROR) 的连接错误( [5.4.1节](http://httpwg.org/specs/rfc7540.html#ConnectionErrorHandler) )。


> Recipients of PUSH\_PROMISE frames can choose to reject promised streams by returning a [RST\_STREAM](http://httpwg.org/specs/rfc7540.html#PROTOCOL_ERROR) referencing the promised stream identifier back to the sender of the PUSH_PROMISE.

PUSH\_PROMISE帧的接收方可以通过给PUSH\_PROMISE帧的发送方返回一个 [RST\_STREAM](http://httpwg.org/specs/rfc7540.html#PROTOCOL_ERROR) 帧来选择拒绝被允诺的流，该RST\_STREAM帧包含被允诺的流的标识符。


> A PUSH\_PROMISE frame modifies the connection state in two ways. First, the inclusion of a header block ([Section 4.3](http://httpwg.org/specs/rfc7540.html#HeaderBlock)) potentially modifies the state maintained for header compression. Second, PUSH\_PROMISE also reserves a stream for later use, causing the promised stream to enter the "reserved" state. A sender MUST NOT send a PUSH_PROMISE on a stream unless that stream is either "open" or "half-closed (remote)"; the sender MUST ensure that the promised stream is a valid choice for a new stream identifier ([Section 5.1.1](http://httpwg.org/specs/rfc7540.html#StreamIdentifiers)) (that is, the promised stream MUST be in the "idle" state).

PUSH\_PROMISE帧有两种方式来修改连接状态。首先，包含一个首部块( [4.3节](http://httpwg.org/specs/rfc7540.html#HeaderBlock) )会潜在地修改为首部压缩维护的状态。其次，PUSH\_PROMISE帧也会为以后的使用保留一个流，从而使被允诺的流进入 保留(reserved) 状态。只有当流处于 打开(open) 或者 半关闭(远端)(half-closed (remote)) 状态时，发送方才能在该流上发送PUSH\_PROMISE帧；发送方必须确保被允诺的流对一个新的流标识符([ 5.1.1节](http://httpwg.org/specs/rfc7540.html#StreamIdentifiers) )是有效的(即，被允诺的流必须处于 空闲(idle) 状态)。


> Since PUSH\_PROMISE reserves a stream, ignoring a PUSH\_PROMISE frame causes the stream state to become indeterminate. A receiver MUST treat the receipt of a PUSH\_PROMISE on a stream that is neither "open" nor "half-closed (local)" as a connection error ([Section 5.4.1](http://httpwg.org/specs/rfc7540.html#ConnectionErrorHandler)) of type [PROTOCOL\_ERROR](http://httpwg.org/specs/rfc7540.html#PROTOCOL_ERROR). However, an endpoint that has sent [RST\_STREAM](http://httpwg.org/specs/rfc7540.html#RST_STREAM) on the associated stream MUST handle PUSH_PROMISE frames that might have been created before the [RST\_STREAM](http://httpwg.org/specs/rfc7540.html#RST_STREAM) frame is received and processed.

因为PUSH\_PROMISE帧会保留一个流，所以忽略PUSH\_PROMISE帧会导致该流的状态变成不确定的。如果在既不是 打开(open) 也不是 半关闭(half-closed(local)) 状态的流上收到了PUSH\_PROMISE帧，接收方必须将其当做类型为 [PROTOCOL\_ERROR](http://httpwg.org/specs/rfc7540.html#PROTOCOL_ERROR) 的连接错误( [5.4.1节](http://httpwg.org/specs/rfc7540.html#ConnectionErrorHandler) )。但是，在关联流上发送了 [RST\_STREAM](http://httpwg.org/specs/rfc7540.html#RST_STREAM) 帧的端点必须能处理PUSH\_PROMISE帧，该PUSH\_PROMISE帧可能在收到并处理 [RST\_STREAM](http://httpwg.org/specs/rfc7540.html#RST_STREAM) 帧之前就已经被创建了。


> A receiver MUST treat the receipt of a PUSH\_PROMISE that promises an illegal stream identifier ([Section 5.1.1](http://httpwg.org/specs/rfc7540.html#StreamIdentifiers)) as a connection error ([Section 5.4.1](http://httpwg.org/specs/rfc7540.html#ConnectionErrorHandler)) of type [PROTOCOL\_ERROR](http://httpwg.org/specs/rfc7540.html#PROTOCOL_ERROR). Note that an illegal stream identifier is an identifier for a stream that is not currently in the "idle" state.

如果接收端收到允诺了一个非法的流标识符( [5.1.1节](http://httpwg.org/specs/rfc7540.html#StreamIdentifiers) )的PUSH\_PROMISE帧，必须将其当做类型为 [PROTOCOL\_ERROR](http://httpwg.org/specs/rfc7540.html#PROTOCOL_ERROR) 的连接错误( [5.4.1节](http://httpwg.org/specs/rfc7540.html#ConnectionErrorHandler) )。注意，非法的流标识符是当前状态不是 空闲(idle) 状态的流的标识符。


> The PUSH\_PROMISE frame can include padding. Padding fields and flags are identical to those defined for DATA frames ([Section 6.1](http://httpwg.org/specs/rfc7540.html#DATA)).

PUSH\_PROMISE帧可以包含填充数据。填充数据域和标识符跟为DATA帧( [6.1节](http://httpwg.org/specs/rfc7540.html#DATA) )定义的相同。