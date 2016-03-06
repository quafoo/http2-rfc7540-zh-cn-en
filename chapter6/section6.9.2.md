# Initial Flow-Control Window Size / 初始化流量控制窗口大小
> When an HTTP/2 connection is first established, new streams are created with an initial flow-control window size of 65,535 octets. The connection flow-control window is also 65,535 octets. Both endpoints can adjust the initial window size for new streams by including a value for [SETTINGS\_INITIAL\_WINDOW\_SIZE](http://httpwg.org/specs/rfc7540.html#SETTINGS_INITIAL_WINDOW_SIZE) in the [SETTINGS](http://httpwg.org/specs/rfc7540.html#SETTINGS) frame that forms part of the connection preface. The connection flow-control window can only be changed using WINDOW\_UPDATE frames.

当HTTP/2连接首次被建立时，新创建流的初始流量控制窗口大小为65，535字节。连接的流量控制窗口也是65，535字节。两端都可以通过组成连接序幕的 [SETTING](http://httpwg.org/specs/rfc7540.html#SETTINGS) 帧里的 [SETTINGS\_INITIAL\_WINDOW\_SIZE](http://httpwg.org/specs/rfc7540.html#SETTINGS_INITIAL_WINDOW_SIZE) 来调整新流的初始流量控制窗口的大小。连接的流量控制窗口只能通过WINDOW\_UPDATE帧来改变。


> Prior to receiving a [SETTINGS](http://httpwg.org/specs/rfc7540.html#SETTINGS) frame that sets a value for [SETTINGS\_INITIAL\_WINDOW\_SIZE](http://httpwg.org/specs/rfc7540.html#SETTINGS_INITIAL_WINDOW_SIZE), an endpoint can only use the default initial window size when sending flow-controlled frames. Similarly, the connection flow-control window is set to the default initial window size until a WINDOW_UPDATE frame is received.

在收到设置了 [SETTINGS\_INITIAL\_WINDOW\_SIZE](http://httpwg.org/specs/rfc7540.html#SETTINGS_INITIAL_WINDOW_SIZE) 的 [SETTIGNS](http://httpwg.org/specs/rfc7540.html#SETTINGS) 帧之前，当一端发送受流量控制影响的帧时，只能使用默认的初始窗口大小。相似地，直到收到了 WINDOW_UPDATE 帧之前，连接的流量控制窗口都设置为默认的初始窗口大小。


> In addition to changing the flow-control window for streams that are not yet active, a [SETTINGS](http://httpwg.org/specs/rfc7540.html#SETTINGS) frame can alter the initial flow-control window size for streams with active flow-control windows (that is, streams in the "open" or "half-closed (remote)" state). When the value of [SETTINGS\_INITIAL\_WINDOW_SIZE](http://httpwg.org/specs/rfc7540.html#SETTINGS_INITIAL_WINDOW_SIZE) changes, a receiver MUST adjust the size of all stream flow-control windows that it maintains by the difference between the new value and the old value.

除了改变还未激活的流的流量控制窗口，[SETTIGNS](http://httpwg.org/specs/rfc7540.html#SETTINGS) 帧还可以用激活的流量控制窗口改变流(即，处于 打开(open) 或者 半关闭(远端)(half-closed (remote)) 状态的流)的初始流量控制窗口的大小。当 [SETTINGS\_INITIAL\_WINDOW_SIZE](http://httpwg.org/specs/rfc7540.html#SETTINGS_INITIAL_WINDOW_SIZE) 的值变化了，接收端必须调整它所维护的所有流的流量控制窗口的值。


> A change to [SETTINGS\_INITIAL\_WINDOW\_SIZE](http://httpwg.org/specs/rfc7540.html#SETTINGS_INITIAL_WINDOW_SIZE) can cause the available space in a flow-control window to become negative. A sender MUST track the negative flow-control window and MUST NOT send new flow-controlled frames until it receives WINDOW_UPDATE frames that cause the flow-control window to become positive.

改变 [SETTINGS\_INITIAL\_WINDOW\_SIZE](http://httpwg.org/specs/rfc7540.html#SETTINGS_INITIAL_WINDOW_SIZE) 可以引发流量控制窗口的可用空间变成负值。发送端必须追踪负的流量控制窗口，并且直到它收到了使流量控制窗口变成正值的 WINDOW_UPDATE 帧，才能发送新的受流量控制影响的帧。


> For example, if the client sends 60 KB immediately on connection establishment and the server sets the initial window size to be 16 KB, the client will recalculate the available flow-control window to be -44 KB on receipt of the [SETTINGS](http://httpwg.org/specs/rfc7540.html#SETTINGS) frame. The client retains a negative flow-control window until WINDOW_UPDATE frames restore the window to being positive, after which the client can resume sending.

例如，如果连接一建立客户端就立即发送60KB的数据，而服务端却将初始窗口大小设置为16KB，那么客户端一收到 [SETTINGS](http://httpwg.org/specs/rfc7540.html#SETTINGS) 帧，就会将可用的流量控制窗口重新计算为-44KB。客户端保持负的流量控制窗口，直到WINDOW_UPDATE帧将窗口值恢复为正值，这之后，客户端才可以继续发送数据。


> A [SETTINGS](http://httpwg.org/specs/rfc7540.html#SETTINGS) frame cannot alter the connection flow-control window.

[SETTINGS](http://httpwg.org/specs/rfc7540.html#SETTINGS) 帧不能改变连接的流量控制窗口值。


> An endpoint MUST treat a change to [SETTINGS\_INITIAL\_WINDOW\_SIZE](http://httpwg.org/specs/rfc7540.html#SETTINGS_INITIAL_WINDOW_SIZE) that causes any flow-control window to exceed the maximum size as a connection error ([Section 5.4.1](http://httpwg.org/specs/rfc7540.html#ConnectionErrorHandler)) of type [FLOW\_CONTROL\_ERROR](http://httpwg.org/specs/rfc7540.html#FLOW_CONTROL_ERROR).

如果改变 [SETTINGS\_INITIAL\_WINDOW\_SIZE](http://httpwg.org/specs/rfc7540.html#SETTINGS_INITIAL_WINDOW_SIZE) 导致流量控制窗口超出了最大值，一端必须将其当做类型为 [FLOW\_CONTROL\_ERROR](http://httpwg.org/specs/rfc7540.html#FLOW_CONTROL_ERROR) 的连接错误( [5.4.1节](http://httpwg.org/specs/rfc7540.html#ConnectionErrorHandler) )。