# RST_STREAM / RST_STREAM帧
> The RST\_STREAM frame (type=0x3) allows for immediate termination of a stream. RST_STREAM is sent to request cancellation of a stream or to indicate that an error condition has occurred.
> 
> ```
> +---------------------------------------------------------------+
> |                        Error Code (32)                        |
> +---------------------------------------------------------------+
> 				Figure 9: RST_STREAM Frame Payload
> ```


> The RST_STREAM frame contains a single unsigned, 32-bit integer identifying the error code (Section 7). The error code indicates why the stream is being terminated.

> The RST_STREAM frame does not define any flags.

> The RST\_STREAM frame fully terminates the referenced stream and causes it to enter the "closed" state. After receiving a RST\_STREAM on a stream, the receiver MUST NOT send additional frames for that stream, with the exception of PRIORITY. However, after sending the RST\_STREAM, the sending endpoint MUST be prepared to receive and process additional frames sent on the stream that might have been sent by the peer prior to the arrival of the RST_STREAM.

> RST\_STREAM frames MUST be associated with a stream. If a RST\_STREAM frame is received with a stream identifier of 0x0, the recipient MUST treat this as a connection error (Section 5.4.1) of type PROTOCOL_ERROR.

> RST\_STREAM frames MUST NOT be sent for a stream in the "idle" state. If a RST\_STREAM frame identifying an idle stream is received, the recipient MUST treat this as a connection error (Section 5.4.1) of type PROTOCOL_ERROR.

> A RST\_STREAM frame with a length other than 4 octets MUST be treated as a connection error (Section 5.4.1) of type FRAME\_SIZE_ERROR.


