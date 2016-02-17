# Defined SETTINGS Parameters / 已定义的SETTINGS帧参数
> The following parameters are defined:
> 
> * **SETTINGS\_HEADER\_TABLE_SIZE (0x1):** Allows the sender to inform the remote endpoint of the maximum size of the header compression table used to decode header blocks, in octets. The encoder can select any size equal to or less than this value by using signaling specific to the header compression format inside a header block (see [*[COMPRESSION]*](http://httpwg.org/specs/rfc7540.html#COMPRESSION)). The initial value is 4,096 octets.
> 
> * **SETTINGS\_ENABLE_PUSH (0x2):** This setting can be used to disable server push ([Section 8.2](http://httpwg.org/specs/rfc7540.html#PushResources)). An endpoint MUST NOT send a [PUSH\_PROMISE](http://httpwg.org/specs/rfc7540.html#PUSH_PROMISE) frame if it receives this parameter set to a value of 0. An endpoint that has both set this parameter to 0 and had it acknowledged MUST treat the receipt of a [PUSH_PROMISE](http://httpwg.org/specs/rfc7540.html#PUSH_PROMISE) frame as a connection error ([Section 5.4.1](http://httpwg.org/specs/rfc7540.html#ConnectionErrorHandler)) of type [PROTOCOL\_ERROR](http://httpwg.org/specs/rfc7540.html#PROTOCOL_ERROR).
> 
>  The initial value is 1, which indicates that server push is permitted. Any value other than 0 or 1 MUST be treated as a connection error ([Section 5.4.1](http://httpwg.org/specs/rfc7540.html#ConnectionErrorHandler)) of type [PROTOCOL_ERROR](http://httpwg.org/specs/rfc7540.html#PROTOCOL_ERROR).
> 
> * **SETTINGS\_MAX\_CONCURRENT_STREAMS (0x3):** Indicates the maximum number of concurrent streams that the sender will allow. This limit is directional: it applies to the number of streams that the sender permits the receiver to create. Initially, there is no limit to this value. It is recommended that this value be no smaller than 100, so as to not unnecessarily limit parallelism.
> 
> 	A value of 0 for SETTINGS\_MAX\_CONCURRENT_STREAMS SHOULD NOT be treated as special by endpoints. A zero value does prevent the creation of new streams; however, this can also happen for any limit that is exhausted with active streams. Servers SHOULD only set a zero value for short durations; if a server does not wish to accept requests, closing the connection is more appropriate.
> 
> * **SETTINGS\_INITIAL\_WINDOW_SIZE (0x4):** Indicates the sender's initial window size (in octets) for stream-level flow control. The initial value is 216-1 (65,535) octets.
> 
> 	This setting affects the window size of all streams (see [Section 6.9.2](http://httpwg.org/specs/rfc7540.html#InitialWindowSize)).
> 
> 	Values above the maximum flow-control window size of 231-1 MUST be treated as a connection error ([Section 5.4.1](http://httpwg.org/specs/rfc7540.html#ConnectionErrorHandler)) of type [FLOW\_CONTROL_ERROR](http://httpwg.org/specs/rfc7540.html#FLOW_CONTROL_ERROR).
> 
> * **SETTINGS\_MAX\_FRAME_SIZE (0x5):** Indicates the size of the largest frame payload that the sender is willing to receive, in octets.
> 
> 	The initial value is 2^14 (16,384) octets. The value advertised by an endpoint MUST be between this initial value and the maximum allowed frame size (2^24 - 1 or 16,777,215 octets), inclusive. Values outside this range MUST be treated as a connection error ([Section 5.4.1](http://httpwg.org/specs/rfc7540.html#ConnectionErrorHandler)) of type [PROTOCOL_ERROR](http://httpwg.org/specs/rfc7540.html#PROTOCOL_ERROR).
> 
> * **SETTINGS\_MAX\_HEADER\_LIST_SIZE (0x6):** This advisory setting informs a peer of the maximum size of header list that the sender is prepared to accept, in octets. The value is based on the uncompressed size of header fields, including the length of the name and value in octets plus an overhead of 32 octets for each header field.
> 
> 	For any given request, a lower limit than what is advertised MAY be enforced. The initial value of this setting is unlimited.

定义了如下参数：

* **SETTINGS\_HEADER\_TABLE_SIZE (0x1):** 允许发送方通知远端用于解码首部块的首部压缩表的最大字节值。通过使用特定于首部块( 参见 [*[COMPRESSION]*](http://httpwg.org/specs/rfc7540.html#COMPRESSION) )内部的首部压缩格式的信令，编码器可以选择任何小于等于该值的大小。其初始值是4096字节。
* **SETTINGS\_ENABLE_PUSH (0x2):** 该设置用于关闭服务端推送( [8.2节](http://httpwg.org/specs/rfc7540.html#PushResources) )。如果一端收到了该参数值为0，该端点不能发送 [PUSH\_PROMISE](http://httpwg.org/specs/rfc7540.html#PUSH_PROMISE) 帧。如果一端把该参数设置为0，并且收到了确认，当它收到了 [PUSH\_PROMISE](http://httpwg.org/specs/rfc7540.html#PUSH_PROMISE) 帧时，它必须将其当做类型 [PROTOCOL_ERROR](http://httpwg.org/specs/rfc7540.html#PROTOCOL_ERROR) 的连接错误( [5.4.1节](http://httpwg.org/specs/rfc7540.html#ConnectionErrorHandler) )。

  该值初始为1，表示允许服务端推送功能。如果该值不是0或者1，必须将其当做类型为 [PROTOCOL\_ERROR](http://httpwg.org/specs/rfc7540.html#PROTOCOL_ERROR) 的连接错误( [5.4.1节](http://httpwg.org/specs/rfc7540.html#ConnectionErrorHandler) )。

* **SETTINGS\_MAX\_CONCURRENT_STREAMS (0x3):** 指明发送端允许的最大并发流数。该值是有方向性的：它适用于发送端允许接收端创建的流数目。最初，对该值是没有限制的。为了不是非必要地限制并发，推荐该值不小于100。

  端点不应该特殊对待SETTINGS\_MAX\_CONCURRENT_STREAMS为0的值。该值为0的确会阻止创建新流，但是，对任何耗尽限制数内活跃流的情况，也会发生阻止新流的创建。服务端应该只能在短期内设置该值为0。如果服务端不想接收请求，关闭连接更合适。

* **SETTINGS\_INITIAL\_WINDOW_SIZE (0x4):** 指明发送端流级别的流量控制窗口的初始字节大小。该初始值是2^16 - 1 (65,535)字节。
  
  该设置会影响所有流的窗口大小(参见 [6.9.2节](http://httpwg.org/specs/rfc7540.html#InitialWindowSize) )。
  
  如果该值大于流量控制窗口的最大值2^31 - 1，必须将其当做类型为 [FLOW\_CONTROL_ERROR](http://httpwg.org/specs/rfc7540.html#FLOW_CONTROL_ERROR) 的连接错误( [5.4.1节](http://httpwg.org/specs/rfc7540.html#ConnectionErrorHandler) )。
* **SETTINGS\_MAX\_FRAME_SIZE (0x5):** 指明发送端希望接收的最大帧负载的字节值。
  
  初始值是2^14 (16,384)字节。一端告知的该值必须介于初始值和允许的最大帧大小(2^24 - 1 或者 16,777,215字节 )之间，包括该最大值。必须将超出该范围的值当做类型为 [PROTOCOL_ERROR](http://httpwg.org/specs/rfc7540.html#PROTOCOL_ERROR) 的 连接错误( [5.4.1节](http://httpwg.org/specs/rfc7540.html#ConnectionErrorHandler) )。
* **SETTINGS\_MAX\_HEADER\_LIST_SIZE (0x6):** 该建议设置通知对端发送端准备接收的首部列表大小的最大字节值。该值是基于未压缩的首部域大小，包括名称和值的字节长度，外加每个首部域的32字节的开销。

  对于任何给定的请求，其大小必须低于告知的值。该设置的初始值大小是没有限制的。

> An endpoint that receives a SETTINGS frame with any unknown or unsupported identifier MUST ignore that setting.

如果一端收到了带有任何未知的或不支持的标识符的SETTIGNS帧，必须忽略该设置参数。