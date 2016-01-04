# HEADERS / HEADERS帧
> The HEADERS frame (type=0x1) is used to open a stream (Section 5.1), and additionally carries a header block fragment. HEADERS frames can be sent on a stream in the "idle", "reserved (local)", "open", or "half-closed (remote)" state.
> 
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
>             Figure 7: HEADERS Frame Payload
> ```

HEADERS帧(type=0x1)用来打开一个流(5.1节)，再额外地携带首部块段(Header Block Fragment)。HEADERS帧可以在一个流处于空闲、保留、打开、或者半关闭状态时被发送。

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
> * **E:** A single-bit flag indicating that the stream dependency is exclusive (see Section 5.3). This field is only present if the PRIORITY flag is set.
> * **Stream Dependency:** A 31-bit stream identifier for the stream that this stream depends on (see Section 5.3). This field is only present if the PRIORITY flag is set.
> * **Weight:** An unsigned 8-bit integer representing a priority weight for the stream (see Section 5.3). Add one to the value to obtain a weight between 1 and 256. This field is only present if the PRIORITY flag is set.
> * **Header Block Fragment:** A header block fragment (Section 4.3).
> * **Padding:** Padding octets.

HEADERS帧载荷包含如下域：

* **填充长度(Pad Length)**：一个8bit的域，包含帧填充数据的字节长度。只有当设置了*PADDED*标识时，才会出现该域。
* **E标识**：1bit标识，表示排除流依赖(参见5.3节)。只有当设置了*PRIORITY*标识，才会出现该域。
* **流依赖(Stream Dependency)**：该流所依赖的流(参见5.3节)的31bit标识符。只有当设置了*PRIORITY*标识，才会出现该域。
* **权重(Weight)**：一个8bit的无符号整数，表示该流的优先级权重(参见5.3节)。范围是1到255。只有当设置了*PRIORITY*标识，才会出现该域。
* **首部块段(Header Block Fragment)**：一个首部块段(参见4.3节)。
* **填充数据(Padding)**：填充字节。

> The HEADERS frame defines the following flags:
> 
> * **END_STREAM (0x1):** When set, bit 0 indicates that the header block (Section 4.3) is the last that the endpoint will send for the identified stream.
> 
> 	A HEADERS frame carries the END\_STREAM flag that signals the end of a stream. However, a HEADERS frame with the END_STREAM flag set can be followed by CONTINUATION frames on the same stream. Logically, the CONTINUATION frames are part of the HEADERS frame.
> 
> * **END_HEADERS (0x4):** When set, bit 2 indicates that this frame contains an entire header block (Section 4.3) and is not followed by any CONTINUATION frames.
> 
> 	A HEADERS frame without the END\_HEADERS flag set MUST be followed by a CONTINUATION frame for the same stream. A receiver MUST treat the receipt of any other type of frame or a frame on a different stream as a connection error (Section 5.4.1) of type PROTOCOL_ERROR.
> 
> * **PADDED (0x8):** When set, bit 3 indicates that the Pad Length field and any padding that it describes are present.
> 
> * **PRIORITY (0x20):** When set, bit 5 indicates that the Exclusive Flag (E), Stream Dependency, and Weight fields are present; see Section 5.3.

HEADERS帧定义了如下标识：

* **END_STREAM(0x1)**：当设置了
* **END_HEADERS(0x4)**：
* **PADDED(0x8)**：
* **PRIORITY(0x20)**：

> The payload of a HEADERS frame contains a header block fragment (Section 4.3). A header block that does not fit within a HEADERS frame is continued in a CONTINUATION frame (Section 6.10).

> HEADERS frames MUST be associated with a stream. If a HEADERS frame is received whose stream identifier field is 0x0, the recipient MUST respond with a connection error (Section 5.4.1) of type PROTOCOL_ERROR.

> The HEADERS frame changes the connection state as described in Section 4.3.

> The HEADERS frame can include padding. Padding fields and flags are identical to those defined for DATA frames (Section 6.1). Padding that exceeds the size remaining for the header block fragment MUST be treated as a PROTOCOL_ERROR.

> Prioritization information in a HEADERS frame is logically equivalent to a separate PRIORITY frame, but inclusion in HEADERS avoids the potential for churn in stream prioritization when new streams are created. Prioritization fields in HEADERS frames subsequent to the first on a stream reprioritize the stream (Section 5.3.3).