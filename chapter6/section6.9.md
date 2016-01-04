# WINDOW\_UPDATE / WINDOW_UPDATE帧
> The WINDOW_UPDATE frame (type=0x8) is used to implement flow control; see Section 5.2 for an overview.

> Flow control operates at two levels: on each individual stream and on the entire connection.

> Both types of flow control are hop by hop, that is, only between the two endpoints. Intermediaries do not forward WINDOW_UPDATE frames between dependent connections. However, throttling of data transfer by any receiver can indirectly cause the propagation of flow-control information toward the original sender.

> Flow control only applies to frames that are identified as being subject to flow control. Of the frame types defined in this document, this includes only DATA frames. Frames that are exempt from flow control MUST be accepted and processed, unless the receiver is unable to assign resources to handling the frame. A receiver MAY respond with a stream error (Section 5.4.2) or connection error (Section 5.4.1) of type FLOW_CONTROL_ERROR if it is unable to accept a frame.
> 
> ```
> +-+-------------------------------------------------------------+
> |R|              Window Size Increment (31)                     |
> +-+-------------------------------------------------------------+
> 				Figure 14: WINDOW_UPDATE Payload Format
> ```

> The payload of a WINDOW_UPDATE frame is one reserved bit plus an unsigned 31-bit integer indicating the number of octets that the sender can transmit in addition to the existing flow-control window. The legal range for the increment to the flow-control window is 1 to 231-1 (2,147,483,647) octets.

> The WINDOW_UPDATE frame does not define any flags.

> The WINDOW_UPDATE frame can be specific to a stream or to the entire connection. In the former case, the frame's stream identifier indicates the affected stream; in the latter, the value "0" indicates that the entire connection is the subject of the frame.

> A receiver MUST treat the receipt of a WINDOW_UPDATE frame with an flow-control window increment of 0 as a stream error (Section 5.4.2) of type PROTOCOL\_ERROR; errors on the connection flow-control window MUST be treated as a connection error (Section 5.4.1).

> WINDOW\_UPDATE can be sent by a peer that has sent a frame bearing the END\_STREAM flag. This means that a receiver could receive a WINDOW_UPDATE frame on a "half-closed (remote)" or "closed" stream. A receiver MUST NOT treat this as an error (see Section 5.1).

> A receiver that receives a flow-controlled frame MUST always account for its contribution against the connection flow-control window, unless the receiver treats this as a connection error (Section 5.4.1). This is necessary even if the frame is in error. The sender counts the frame toward the flow-control window, but if the receiver does not, the flow-control window at the sender and receiver can become different.

> A WINDOW\_UPDATE frame with a length other than 4 octets MUST be treated as a connection error (Section 5.4.1) of type FRAME\_SIZE_ERROR.

