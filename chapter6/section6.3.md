# PRIORITY / PRIORITY帧
> The PRIORITY frame (type=0x2) specifies the sender-advised priority of a stream ([Section 5.3](https://httpwg.github.io/specs/rfc7540.html#StreamPriority)). It can be sent in any stream state, including idle or closed streams.
> 
> ```
> +-+-------------------------------------------------------------+
> |E|                  Stream Dependency (31)                     |
> +-+-------------+-----------------------------------------------+
> |   Weight (8)  |
> +-+-------------+
> 				Figure 8: PRIORITY Frame Payload
> ```

PRIORITY帧(type=0x2)指定了发送者建议的流优先级( [5.3节](https://httpwg.github.io/specs/rfc7540.html#StreamPriority) )。可以在任何流状态下发送PRIORITY帧，包括空闲(idle)的 和 关闭(closed) 的流。

```
+-+-------------------------------------------------------------+
|E|                  Stream Dependency (31)                     |
+-+-------------+-----------------------------------------------+
|   Weight (8)  |
+-+-------------+
						图 8: PRIORITY帧载荷
```
 

> The payload of a PRIORITY frame contains the following fields:
> 
> * **E:** A single-bit flag indicating that the stream dependency is exclusive (see [Section 5.3](https://httpwg.github.io/specs/rfc7540.html#StreamPriority)).
> * **Stream Dependency:** A 31-bit stream identifier for the stream that this stream depends on (see [Section 5.3](https://httpwg.github.io/specs/rfc7540.html#StreamPriority)).
> * **Weight:** An unsigned 8-bit integer representing a priority weight for the stream (see [Section 5.3](https://httpwg.github.io/specs/rfc7540.html#StreamPriority)). Add one to the value to obtain a weight between 1 and 256.

PRIORITY帧的载荷包含以下域：

* **E标识(E):** 1bit标识，指明流依赖(Stream Dependency)是否是专用的(参见 [5.3节](https://httpwg.github.io/specs/rfc7540.html#StreamPriority) )。
* **流依赖(Stream Dependency):** 该流所依赖的流(参见 [5.3节](https://httpwg.github.io/specs/rfc7540.html#StreamPriority) )的31bit标识符。
* **权重(Weight):** 一个8bit的无符号整数，表示该流的优先级权重(参见 [5.3节](https://httpwg.github.io/specs/rfc7540.html#StreamPriority) )。范围是1到255。


> The PRIORITY frame does not define any flags.

PRIORITY帧没有定义任何标识。 


> The PRIORITY frame always identifies a stream. If a PRIORITY frame is received with a stream identifier of 0x0, the recipient MUST respond with a connection error ([Section 5.4.1](https://httpwg.github.io/specs/rfc7540.html#ConnectionErrorHandler)) of type [PROTOCOL_ERROR](https://httpwg.github.io/specs/rfc7540.html#PROTOCOL_ERROR).

PRIORITY帧总是标识出了某一个流。如果收到一个流标识符为0x0的PRIORITY帧，接收方必须响应一个 [PROTOCOL_ERROR](https://httpwg.github.io/specs/rfc7540.html#PROTOCOL_ERROR) 类型的连接错误( [5.4.1节](https://httpwg.github.io/specs/rfc7540.html#ConnectionErrorHandler) )。

 
> The PRIORITY frame can be sent on a stream in any state, though it cannot be sent between consecutive frames that comprise a single header block ([Section 4.3](https://httpwg.github.io/specs/rfc7540.html#HeaderBlock)). Note that this frame could arrive after processing or frame sending has completed, which would cause it to have no effect on the identified stream. For a stream that is in the "half-closed (remote)" or "closed" state, this frame can only affect processing of the identified stream and its dependent streams; it does not affect frame transmission on that stream.
 
可以在处于任意状态的流上发送PRIORITY帧，但却不能在包含同一个首部块[4.3节](https://httpwg.github.io/specs/rfc7540.html#HeaderBlock)的连续帧之间发送。需要注意的是，PRIORITY帧可能会在处理或发送帧完成后到达，这会导致PRIORITY帧在特定的流上不起作用。对处于 半关闭(远端)(half-closed (remote)) 或者 关闭(closed) 状态的流来说，PRIORITY帧只能影响对该特定流及其从属流的处理，而不会影响在该流上的帧传送。


> The PRIORITY frame can be sent for a stream in the "idle" or "closed" state. This allows for the reprioritization of a group of dependent streams by altering the priority of an unused or closed parent stream.

可以在处于 空闲(idle) 或者 关闭(closed) 状态的流上发送PRIORITY帧，从而可以通过改变一个未使用的或者关闭的母流的优先级，来改变其从属流的优先级顺序。


> A PRIORITY frame with a length other than 5 octets MUST be treated as a stream error ([Section 5.4.2](https://httpwg.github.io/specs/rfc7540.html#StreamErrorHandler)) of type [FRAME\_SIZE\_ERROR](https://httpwg.github.io/specs/rfc7540.html#FRAME_SIZE_ERROR).

必须将不是5字节长度的PRIORITY帧当做一个类型为 [FRAME\_SIZE\_ERROR](https://httpwg.github.io/specs/rfc7540.html#FRAME_SIZE_ERROR) 的流错误 [5.4.2节](https://httpwg.github.io/specs/rfc7540.html#StreamErrorHandler)。
