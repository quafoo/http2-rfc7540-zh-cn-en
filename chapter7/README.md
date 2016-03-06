# Error codes / 错误码
> Error codes are 32-bit fields that are used in [RST_STREAM](http://httpwg.org/specs/rfc7540.html#RST_STREAM) and [GOAWAY](http://httpwg.org/specs/rfc7540.html#GOAWAY) frames to convey the reasons for the stream or connection error.

错误码是 [RST\_STREAM](http://httpwg.org/specs/rfc7540.html#RST_STREAM) 帧和 [GOAWAY](http://httpwg.org/specs/rfc7540.html#GOAWAY) 帧中32-bit位的域，用于表达流或连接错误的原因。


> Error codes share a common code space. Some error codes apply only to either streams or the entire connection and have no defined semantics in the other context.

错误码共享一个公共的码空间。一些错误码要么只适用于流，要么只适用于整个连接，并且在其他的上下文中没有语义定义。


> The following error codes are defined:
> 
> * **NO_ERROR (0x0):** The associated condition is not a result of an error. For example, a [GOAWAY](http://httpwg.org/specs/rfc7540.html#GOAWAY) might include this code to indicate graceful shutdown of a connection.
> * **PROTOCOL_ERROR (0x1):** The endpoint detected an unspecific protocol error. This error is for use when a more specific error code is not available.
> * **INTERNAL_ERROR (0x2):** The endpoint encountered an unexpected internal error.
> * **FLOW\_CONTROL_ERROR (0x3):** The endpoint detected that its peer violated the flow-control protocol.
> * **SETTINGS_TIMEOUT (0x4):** The endpoint sent a [SETTINGS](http://httpwg.org/specs/rfc7540.html#SETTINGS) frame but did not receive a response in a timely manner. See [Section 6.5.3](http://httpwg.org/specs/rfc7540.html#SettingsSync) ("Settings Synchronization").
> * **STREAM_CLOSED (0x5):** The endpoint received a frame after a stream was half-closed.
> * **FRAME\_SIZE_ERROR (0x6):** The endpoint received a frame with an invalid size.
> * **REFUSED_STREAM (0x7):** The endpoint refused the stream prior to performing any application processing (see [Section 8.1.4](http://httpwg.org/specs/rfc7540.html#Reliability) for details).
> * **CANCEL (0x8):** Used by the endpoint to indicate that the stream is no longer needed.
> * **COMPRESSION_ERROR (0x9):** The endpoint is unable to maintain the header compression context for the connection.
> * **CONNECT_ERROR (0xa):** The connection established in response to a CONNECT request ([Section 8.3](http://httpwg.org/specs/rfc7540.html#CONNECT)) was reset or abnormally closed.
> * **ENHANCE\_YOUR_CALM (0xb):** The endpoint detected that its peer is exhibiting a behavior that might be generating excessive load.
> * **INADEQUATE\_SECURITY (0xc):** The underlying transport has properties that do not meet minimum security requirements (see [Section 9.2](http://httpwg.org/specs/rfc7540.html#TLSUsage)).
> * **HTTP\_1\_1\_REQUIRED (0xd):** The endpoint requires that HTTP/1.1 be used instead of HTTP/2.

定义了以下错误码：

* **NO_ERROR (0x0):** 相关情况不是错误。例如，[GOAWAY](http://httpwg.org/specs/rfc7540.html#GOAWAY) 帧可以包含该码，表示优雅地关闭连接。
* **PROTOCOL_ERROR (0x1):** 端点检测到一个不确定的协议错误。当没有更加具体的错误码可用时，可以使用该错误码。
* **INTERNAL_ERROR (0x2):** 端点遭遇到未知的内部错误。
* **FLOW\_CONTROL_ERROR (0x3):** 端点检测到其对端违反了流量控制协议。
* **SETTINGS_TIMEOUT (0x4):** 端点发送了一个 [SETTIGNS](http://httpwg.org/specs/rfc7540.html#SETTINGS) 帧，但是没有及时收到响应。参见 [6.5.3节](http://httpwg.org/specs/rfc7540.html#SettingsSync) ("同步设置")。
* **STREAM_CLOSED (0x5):** 流半关闭以后，端点收到一个帧。
* **FRAME\_SIZE_ERROR (0x6):** 端点收到了一个大小无效的帧。
* **REFUSED_STREAM (0x7):** 在执行任何处理之前，端点拒绝了流(细节参见 [8.4.1节](http://httpwg.org/specs/rfc7540.html#Reliability) )。
* **CANCEL (0x8):** 被端点用于表示不再需要该流了。
* **COMPRESSION_ERROR (0x9):** 端点不能为连接维持首部压缩上下文。
* **CONNECT_ERROR (0xa):** 响应CONNECT请求( [8.3节](http://httpwg.org/specs/rfc7540.html#CONNECT) )而建立的连接被重置，或者被非正常关闭。
* **ENHANCE\_YOUR_CALM (0xb):** 端点检测到对端正在展现出可能会产生极大负荷的行为。
* **INADEQUATE\_SECURITY (0xc):** 下层的传输层具有不满足最低安全要求( [9.2节](http://httpwg.org/specs/rfc7540.html#TLSUsage) )的属性。
* **HTTP\_1\_1\_REQUIRED (0xd):** 端点要求使用HTTP/1.1代替HTTP/2。


> Unknown or unsupported error codes MUST NOT trigger any special behavior. These MAY be treated by an implementation as being equivalent to [INTERNAL_ERROR](http://httpwg.org/specs/rfc7540.html#INTERNAL_ERROR).

未知的或者不支持的错误码不能触发任何特别的行为。实现可以将这些错误看做等价于[INTERNAL\_ERROR](http://httpwg.org/specs/rfc7540.html#INTERNAL_ERROR)。