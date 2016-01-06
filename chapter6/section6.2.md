# HEADERS / HEADERS帧
> The HEADERS frame (type=0x1) is used to open a stream ([Section 5.1](https://httpwg.github.io/specs/rfc7540.html#StreamStates)), and additionally carries a header block fragment. HEADERS frames can be sent on a stream in the "idle", "reserved (local)", "open", or "half-closed (remote)" state.
 
> ```
> +---------------+
> |Pad Length? (8)|
> +-+-------------+-----------------------------------------------+
> |E|                 Stream Dependency? (31)                     |
> +-+-------------+-----------------------------------------------+
> |  Weight? (8)  |
> +-+-------------+-----------------------------------------------+
> |                   Header Block Fragment (*)                 ...
> +---------------------------------------------------------------+
> |                           Padding (*)                       ...
> +---------------------------------------------------------------+
>              Figure 7: HEADERS Frame Payload
> ```

HEADERS帧(type=0x1)用来打开一个流( [5.1节](https://httpwg.github.io/specs/rfc7540.html#StreamStates) )，再额外地携带一个 首部块片段(Header Block Fragment)。HEADERS帧可以在一个流处于 空闲(idle)、保留(本地)(reserved (local))、打开(open)、或者 半关闭(远端)(half-closed (remote)) 状态时被发送。

```
+---------------+
|Pad Length? (8)|
+-+-------------+-----------------------------------------------+
|E|                 Stream Dependency? (31)                     |
+-+-------------+-----------------------------------------------+
|  Weight? (8)  |
+-+-------------+-----------------------------------------------+
|                   Header Block Fragment (*)                 ...
+---------------------------------------------------------------+
|                           Padding (*)                       ...
+---------------------------------------------------------------+
						图 7：HEADERS帧载荷
```


> The HEADERS frame payload has the following fields:
>  
> * **Pad Length:** An 8-bit field containing the length of the frame padding in units of octets. This field is only present if the PADDED flag is set.
> 
> * **E:** A single-bit flag indicating that the stream dependency is exclusive (see [Section 5.3](https://httpwg.github.io/specs/rfc7540.html#StreamPriority)). This field is only present if the PRIORITY flag is set.
> 
> * **Stream Dependency:** A 31-bit stream identifier for the stream that this stream depends on (see [Section 5.3](https://httpwg.github.io/specs/rfc7540.html#StreamPriority)). This field is only present if the PRIORITY flag is set.
> 
> * **Weight:** An unsigned 8-bit integer representing a priority weight for the stream (see [Section 5.3](https://httpwg.github.io/specs/rfc7540.html#StreamPriority)). Add one to the value to obtain a weight between 1 and 256. This field is only present if the PRIORITY flag is set.
> 
> * **Header Block Fragment:** A header block fragment ([Section 4.3](https://httpwg.github.io/specs/rfc7540.html#HeaderBlock)).
> 
> * **Padding:** Padding octets.

HEADERS帧载荷包含如下域：

* **填充长度(Pad Length)**：一个8bit的域，包含帧填充数据的字节长度。只有当设置了PADDED标识时，才会有该域。
* **E标识**：1bit标识，表示流依赖是否是专用的(参见 [5.3节](https://httpwg.github.io/specs/rfc7540.html#StreamPriority) )。只有当设置了PRIORITY标识，才会有该域。
* **流依赖(Stream Dependency)**：该流所依赖的流(参见 [5.3节](https://httpwg.github.io/specs/rfc7540.html#StreamPriority) )的31bit标识符。只有当设置了PRIORITY标识，才会有该域。
* **权重(Weight)**：一个8bit的无符号整数，表示该流的优先级权重(参见 [5.3节](https://httpwg.github.io/specs/rfc7540.html#StreamPriority) )。范围是1到255。只有当设置了PRIORITY标识，才会有该域。
* **首部块片段(Header Block Fragment)**：一个首部块片段(参见 [4.3节](https://httpwg.github.io/specs/rfc7540.html#HeaderBlock) )。
* **填充数据(Padding)**：填充字节。


> The HEADERS frame defines the following flags:
> 
> * **END_STREAM (0x1):** When set, bit 0 indicates that the header block ([Section 4.3](https://httpwg.github.io/specs/rfc7540.html#HeaderBlock)) is the last that the endpoint will send for the identified stream.
> 
> 	A HEADERS frame carries the END\_STREAM flag that signals the end of a stream. However, a HEADERS frame with the END_STREAM flag set can be followed by [CONTINUATION](https://httpwg.github.io/specs/rfc7540.html#CONTINUATION) frames on the same stream. Logically, the [CONTINUATION](https://httpwg.github.io/specs/rfc7540.html#CONTINUATION) frames are part of the HEADERS frame.
> 
> * **END_HEADERS (0x4):** When set, bit 2 indicates that this frame contains an entire header block ([Section 4.3](https://httpwg.github.io/specs/rfc7540.html#HeaderBlock)) and is not followed by any [CONTINUATION](https://httpwg.github.io/specs/rfc7540.html#CONTINUATION) frames.
> 
> 	A HEADERS frame without the END\_HEADERS flag set MUST be followed by a [CONTINUATION](https://httpwg.github.io/specs/rfc7540.html#CONTINUATION) frame for the same stream. A receiver MUST treat the receipt of any other type of frame or a frame on a different stream as a connection error ([Section 5.4.1](https://httpwg.github.io/specs/rfc7540.html#ConnectionErrorHandler)) of type [PROTOCOL_ERROR](https://httpwg.github.io/specs/rfc7540.html#PROTOCOL_ERROR).
> 
> * **PADDED (0x8):** When set, bit 3 indicates that the Pad Length field and any padding that it describes are present.
> 
> * **PRIORITY (0x20):** When set, bit 5 indicates that the Exclusive Flag (E), Stream Dependency, and Weight fields are present; see [Section 5.3](https://httpwg.github.io/specs/rfc7540.html#StreamPriority).

HEADERS帧定义了如下标识：

* **END_STREAM(0x1)**：当设置了该标识，第0 bit位就指明了该首部块是端点在指定流上发送的最后一帧( [4.3节](https://httpwg.github.io/specs/rfc7540.html#HeaderBlock) )。

	HEADERS帧携带的END\_STREAM标识指明流要结束了。但是，在同一个流上，设置了END\_STREAM标识的HEADERS帧后面还可以有 [CONTINUATION](https://httpwg.github.io/specs/rfc7540.html#CONTINUATION) 帧。从逻辑上来说，[CONTINUATION](https://httpwg.github.io/specs/rfc7540.html#CONTINUATION) 帧也是HEADERS帧的一部分。
	
* **END_HEADERS(0x4)**：当设置了该标识，第2 bit位表示该帧包含整个首部块( [4.3节](https://httpwg.github.io/specs/rfc7540.html#HeaderBlock) )，后面不会有任何 [CONTINUATION](https://httpwg.github.io/specs/rfc7540.html#CONTINUATION) 帧。

	对于同一个流，没有设置END\_HEADERS标识的HEADERS帧后面必须跟一个 [CONTINUATION](https://httpwg.github.io/specs/rfc7540.html#CONTINUATION) 帧。如果接收到任何其他类型的帧，或者其他流上的帧，接收方必须将其看做 [PROTOCOL\_ERROR](https://httpwg.github.io/specs/rfc7540.html#PROTOCOL_ERROR) 类型的连接错误( [5.4.1节](https://httpwg.github.io/specs/rfc7540.html#ConnectionErrorHandler) )。

* **PADDED(0x8)**：当设置了该标识，第3 bit位表示存在 填充长度(Pad Length) 域和 填充数据(padding)。
* **PRIORITY(0x20)**：当设置了该标识，第5 bit位表示存在 独占标识(E)(Exclusive Flag (E))、流依赖(Stream Dependency) 和 权重(Weight) 域。参加 [5.3节](https://httpwg.github.io/specs/rfc7540.html#StreamPriority)


> The payload of a HEADERS frame contains a header block fragment ([Section 4.3](https://httpwg.github.io/specs/rfc7540.html#HeaderBlock)). A header block that does not fit within a HEADERS frame is continued in a CONTINUATION frame ([Section 6.10](https://httpwg.github.io/specs/rfc7540.html#CONTINUATION)).

HEADERS帧的载荷包含一个首部块片段( [4.3节](https://httpwg.github.io/specs/rfc7540.html#HeaderBlock) )。同一个首部块在一个HEADERS帧里装不下就继续装入CONTINUATION帧( [6.10节](https://httpwg.github.io/specs/rfc7540.html#CONTINUATION) )。

> HEADERS frames MUST be associated with a stream. If a HEADERS frame is received whose stream identifier field is 0x0, the recipient MUST respond with a connection error ([Section 5.4.1](https://httpwg.github.io/specs/rfc7540.html#ConnectionErrorHandler)) of type [PROTOCOL_ERROR](https://httpwg.github.io/specs/rfc7540.html#PROTOCOL_ERROR).

HEADERS帧必须与某一个流相关联。如果收到一个流标识符域为0x0的HEADERS帧，接收方必须响应一个 [PROTOCOL_ERROR](https://httpwg.github.io/specs/rfc7540.html#PROTOCOL_ERROR) 类型的连接错误( [5.4.1节](https://httpwg.github.io/specs/rfc7540.html#ConnectionErrorHandler) )。

> The HEADERS frame changes the connection state as described in [Section 4.3](https://httpwg.github.io/specs/rfc7540.html#HeaderBlock).

HEADERS帧改变连接状态在 [4.3节](https://httpwg.github.io/specs/rfc7540.html#HeaderBlock) 中有表述。

> The HEADERS frame can include padding. Padding fields and flags are identical to those defined for DATA frames ([Section 6.1](https://httpwg.github.io/specs/rfc7540.html#DATA)). Padding that exceeds the size remaining for the header block fragment MUST be treated as a PROTOCOL_ERROR.

HEADERS帧可以包含填充数据。填充数据域和填充标识与DATA帧( [6.1节](https://httpwg.github.io/specs/rfc7540.html#DATA) )中定义的一样。如果填充数据量超出了为首部块片段预留的大小，必须将其处理为 [PROTOCOL_ERROR](https://httpwg.github.io/specs/rfc7540.html#PROTOCOL_ERROR)。

> Prioritization information in a HEADERS frame is logically equivalent to a separate [PRIORITY](https://httpwg.github.io/specs/rfc7540.html#PRIORITY) frame, but inclusion in HEADERS avoids the potential for churn in stream prioritization when new streams are created. Prioritization fields in HEADERS frames subsequent to the first on a stream reprioritize the stream ([Section 5.3.3](https://httpwg.github.io/specs/rfc7540.html#reprioritize)).

HEADERS帧里的优先级信息逻辑上等价于一个单独的 [PRIORITY](https://httpwg.github.io/specs/rfc7540.html#PRIORITY) 帧，但是包含在HEADERS帧里可以避免创建新流时对流优先级潜在的扰动。一个流上第一个HEADERS帧之后的HEADERS帧里的优先级域会变更该流的优先级顺序( [5.3.3节](https://httpwg.github.io/specs/rfc7540.html#reprioritize) )。