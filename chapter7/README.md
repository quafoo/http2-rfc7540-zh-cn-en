# Error codes / 错误码
> Error codes are 32-bit fields that are used in RST_STREAM and GOAWAY frames to convey the reasons for the stream or connection error.

> Error codes share a common code space. Some error codes apply only to either streams or the entire connection and have no defined semantics in the other context.

> The following error codes are defined:
> 
> * **NO_ERROR (0x0):** The associated condition is not a result of an error. For example, a GOAWAY might include this code to indicate graceful shutdown of a connection.
> * **PROTOCOL_ERROR (0x1):** The endpoint detected an unspecific protocol error. This error is for use when a more specific error code is not available.
> * **INTERNAL_ERROR (0x2):** The endpoint encountered an unexpected internal error.
> * **FLOW\_CONTROL_ERROR (0x3):** The endpoint detected that its peer violated the flow-control protocol.
> * **SETTINGS_TIMEOUT (0x4):** The endpoint sent a SETTINGS frame but did not receive a response in a timely manner. See Section 6.5.3 ("Settings Synchronization").
> * **STREAM_CLOSED (0x5):** The endpoint received a frame after a stream was half-closed.
> * **FRAME\_SIZE_ERROR (0x6):** The endpoint received a frame with an invalid size.
> * **REFUSED_STREAM (0x7):** The endpoint refused the stream prior to performing any application processing (see Section 8.1.4 for details).
> * **CANCEL (0x8):** Used by the endpoint to indicate that the stream is no longer needed.
> * **COMPRESSION_ERROR (0x9):** The endpoint is unable to maintain the header compression context for the connection.
> * **CONNECT_ERROR (0xa):** The connection established in response to a CONNECT request (Section 8.3) was reset or abnormally closed.
> * **ENHANCE\_YOUR_CALM (0xb):** The endpoint detected that its peer is exhibiting a behavior that might be generating excessive load.
> * **INADEQUATE\_SECURITY (0xc):** The underlying transport has properties that do not meet minimum security requirements (see Section 9.2).
> * **HTTP\_1\_1\_REQUIRED (0xd):** The endpoint requires that HTTP/1.1 be used instead of HTTP/2.

> Unknown or unsupported error codes MUST NOT trigger any special behavior. These MAY be treated by an implementation as being equivalent to INTERNAL_ERROR.

