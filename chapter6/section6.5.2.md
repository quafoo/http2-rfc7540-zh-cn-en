# Defined SETTINGS Parameters / Defined SETTINGS Parameters
> The following parameters are defined:
> 
> * **SETTINGS\_HEADER\_TABLE_SIZE (0x1):** Allows the sender to inform the remote endpoint of the maximum size of the header compression table used to decode header blocks, in octets. The encoder can select any size equal to or less than this value by using signaling specific to the header compression format inside a header block (see [COMPRESSION]). The initial value is 4,096 octets.
> 
> * **SETTINGS\_ENABLE_PUSH (0x2):** This setting can be used to disable server push (Section 8.2). An endpoint MUST NOT send a PUSH\_PROMISE frame if it receives this parameter set to a value of 0. An endpoint that has both set this parameter to 0 and had it acknowledged MUST treat the receipt of a PUSH_PROMISE frame as a connection error (Section 5.4.1) of type PROTOCOL\_ERROR.
> 
> 	The initial value is 1, which indicates that server push is permitted. Any value other than 0 or 1 MUST be treated as a connection error (Section 5.4.1) of type PROTOCOL_ERROR.
> 
> * **SETTINGS\_MAX\_CONCURRENT_STREAMS (0x3):** Indicates the maximum number of concurrent streams that the sender will allow. This limit is directional: it applies to the number of streams that the sender permits the receiver to create. Initially, there is no limit to this value. It is recommended that this value be no smaller than 100, so as to not unnecessarily limit parallelism.
> 
> 	A value of 0 for SETTINGS\_MAX\_CONCURRENT_STREAMS SHOULD NOT be treated as special by endpoints. A zero value does prevent the creation of new streams; however, this can also happen for any limit that is exhausted with active streams. Servers SHOULD only set a zero value for short durations; if a server does not wish to accept requests, closing the connection is more appropriate.
> 
> * **SETTINGS\_INITIAL\_WINDOW_SIZE (0x4):** Indicates the sender's initial window size (in octets) for stream-level flow control. The initial value is 216-1 (65,535) octets.
> 
> 	This setting affects the window size of all streams (see Section 6.9.2).
> 
> 	Values above the maximum flow-control window size of 231-1 MUST be treated as a connection error (Section 5.4.1) of type FLOW\_CONTROL_ERROR.
> 
> * **SETTINGS\_MAX\_FRAME_SIZE (0x5):** Indicates the size of the largest frame payload that the sender is willing to receive, in octets.
> 
> 	The initial value is 2^14 (16,384) octets. The value advertised by an endpoint MUST be between this initial value and the maximum allowed frame size (2^24 - 1 or 16,777,215 octets), inclusive. Values outside this range MUST be treated as a connection error (Section 5.4.1) of type PROTOCOL_ERROR.
> 
> * **SETTINGS\_MAX\_HEADER\_LIST_SIZE (0x6):** This advisory setting informs a peer of the maximum size of header list that the sender is prepared to accept, in octets. The value is based on the uncompressed size of header fields, including the length of the name and value in octets plus an overhead of 32 octets for each header field.
> 
> 	For any given request, a lower limit than what is advertised MAY be enforced. The initial value of this setting is unlimited.

> An endpoint that receives a SETTINGS frame with any unknown or unsupported identifier MUST ignore that setting.

