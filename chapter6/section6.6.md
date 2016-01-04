# PUSH_PROMISE / PUSH_PROMISE帧
> The PUSH\_PROMISE frame (type=0x5) is used to notify the peer endpoint in advance of streams the sender intends to initiate. The PUSH\_PROMISE frame includes the unsigned 31-bit identifier of the stream the endpoint plans to create along with a set of headers that provide additional context for the stream. Section 8.2 contains a thorough description of the use of PUSH_PROMISE frames.
> 
> ``` 
> +---------------+
> |Pad Length? (8)|
> +-+-------------+-----------------------------------------------+
> |R|                  Promised Stream ID (31)                    |
> +-+-----------------------------+-------------------------------+
> |                   Header Block Fragment (*)                 ...
> +---------------------------------------------------------------+
> |                           Padding (*)                       ...
> +---------------------------------------------------------------+
> 					Figure 11: PUSH_PROMISE Payload Format
> ```



> The PUSH_PROMISE frame payload has the following fields:
> 
> * **Pad Length:** An 8-bit field containing the length of the frame padding in units of octets. This field is only present if the PADDED flag is set.
> * **R:** A single reserved bit.
> * **Promised Stream ID:** An unsigned 31-bit integer that identifies the stream that is reserved by the PUSH_PROMISE. The promised stream identifier MUST be a valid choice for the next stream sent by the sender (see "new stream identifier" in Section 5.1.1).
> * **Header Block Fragment:** A header block fragment (Section 4.3) containing request header fields.
> * **Padding:**	Padding octets.


> The PUSH\_PROMISE frame defines the following flags:
> 
> * **END_HEADERS (0x4):** When set, bit 2 indicates that this frame contains an entire header block (Section 4.3) and is not followed by any CONTINUATION frames.
> 
> 	A PUSH\_PROMISE frame without the END\_HEADERS flag set MUST be followed by a CONTINUATION frame for the same stream. A receiver MUST treat the receipt of any other type of frame or a frame on a different stream as a connection error (Section 5.4.1) of type PROTOCOL_ERROR.
> 
> * **PADDED (0x8):** When set, bit 3 indicates that the Pad Length field and any padding that it describes are present.


> PUSH\_PROMISE frames MUST only be sent on a peer-initiated stream that is in either the "open" or "half-closed (remote)" state. The stream identifier of a PUSH\_PROMISE frame indicates the stream it is associated with. If the stream identifier field specifies the value 0x0, a recipient MUST respond with a connection error (Section 5.4.1) of type PROTOCOL_ERROR.

> Promised streams are not required to be used in the order they are promised. The PUSH_PROMISE only reserves stream identifiers for later use.

> PUSH\_PROMISE MUST NOT be sent if the SETTINGS\_ENABLE\_PUSH setting of the peer endpoint is set to 0. An endpoint that has set this setting and has received acknowledgement MUST treat the receipt of a PUSH\_PROMISE frame as a connection error (Section 5.4.1) of type PROTOCOL_ERROR.

> Recipients of PUSH\_PROMISE frames can choose to reject promised streams by returning a RST\_STREAM referencing the promised stream identifier back to the sender of the PUSH_PROMISE.

> A PUSH\_PROMISE frame modifies the connection state in two ways. First, the inclusion of a header block (Section 4.3) potentially modifies the state maintained for header compression. Second, PUSH\_PROMISE also reserves a stream for later use, causing the promised stream to enter the "reserved" state. A sender MUST NOT send a PUSH_PROMISE on a stream unless that stream is either "open" or "half-closed (remote)"; the sender MUST ensure that the promised stream is a valid choice for a new stream identifier (Section 5.1.1) (that is, the promised stream MUST be in the "idle" state).

> Since PUSH\_PROMISE reserves a stream, ignoring a PUSH\_PROMISE frame causes the stream state to become indeterminate. A receiver MUST treat the receipt of a PUSH\_PROMISE on a stream that is neither "open" nor "half-closed (local)" as a connection error (Section 5.4.1) of type PROTOCOL\_ERROR. However, an endpoint that has sent RST\_STREAM on the associated stream MUST handle PUSH_PROMISE frames that might have been created before the RST\_STREAM frame is received and processed.

> A receiver MUST treat the receipt of a PUSH\_PROMISE that promises an illegal stream identifier (Section 5.1.1) as a connection error (Section 5.4.1) of type PROTOCOL\_ERROR. Note that an illegal stream identifier is an identifier for a stream that is not currently in the "idle" state.

> The PUSH\_PROMISE frame can include padding. Padding fields and flags are identical to those defined for DATA frames (Section 6.1).