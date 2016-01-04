# PRIORITY / PRIORITY帧
> The PRIORITY frame (type=0x2) specifies the sender-advised priority of a stream (Section 5.3). It can be sent in any stream state, including idle or closed streams.
>
> ```
> +-+-------------------------------------------------------------+
> |E|                  Stream Dependency (31)                     |
> +-+-------------+-----------------------------------------------+
> |   Weight (8)  |
> +-+-------------+
> 				Figure 8: PRIORITY Frame Payload
> ```

> The payload of a PRIORITY frame contains the following fields:
> 
> * **E:** A single-bit flag indicating that the stream dependency is exclusive (see Section 5.3).
> * **Stream Dependency:** A 31-bit stream identifier for the stream that this stream depends on (see Section 5.3).
> * **Weight:** An unsigned 8-bit integer representing a priority weight for the stream (see Section 5.3). Add one to the value to obtain a weight between 1 and 256.


> The PRIORITY frame does not define any flags.

> The PRIORITY frame always identifies a stream. If a PRIORITY frame is received with a stream identifier of 0x0, the recipient MUST respond with a connection error (Section 5.4.1) of type PROTOCOL_ERROR.

> The PRIORITY frame can be sent on a stream in any state, though it cannot be sent between consecutive frames that comprise a single header block (Section 4.3). Note that this frame could arrive after processing or frame sending has completed, which would cause it to have no effect on the identified stream. For a stream that is in the "half-closed (remote)" or "closed" state, this frame can only affect processing of the identified stream and its dependent streams; it does not affect frame transmission on that stream.

> The PRIORITY frame can be sent for a stream in the "idle" or "closed" state. This allows for the reprioritization of a group of dependent streams by altering the priority of an unused or closed parent stream.

> A PRIORITY frame with a length other than 5 octets MUST be treated as a stream error (Section 5.4.2) of type FRAME_SIZE_ERROR.
