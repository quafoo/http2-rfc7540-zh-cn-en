# SETTINGS / SETTINGS帧
> The SETTINGS frame (type=0x4) conveys configuration parameters that affect how endpoints communicate, such as preferences and constraints on peer behavior. The SETTINGS frame is also used to acknowledge the receipt of those parameters. Individually, a SETTINGS parameter can also be referred to as a "setting".

SETTINGS帧(type=0x4)用来传送影响两端通信的配置参数，比如：对对端行为的偏好与约束等。SETTINGS帧也用于通知对端自己收到了这些参数。特别地，SETTIGNS参数也可以被称做 设置(setting)。


> SETTINGS parameters are not negotiated; they describe characteristics of the sending peer, which are used by the receiving peer. Different values for the same parameter can be advertised by each peer. For example, a client might set a high initial flow-control window, whereas a server might set a lower value to conserve resources.

SETTINGS参数不是靠协商得来的。这些参数描述了发送端的特性，并被接收端所使用。对于相同的参数，两端可以使用不同的值。例如，客户端可以设置一个较大的流量控制窗口 (flow-control window) 值，而服务端为了保存资源，可以设置一个较小的值。


> A SETTINGS frame MUST be sent by both endpoints at the start of a connection and MAY be sent at any other time by either endpoint over the lifetime of the connection. Implementations MUST support all of the parameters defined by this specification.

在连接建立的时候两端必须发送SETTIGNS帧，也可以在连接的整个生命周期内由任何一端在任意时间发送SETTIGNS帧。实现必须支持本规范文档定义的所有参数。


> Each parameter in a SETTINGS frame replaces any existing value for that parameter. Parameters are processed in the order in which they appear, and a receiver of a SETTINGS frame does not need to maintain any state other than the current value of its parameters. Therefore, the value of a SETTINGS parameter is the last value that is seen by a receiver.

SETTIGNS帧里的参数会各自替换该参数已存在的值。以参数出现的顺序来处理参数，除了参数的当前值，SETTIGNS帧的接收方不需要维护任何状态。因此，接收方看到的最后的值就是SETTIGNS帧的参数值。


> SETTINGS parameters are acknowledged by the receiving peer. To enable this, the SETTINGS frame defines the following flag:
> 
> * **ACK (0x1):** When set, bit 0 indicates that this frame acknowledges receipt and application of the peer's SETTINGS frame. When this bit is set, the payload of the SETTINGS frame MUST be empty. Receipt of a SETTINGS frame with the ACK flag set and a length field value other than 0 MUST be treated as a connection error ([Section 5.4.1](http://httpwg.org/specs/rfc7540.html#ConnectionErrorHandler)) of type [FRAME\_SIZE_ERROR](http://httpwg.org/specs/rfc7540.html#FRAME_SIZE_ERROR). For more information, see [Section 6.5.3](http://httpwg.org/specs/rfc7540.html#SettingsSync) ("[Settings Synchronization](http://httpwg.org/specs/rfc7540.html#SettingsSync)").

SETTINGS帧携带的参数会被接收方确认。为了做到这个，SETTINGS帧定义了如下标识：

* **ACK (0x1):** 当设置了该标识，bit 0表示该帧确认收到并应用了对端的SETTIGNS帧。当设置了该bit，SETTIGNS帧的负载必须为空。如果收到了设置了ACK标识，并且长度域的值不为0的SETTIGNS帧，必须将其当做类型为[FRAME\_SIZE_ERROR](http://httpwg.org/specs/rfc7540.html#FRAME_SIZE_ERROR)的连接错误( [5.4.1节](http://httpwg.org/specs/rfc7540.html#ConnectionErrorHandler) )。更多信息，参见 [6.5.3节](http://httpwg.org/specs/rfc7540.html#SettingsSync) ( "[Settings Synchronization](http://httpwg.org/specs/rfc7540.html#SettingsSync)" )。


> SETTINGS frames always apply to a connection, never a single stream. The stream identifier for a SETTINGS frame MUST be zero (0x0). If an endpoint receives a SETTINGS frame whose stream identifier field is anything other than 0x0, the endpoint MUST respond with a connection error ([Section 5.4.1](http://httpwg.org/specs/rfc7540.html#ConnectionErrorHandler)) of type [PROTOCOL_ERROR](http://httpwg.org/specs/rfc7540.html#PROTOCOL_ERROR).

SETTIGNS帧总是作用于连接，而不是一个流。SETTIGNS帧的流标识符必须为0(0x0)。如果一端收到了流标识符不是0x0的SETTIGNS帧，该端点必须响应一个类型为 [PROTOCOL_ERROR](http://httpwg.org/specs/rfc7540.html#PROTOCOL_ERROR) 的连接错误( [Section 5.4.1](http://httpwg.org/specs/rfc7540.html#ConnectionErrorHandler) )。


> The SETTINGS frame affects connection state. A badly formed or incomplete SETTINGS frame MUST be treated as a connection error ([Section 5.4.1](http://httpwg.org/specs/rfc7540.html#ConnectionErrorHandler)) of type [PROTOCOL_ERROR](http://httpwg.org/specs/rfc7540.html#PROTOCOL_ERROR).

SETTINGS帧影响连接状态。必须将损坏的或者不完整的SETTIGNS帧当做类型为 [PROTOCOL_ERROR](http://httpwg.org/specs/rfc7540.html#PROTOCOL_ERROR) 的连接错误( [5.4.1节](http://httpwg.org/specs/rfc7540.html#ConnectionErrorHandler) )。


> A SETTINGS frame with a length other than a multiple of 6 octets MUST be treated as a connection error (Section 5.4.1) of type FRAME\_SIZE_ERROR.

必须将长度不是6的倍数的SETTINGS帧当做类型为 [FRAME\_SIZE_ERROR](http://httpwg.org/specs/rfc7540.html#FRAME_SIZE_ERROR) 的连接错误( [5.4.1节](http://httpwg.org/specs/rfc7540.html#ConnectionErrorHandler) )。