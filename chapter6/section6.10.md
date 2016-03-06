# CONTINUATION / CONTINUATION帧
> The CONTINUATION frame (type=0x9) is used to continue a sequence of header block fragments ([Section 4.3](http://httpwg.org/specs/rfc7540.html#HeaderBlock)). Any number of CONTINUATION frames can be sent, as long as the preceding frame is on the same stream and is a [HEADERS](http://httpwg.org/specs/rfc7540.html#HEADERS), [PUSH\_PROMISE](http://httpwg.org/specs/rfc7540.html#PUSH_PROMISE), or CONTINUATION frame without the END_HEADERS flag set.

> ```
> +---------------------------------------------------------------+
> |                   Header Block Fragment (*)                 ...
> +---------------------------------------------------------------+
> 				Figure 15: CONTINUATION Frame Payload
> ```

CONTINUATION帧(type=0x9)用于继续传送首部块片段序列( [4.3节](http://httpwg.org/specs/rfc7540.html#HeaderBlock) )。只要前面的帧在同一个流上，而且是一个没有设置END\_HEADERS标志的 [HEADERS](http://httpwg.org/specs/rfc7540.html#HEADERS) 帧，[PUSH\_PROMISE](http://httpwg.org/specs/rfc7540.html#PUSH_PROMISE)帧，或者CONTINUATION帧，就可以发送任意数量的CONTINUATION帧。

```
+---------------------------------------------------------------+
|                   Header Block Fragment (*)                 ...
+---------------------------------------------------------------+
 				     图 15: CONTINUATION帧负载
```



> The CONTINUATION frame payload contains a header block fragment ([Section 4.3](http://httpwg.org/specs/rfc7540.html#HeaderBlock)).

CONTINUATION帧的负载包含一个首部块片段( [4.3节](http://httpwg.org/specs/rfc7540.html#HeaderBlock) )。


> The CONTINUATION frame defines the following flag:
> 
> * **END_HEADERS (0x4):** When set, bit 2 indicates that this frame ends a header block ([Section 4.3](http://httpwg.org/specs/rfc7540.html#HeaderBlock)).
> 
> 	If the END\_HEADERS bit is not set, this frame MUST be followed by another CONTINUATION frame. A receiver MUST treat the receipt of any other type of frame or a frame on a different stream as a connection error ([Section 5.4.1](http://httpwg.org/specs/rfc7540.html#ConnectionErrorHandler)) of type [PROTOCOL\_ERROR](http://httpwg.org/specs/rfc7540.html#PROTOCOL_ERROR).

CONTINUATION帧定义了如下标志：

* **END_HEADERS (0x4):** 当设置了该标志，第2个bit位表示该帧是一个首部块的结束( [4.3节](http://httpwg.org/specs/rfc7540.html#HeaderBlock) )。

  如果没有设置 END_HEADERS bit位，该帧的后面必须跟有其他的CONTINUATION帧。如果接收端收到了任何其他类型的帧，或者另外一条流上的帧，必须将其当做类型为 [PROTOCOL\_ERROR](http://httpwg.org/specs/rfc7540.html#PROTOCOL_ERROR) 的连接错误( [5.4.1节](http://httpwg.org/specs/rfc7540.html#ConnectionErrorHandler) )。


> The CONTINUATION frame changes the connection state as defined in [Section 4.3](http://httpwg.org/specs/rfc7540.html#HeaderBlock).

CONTINUATION帧改变了 [4.3节](http://httpwg.org/specs/rfc7540.html#HeaderBlock) 定义的连接状态。


> CONTINUATION frames MUST be associated with a stream. If a CONTINUATION frame is received whose stream identifier field is 0x0, the recipient MUST respond with a connection error ([Section 5.4.1](http://httpwg.org/specs/rfc7540.html#ConnectionErrorHandler)) of type PROTOCOL_ERROR.

CONTINUATION帧必须和一个流相关联。如果收到了流标识符域的值为0x0的CONTINUATION帧，接收端必须响应一个类型为PROTOCOL\_ERROR的连接错误( [5.4.1节](http://httpwg.org/specs/rfc7540.html#ConnectionErrorHandler) )。


> A CONTINUATION frame MUST be preceded by a [HEADERS](http://httpwg.org/specs/rfc7540.html#HEADERS), [PUSH\_PROMISE](http://httpwg.org/specs/rfc7540.html#PUSH_PROMISE) or CONTINUATION frame without the END\_HEADERS flag set. A recipient that observes violation of this rule MUST respond with a connection error ([Section 5.4.1](http://httpwg.org/specs/rfc7540.html#ConnectionErrorHandler)) of type [PROTOCOL\_ERROR](http://httpwg.org/specs/rfc7540.html#PROTOCOL_ERROR).

CONTINUATION帧之前必须是一个没有设置END\_HEADERS标志的 [HEADERS](http://httpwg.org/specs/rfc7540.html#HEADERS) 帧，[PUSH\_PROMISE](http://httpwg.org/specs/rfc7540.html#PUSH_PROMISE) 帧，或者CONTINUATION帧。如果接收端发现违反该规则了，必须响应一个类型为 [PROTOCOL\_ERROR](http://httpwg.org/specs/rfc7540.html#PROTOCOL_ERROR) 的连接错误( [5.4.1节](http://httpwg.org/specs/rfc7540.html#ConnectionErrorHandler) )。