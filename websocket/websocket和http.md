
The `WebSocket` object provides the API for creating and managing a **WebSocket connection** to a server, as well as for sending and receiving data on the connection.

With WebSocket API, it is possible to open a two-way interactive communication session between the user's browser and a server. And we can send messages to a server and receive event-driven responses without having to poll(轮询) the server for a reply.


### websocket 协议 和 http 协议

websocket 协议是 HTML 5 引入的一种新协议，可实现客户端和服务器的全双工通信。

在 websocket 之前，想在网页中进行消息即时通信，一般是采用轮询技术。

和 http 关系：

1. 都是应用层协议
2. 都是基于 tcp

和 http 区别：

1. http 是单向的，客户端主动发送给服务端。websocket 是双向的
2. websocket 必须握手才可以建立连接。注意，在建立握手时数据通过 http 传输，在建立之后的真正传输过程不再需要 http

