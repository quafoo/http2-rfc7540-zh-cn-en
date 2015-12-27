# Frame Format / 帧格式
> All frames begin with a fixed 9-octet header followed by a variable-length payload.
> 
> ```
> +-----------------------------------------------+
> |                 Length (24)                   |
> +---------------+---------------+---------------+
> |   Type (8)    |   Flags (8)   |
> +-+-------------+---------------+-------------------------------+
> |R|                 Stream Identifier (31)                      |
> +=+=============================================================+
> |                   Frame Payload (0...)                      ..
> +---------------------------------------------------------------+
> 						Figure 1: Frame Layout
> ```

所有的帧都以固定的9字节的首部开始，后面接可变长度的有效载荷。

```
 +-----------------------------------------------+
 |                 Length (24)                   |
 +---------------+---------------+---------------+
 |   Type (8)    |   Flags (8)   |
 +-+-------------+---------------+-------------------------------+
 |R|                 Stream Identifier (31)                      |
 +=+=============================================================+
 |                   Frame Payload (0...)                      ...
 +---------------------------------------------------------------+
							图 1：帧格式
```

> The fields of the frame header are defined as:
> 
> * **Length:** The length of the frame payload expressed as an unsigned 24-bit integer. Values greater than 214 (16,384) MUST NOT be sent unless the receiver has set a larger value for SETTINGS_MAX_FRAME_SIZE.
> 
> 	The 9 octets of the frame header are not included in this value.
> 
> * **Type:** The 8-bit type of the frame. The frame type determines the format and semantics of the frame. Implementations MUST ignore and discard any frame that has a type that is unknown.
> 
> * **Flags:** An 8-bit field reserved for boolean flags specific to the frame type.
> 
> 	Flags are assigned semantics specific to the indicated frame type. Flags that have no defined semantics for a particular frame type MUST be ignored and MUST be left unset (0x0) when sending.
> 
> * **R:** A reserved 1-bit field. The semantics of this bit are undefined, and the bit MUST remain unset (0x0) when sending and MUST be ignored when receiving.
> 
> * **Stream Identifier:** A stream identifier (see Section 5.1.1) expressed as an unsigned 31-bit integer. The value 0x0 is reserved for frames that are associated with the connection as a whole as opposed to an individual stream.

> The structure and content of the frame payload is dependent entirely on the frame type.

帧首部字段定义如下：

* **Length:** 帧有效载荷的长度，以24位无符号整数表示。除非接收端通过SETTINGS\_MAX\_FRAME\_SIZE设置了更大的值，否则不能发送长度值大于2^14 (16,384)的帧。
* **Type:** 帧的8bit类型。帧类型决定了帧的格式和语义。必须忽略和丢弃任何未知帧类型。
* **Flags:** 为帧类型保留的8bit布尔标识字段。

	针对确定的帧类型赋予标识特定的语义。定义在确定的帧类型之外的标识语义必须被忽略，并且在发送时必须是未设置的(0x0)。
* **R:** 1bit的保留字段。未定义该bit的语义。当发送时，该bit必须是未设置的(0x0)；当接收时，必须忽略该bit。
* **Stream Identifier:** 流标识符(5.1.1节)是一个31bit的无符号整数。值0x0是保留的，标明帧是与整体的连接相关的，而不是和单独的流相关。

帧有效载荷结构和内容完全取决于帧类型。