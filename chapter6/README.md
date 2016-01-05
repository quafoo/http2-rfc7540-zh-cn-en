# Frame / 帧定义
> This specification defines a number of frame types, each identified by a unique 8-bit type code. Each frame type serves a distinct purpose in the establishment and management either of the connection as a whole or of individual streams.

该规范定义了若干帧类型，每种帧类型由唯一的8bit类型码标识。在建立和管理整个连接或者单独一个流时，每种帧类型都服务于特定的目的。

> The transmission of specific frame types can alter the state of a connection. If endpoints fail to maintain a synchronized view of the connection state, successful communication within the connection will no longer be possible. Therefore, it is important that endpoints have a shared comprehension of how the state is affected by the use any given frame.

传送特定的帧类型可以改变连接的状态。如果两端不能保持同步的连接状态，就不可能再成功地进行通信。因此，对给定的帧怎样影响连接的状态，两端的理解应该保持一致。