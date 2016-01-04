# Initial Flow-Control Window Size / Initial Flow-Control Window Size
> When an HTTP/2 connection is first established, new streams are created with an initial flow-control window size of 65,535 octets. The connection flow-control window is also 65,535 octets. Both endpoints can adjust the initial window size for new streams by including a value for SETTINGS\_INITIAL\_WINDOW\_SIZE in the SETTINGS frame that forms part of the connection preface. The connection flow-control window can only be changed using WINDOW\_UPDATE frames.

> Prior to receiving a SETTINGS frame that sets a value for SETTINGS\_INITIAL\_WINDOW\_SIZE, an endpoint can only use the default initial window size when sending flow-controlled frames. Similarly, the connection flow-control window is set to the default initial window size until a WINDOW_UPDATE frame is received.

> In addition to changing the flow-control window for streams that are not yet active, a SETTINGS frame can alter the initial flow-control window size for streams with active flow-control windows (that is, streams in the "open" or "half-closed (remote)" state). When the value of SETTINGS\_INITIAL\_WINDOW_SIZE changes, a receiver MUST adjust the size of all stream flow-control windows that it maintains by the difference between the new value and the old value.

> A change to SETTINGS\_INITIAL\_WINDOW\_SIZE can cause the available space in a flow-control window to become negative. A sender MUST track the negative flow-control window and MUST NOT send new flow-controlled frames until it receives WINDOW_UPDATE frames that cause the flow-control window to become positive.

> For example, if the client sends 60 KB immediately on connection establishment and the server sets the initial window size to be 16 KB, the client will recalculate the available flow-control window to be -44 KB on receipt of the SETTINGS frame. The client retains a negative flow-control window until WINDOW_UPDATE frames restore the window to being positive, after which the client can resume sending.

> A SETTINGS frame cannot alter the connection flow-control window.

> An endpoint MUST treat a change to SETTINGS\_INITIAL\_WINDOW\_SIZE that causes any flow-control window to exceed the maximum size as a connection error (Section 5.4.1) of type FLOW\_CONTROL\_ERROR.