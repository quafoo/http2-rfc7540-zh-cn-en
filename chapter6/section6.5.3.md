# Settings Synchronization / 设置同步
> Most values in SETTINGS benefit from or require an understanding of when the peer has received and applied the changed parameter values. In order to provide such synchronization timepoints, the recipient of a SETTINGS frame in which the ACK flag is not set MUST apply the updated parameters as soon as possible upon receipt.

SETTINGS帧里的大部分值都受益于或者要求理解对端什么时候收到并且应用了已改变的参数值。为了提供这样的同步时间点，接收方必须一收到没有设置ACK标识的SETTINGS帧就尽快应用更新后的参数值。


> The values in the SETTINGS frame MUST be processed in the order they appear, with no other frame processing between values. Unsupported parameters MUST be ignored. Once all values have been processed, the recipient MUST immediately emit a SETTINGS frame with the ACK flag set. Upon receiving a SETTINGS frame with the ACK flag set, the sender of the altered parameters can rely on the setting having been applied.

SETTINGS帧里的值必须以其出现的顺序进行处理，而不能在值之间加入对其它帧的处理。不支持的参数必须被忽略。一旦所有的值都被处理完毕，接收方必须立即发送一个设置了ACK标识的SETTIGNS帧。一收到设置了ACK标识的SETTIGNS帧，已改变的参数的发送方就可以信赖已被应用的这些设置。


> If the sender of a SETTINGS frame does not receive an acknowledgement within a reasonable amount of time, it MAY issue a connection error ([Section 5.4.1](http://httpwg.org/specs/rfc7540.html#ConnectionErrorHandler)) of type [SETTINGS_TIMEOUT](http://httpwg.org/specs/rfc7540.html#SETTINGS_TIMEOUT).

如果SETTINGS帧的发送方在一个合理的时间内没有收到确认，它可以发送一个类型为 [SETTINGS_TIMEOUT](http://httpwg.org/specs/rfc7540.html#SETTINGS_TIMEOUT) 的连接错误( [5.4.1节](http://httpwg.org/specs/rfc7540.html#ConnectionErrorHandler) )。