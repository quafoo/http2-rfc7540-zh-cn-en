# The Flow-Control Window / 流量控制窗口
> Flow control in HTTP/2 is implemented using a window kept by each sender on every stream. The flow-control window is a simple integer value that indicates how many octets of data the sender is permitted to transmit; as such, its size is a measure of the buffering capacity of the receiver.

HTTP/2中的流量控制功能通过每个流上的发送端各自维持的窗口来实现。流量控制窗口是一个简单的整数值，指出了准许发送端传送的数据的字节数。这样，窗口值衡量了接收端的缓存能力。


> Two flow-control windows are applicable: the stream flow-control window and the connection flow-control window. The sender MUST NOT send a flow-controlled frame with a length that exceeds the space available in either of the flow-control windows advertised by the receiver. Frames with zero length with the END_STREAM flag set (that is, an empty [DATA](http://httpwg.org/specs/rfc7540.html#DATA) frame) MAY be sent if there is no available space in either flow-control window.

有两种流量控制窗口：流的流量控制窗口和连接的流量控制窗口。发送端不能发送受流量控制影响的、其长度超出接收端告知的这两种流量控制窗口可用空间的帧。即使这两种流量控制窗口都没有可用空间了，也可以发送长度为0、设置了END\_STREAM标志的帧(即，空的 [DATA](http://httpwg.org/specs/rfc7540.html#DATA) 帧)。


> For flow-control calculations, the 9-octet frame header is not counted.

9字节的帧首部不计入流量控制的计算。


> After sending a flow-controlled frame, the sender reduces the space available in both windows by the length of the transmitted frame.

发送了一个受流量控制影响的帧以后，发送端减少两个窗口的可用空间，减少量为已经发送的帧的长度大小。


> The receiver of a frame sends a WINDOW\_UPDATE frame as it consumes data and frees up space in flow-control windows. Separate WINDOW_UPDATE frames are sent for the stream- and connection-level flow-control windows.

当帧的接收端消耗了数据并释放了流量控制窗口的空间时，它发送一个WINDOW\_UPDATE帧。对于流级别和连接级别的流量控制窗口，需要分别发送WINDOW\_UPDATE帧。


> A sender that receives a WINDOW_UPDATE frame updates the corresponding window by the amount specified in the frame.

收到WINDOW\_UPDATE帧的发送端要更新相应的窗口，更新量已在WINDOW\_UPDATE帧里指定。


> A sender MUST NOT allow a flow-control window to exceed 2^31 - 1 octets. If a sender receives a WINDOW\_UPDATE that causes a flow-control window to exceed this maximum, it MUST terminate either the stream or the connection, as appropriate. For streams, the sender sends a [RST\_STREAM](http://httpwg.org/specs/rfc7540.html#RST_STREAM) with an error code of [FLOW\_CONTROL\_ERROR](http://httpwg.org/specs/rfc7540.html#FLOW_CONTROL_ERROR); for the connection, a [GOAWAY](http://httpwg.org/specs/rfc7540.html#GOAWAY) frame with an error code of [FLOW\_CONTROL\_ERROR](http://httpwg.org/specs/rfc7540.html#FLOW_CONTROL_ERROR) is sent.

发送端不能让流量控制窗口超出 2^31 - 1 字节。如果发送端收到一个WINDOW\_UPDATE帧使流量控制窗口超出该最大值，它必须终止相应的流或连接。对于流，发送端发送一个 [RST\_STREAM](http://httpwg.org/specs/rfc7540.html#RST_STREAM) 帧，错误码为 [FLOW\_CONTROL\_ERROR](http://httpwg.org/specs/rfc7540.html#FLOW_CONTROL_ERROR) ；对于连接，发送一个[GOAWAY](http://httpwg.org/specs/rfc7540.html#GOAWAY)帧，错误码为 [FLOW\_CONTROL\_ERROR](http://httpwg.org/specs/rfc7540.html#FLOW_CONTROL_ERROR) 。


> Flow-controlled frames from the sender and WINDOW_UPDATE frames from the receiver are completely asynchronous with respect to each other. This property allows a receiver to aggressively update the window size kept by the sender to prevent streams from stalling.

发送端发送的受流量控制影响的帧和来自于接收端的WINDOW\_UPDATE帧之间彼此完全异步。这个属性允许接收端剧烈地更新发送端维持的窗口大小，以防止流中止。