# PING / PING帧
> The PING frame (type=0x6) is a mechanism for measuring a minimal round-trip time from the sender, as well as determining whether an idle connection is still functional. PING frames can be sent from any endpoint.
> 
> ```
> +---------------------------------------------------------------+
> |                                                               |
> |                      Opaque Data (64)                         |
> |                                                               |
> +---------------------------------------------------------------+
> 						Figure 12: PING Payload Format
> ```

除了判断一个空闲的连接是否仍然可用之外，PING帧(type=0x6)还是发送端测量最小往返时间的一种机制。任何端点都可以发送PING帧。

```
+---------------------------------------------------------------+
|                                                               |
|                      Opaque Data (64)                         |
|                                                               |
+---------------------------------------------------------------+
						图 12: PING帧的负载格式
```


> In addition to the frame header, PING frames MUST contain 8 octets of opaque data in the payload. A sender can include any value it chooses and use those octets in any fashion.

除了帧首部，PING帧还必须在负载中包含8字节的不透明数据。发送端可以选择任意值，而且可以以任意形式使用这些值。


> Receivers of a PING frame that does not include an ACK flag MUST send a PING frame with the ACK flag set in response, with an identical payload. PING responses SHOULD be given higher priority than any other frame.

如果接收端收到了不包含ACK标识的PING帧，必须响应一个设置了ACK标识的PING帧，其负载与收到的PING帧的负载相同。PING帧的响应应该被赋予最高优先级。


> The PING frame defines the following flags:
> 
> * **ACK (0x1):** When set, bit 0 indicates that this PING frame is a PING response. An endpoint MUST set this flag in PING responses. An endpoint MUST NOT respond to PING frames containing this flag.

PING帧定义了如下标识符：

* **ACK (0x1):** 当设置了该标识符，第0位表示该PING帧是一个PING帧的响应。端点必须在PING帧的响应里设置该标识符。端点不能响应包含该标识符的PING帧。

> PING frames are not associated with any individual stream. If a PING frame is received with a stream identifier field value other than 0x0, the recipient MUST respond with a connection error ([Section 5.4.1](http://httpwg.org/specs/rfc7540.html#ConnectionErrorHandler)) of type [PROTOCOL_ERROR](http://httpwg.org/specs/rfc7540.html#PROTOCOL_ERROR).

PING帧不能与任何流相关联。如果收到了流标识符域的值不是0x0的PING帧，接收端必须响应一个类型为 [PROTOCOL\_ERROR](http://httpwg.org/specs/rfc7540.html#PROTOCOL_ERROR) 的连接错误( [5.4.1节](http://httpwg.org/specs/rfc7540.html#ConnectionErrorHandler) )。


> Receipt of a PING frame with a length field value other than 8 MUST be treated as a connection error ([Section 5.4.1](http://httpwg.org/specs/rfc7540.html#ConnectionErrorHandler)) of type [FRAME\_SIZE\_ERROR](http://httpwg.org/specs/rfc7540.html#FRAME_SIZE_ERROR).

如果收到了长度域的值不是8的PING帧，必须将其当做类型为 [FRAME\_SIZE\_ERROR](http://httpwg.org/specs/rfc7540.html#FRAME_SIZE_ERROR) 的连接错误( [5.4.1节](http://httpwg.org/specs/rfc7540.html#ConnectionErrorHandler) )。

