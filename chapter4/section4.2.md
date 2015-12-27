# Frame Size / 帧大小
> The size of a frame payload is limited by the maximum size that a receiver advertises in the SETTINGS\_MAX\_FRAME\_SIZE setting. This setting can have any value between 2^14 (16,384) and 2^24-1 (16,777,215) octets, inclusive.

帧有效载荷的大小不能超过接收端通过SETTINGS\_MAX\_FRAME\_SIZE所告知的值。该值可以取2^14 (16,384)和2^24-1 (16,777,215)之间的任何值，包括2^24-1 (16,777,215)。

> All implementations MUST be capable of receiving and minimally processing frames up to 2^14 octets in length, plus the 9-octet frame header (Section 4.1). The size of the frame header is not included when describing frame sizes.

所有的实现都必须能接收，并且处理最小长度为2^14字节、外加9字节帧头部(4.1节)的帧。当描述帧大小时，不包括帧头部的大小。

> Note: Certain frame types, such as PING (Section 6.7), impose additional limits on the amount of payload data allowed.

注意：某些帧类型，如PING(6.7节)帧，对允许的有效载荷数据量强加了额外的限制。

> An endpoint MUST send an error code of FRAME\_SIZE\_ERROR if a frame exceeds the size defined in SETTINGS\_MAX\_FRAME\_SIZE, exceeds any limit defined for the frame type, or is too small to contain mandatory frame data. A frame size error in a frame that could alter the state of the entire connection MUST be treated as a connection error (Section 5.4.1); this includes any frame carrying a header block (Section 4.3) (that is, HEADERS, PUSH_PROMISE, and CONTINUATION), SETTINGS, and any frame with a stream identifier of 0.

如果一个帧超出了SETTINGS\_MAX\_FRAME\_SIZE定义的大小，或者超出了任何为该帧类型设定的限制，或者帧太小而无法包含强制性的帧数据，端点必须发送一个FRAME\_SIZE\_ERROR错误码。必须将可以改变整个连接状态的帧大小错误处理为连接错误(5.4.1节)，这包括所有携带首部块(4.3节)的帧(即，HEADERS，PUSH\_PROMISE和CONTINUATION)，SETTINGS帧，和所有流标识符为0的帧。

> Endpoints are not obligated to use all available space in a frame. Responsiveness can be improved by using frames that are smaller than the permitted maximum size. Sending large frames can result in delays in sending time-sensitive frames (such as RST\_STREAM, WINDOW\_UPDATE, or PRIORITY), which, if blocked by the transmission of a large frame, could affect performance.

端点不必用掉帧的所有可用空间。如果帧大小小于允许的最大值，可以改善响应性能。发送大的帧会导致时间敏感的帧(如，RST\_STREAM，WINDOW\_UPDATE，或PRIORITY)发送延迟，这些帧如果被大帧阻塞住了，会影响性能。