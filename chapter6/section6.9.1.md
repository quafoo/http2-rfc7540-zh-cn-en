# The Flow-Control Window / The Flow-Control Window
> Flow control in HTTP/2 is implemented using a window kept by each sender on every stream. The flow-control window is a simple integer value that indicates how many octets of data the sender is permitted to transmit; as such, its size is a measure of the buffering capacity of the receiver.

> Two flow-control windows are applicable: the stream flow-control window and the connection flow-control window. The sender MUST NOT send a flow-controlled frame with a length that exceeds the space available in either of the flow-control windows advertised by the receiver. Frames with zero length with the END_STREAM flag set (that is, an empty DATA frame) MAY be sent if there is no available space in either flow-control window.

> For flow-control calculations, the 9-octet frame header is not counted.

> After sending a flow-controlled frame, the sender reduces the space available in both windows by the length of the transmitted frame.

> The receiver of a frame sends a WINDOW\_UPDATE frame as it consumes data and frees up space in flow-control windows. Separate WINDOW_UPDATE frames are sent for the stream- and connection-level flow-control windows.

> A sender that receives a WINDOW_UPDATE frame updates the corresponding window by the amount specified in the frame.

> A sender MUST NOT allow a flow-control window to exceed 231-1 octets. If a sender receives a WINDOW\_UPDATE that causes a flow-control window to exceed this maximum, it MUST terminate either the stream or the connection, as appropriate. For streams, the sender sends a RST\_STREAM with an error code of FLOW\_CONTROL\_ERROR; for the connection, a GOAWAY frame with an error code of FLOW\_CONTROL_ERROR is sent.

> Flow-controlled frames from the sender and WINDOW_UPDATE frames from the receiver are completely asynchronous with respect to each other. This property allows a receiver to aggressively update the window size kept by the sender to prevent streams from stalling.

