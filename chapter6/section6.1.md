# DATA / DATA帧
> DATA frames (type=0x0) convey arbitrary, variable-length sequences of octets associated with a stream. One or more DATA frames are used, for instance, to carry HTTP request or response payloads.

DATA帧(type=0x0)用于传送某一个流的任意的、可变长度的字节序列。比如：用一个或多个DATA帧来携带HTTP请求或响应的载荷。


> DATA frames MAY also contain padding. Padding can be added to DATA frames to obscure the size of messages. Padding is a security feature; see [Section 10.7](https://httpwg.github.io/specs/rfc7540.html#padding).
 
> ```
> +---------------+
> |Pad Length? (8)|
> +---------------+-----------------------------------------------+
> |                            Data (*)                         ...
> +---------------------------------------------------------------+
> |                           Padding (*)                       ...
> +---------------------------------------------------------------+
> 						Figure 6: DATA Frame Payload
> ```

DATA帧也可以包含填充数。为了模糊消息的大小，可以在DATA帧里加入填充数。填充数是一种安全特性，参见 [10.7节](https://httpwg.github.io/specs/rfc7540.html#padding)。

```
+---------------+
|Pad Length? (8)|
+---------------+-----------------------------------------------+
|                            Data (*)                         ...
+---------------------------------------------------------------+
|                           Padding (*)                       ...
+---------------------------------------------------------------+
							图 6：DATA帧载荷
```


> The DATA frame contains the following fields:
> 
> * **Pad Length:** An 8-bit field containing the length of the frame padding in units of octets. This field is conditional (as signified by a "?" in the diagram) and is only present if the PADDED flag is set.
> 
> * **Data:** Application data. The amount of data is the remainder of the frame payload after subtracting the length of the other fields that are present.
> 
> * **Padding:** Padding octets that contain no application semantic value. Padding octets MUST be set to zero when sending. A receiver is not obligated to verify padding but MAY treat non-zero padding as a connection error ([Section 5.4.1](https://httpwg.github.io/specs/rfc7540.html#ConnectionErrorHandler)) of type [PROTOCOL_ERROR](https://httpwg.github.io/specs/rfc7540.html#PROTOCOL_ERROR).

DATA帧包含如下域：

* **填充长度(Pad Length)**：一个8bit的域，包含帧填充数据的字节长度。该域是可选的(正如框图中的"?"所示)，只有当设置了PADDED标识时，才会有该域。
* **数据(Data)**：应用数据。数据量等于帧载荷减去其它域的长度。
* **填充数据(Padding)**：不包含应用语义值的填充字节。当发送的时候，必须将填充数设置为0。接收方不必非得校验填充数据，但是可能会把非零的填充数当做 [PROTOCOL_ERROR](https://httpwg.github.io/specs/rfc7540.html#PROTOCOL_ERROR) 类型的连接错误( [5.4.1节](https://httpwg.github.io/specs/rfc7540.html%23ConnectionErrorHandler) )。


> The DATA frame defines the following flags:
> 
> * **END_STREAM (0x1):** When set, bit 0 indicates that this frame is the last that the endpoint will send for the identified stream. Setting this flag causes the stream to enter one of the "half-closed" states or the "closed" state ([Section 5.1](https://httpwg.github.io/specs/rfc7540.html#StreamStates)).
> 
> * **PADDED (0x8):** When set, bit 3 indicates that the Pad Length field and any padding that it describes are present.

DATA帧定义了如下标识：

*  **END_STREAM(0x1)**：当设置了该标识，第0位就指明了该帧是端点在指定流上发送的最后一帧。设置该标识会使流进入半关闭状态之一或者关闭状态( [5.1节](https://httpwg.github.io/specs/rfc7540.html#StreamStates) )。
*  **PADDED(0x8)**：当设置了该标识，第3位表示存在填充长度域和相应的填充数据。


> DATA frames MUST be associated with a stream. If a DATA frame is received whose stream identifier field is 0x0, the recipient MUST respond with a connection error ([Section 5.4.1](https://httpwg.github.io/specs/rfc7540.html#ConnectionErrorHandler)) of type [PROTOCOL_ERROR](https://httpwg.github.io/specs/rfc7540.html#PROTOCOL_ERROR).

DATA帧必须和某一个流相关联。如果收到一个流标识符域为0x0的DATA帧，接收方必须响应一个 [PROTOCOL_ERROR](https://httpwg.github.io/specs/rfc7540.html#PROTOCOL_ERROR) 类型的连接错误( [5.4.1节](https://httpwg.github.io/specs/rfc7540.html#ConnectionErrorHandler) )。


> DATA frames are subject to flow control and can only be sent when a stream is in the "open" or "half-closed (remote)" state. The entire DATA frame payload is included in flow control, including the Pad Length and Padding fields if present. If a DATA frame is received whose stream is not in "open" or "half-closed (local)" state, the recipient MUST respond with a stream error ([Section 5.4.2](https://httpwg.github.io/specs/rfc7540.html#StreamErrorHandler)) of type [STREAM_CLOSED](https://httpwg.github.io/specs/rfc7540.html#STREAM_CLOSED).

DATA帧受流量控制限制，并且只能在流处于 打开(open) 或者 半关闭(远端)(half-closed (remote)) 状态时发送。整个DATA帧载荷都受流量控制限制，如果有的话，也包括 填充长度(Pad Length) 域和 填充数据(Padding) 域。如果流不是 打开(open) 或者 半关闭(本地)(half-closed (local)) 状态时收到了DATA帧，接收方必须响应一个 [STREAM_CLOSED](https://httpwg.github.io/specs/rfc7540.html#STREAM_CLOSED) 类型的流错误( [5.4.2节](https://httpwg.github.io/specs/rfc7540.html#StreamErrorHandler) )。


> The total number of padding octets is determined by the value of the Pad Length field. If the length of the padding is the length of the frame payload or greater, the recipient MUST treat this as a connection error ([Section 5.4.1](https://httpwg.github.io/specs/rfc7540.html#ConnectionErrorHandler)) of type [PROTOCOL_ERROR](https://httpwg.github.io/specs/rfc7540.html#PROTOCOL_ERROR).

填充长度(Pad Length) 域的值决定了填充数据的字节总数。如果填充数据的长度大于等于帧载荷的长度，接收方必须把这种情况当做 [PROTOCOL_ERROR](https://httpwg.github.io/specs/rfc7540.html#PROTOCOL_ERROR) 类型的连接错误( [5.4.1节](https://httpwg.github.io/specs/rfc7540.html#ConnectionErrorHandler) )。


> Note: A frame can be increased in size by one octet by including a Pad Length field with a value of zero.

注意：通过包含一个值为零的 填充长度(Pad Length) 域，帧大小可以增加一个字节。