Node is designed to provide an "easy way to create scalable web servers."

Node runs on Chrome's high-speed V8 engine and carries a library of fast, robust asynchronous network I/O components. 这个异步网络 I/O 组件库足以实现采用任何 TCP/UDP 协议的、任意类型的服务器，无论是 DNS 服务器，还是采用 HTTP、IRC、FTP 服务器。

Node is primarily used to build "high-performance, highly scalable server and client applications".



### 一、what's in it

首先，**Node 把异步 I/O 和服务器端 JavaScript 巧妙地组合在一起，聪明地运用了强大的 JavaScript 匿名函数和单线程执行的事件驱动架构，是专为网络应用的极高扩展性而设计的。**



### 二、why to use

#### 2.1 JavaScript

我们都知道，JavaScript 由于无处不在的浏览器而非常流行，并且它随着发展实现了许多现代高级语言的概念，比其他任何语言都不逊色。

JavaScript 的一个短板是全局对象。所有的顶级变量都被扔给一个全局对象，这在混用多个模块时会导致难以预料的混乱。但 Node 使用了 CommonJS 模块系统，这意味着模块的局部变量即使看起来像全局变量，实际上也是局部变量，这种模式避免了全局对象的问题。

另外，在 Web 应用服务器端和客户端使用同样的编程语言是人们由来已久的梦想。这个梦想可以追溯到早期的 Java 时代，那时 Applet 是用 Java 编写的服务器应用的前端，而对 JavaScript 的最初设想是将其作为 Applet 的一种轻量级脚本语言。但世事难料，到头来 JavaScript 取代 Java 成为在浏览器中使用的唯一语言。而有了 Node，在客户端和服务器端使用相同编程语言的梦想终于有望实现了。

语言在前后端通用有如下几个优势:

- 代码可以容易地在服务器端和客户端间迁移;
- 服务器端和客户端可以方便地使用相同的数据格式(JSON);
- 服务器端和客户端可以使用相同的开发工具;
- 服务器端和客户端可以使用相同的测试或质量评估工具; 
- ...

#### 2.2 架构问题：线程，还是异步事件驱动

首先，Node 不是 `traditional thread-based concurrency model`，而是 `event-driven asynchronous systems`. 

传统应用服务器，使用阻塞 I/O 和线程来实现并发，**阻塞 I/O 会导致线程等待**，如果所有线程都处于繁忙状态，客户端请求就会被丢弃，这当然会造成线程资源浪费。因此，当应用服务器处理请求时，需要等待 I/O 执行结束后才能继续处理。



而相比多线程系统，Node 有一个非阻塞的异步事件驱动 I/O 的单独执行环境，具体来说，其 I/O 调用会转换为请求处理函数。


编程模型更简单，内存占用量低，吞吐量高。因为 Node 通过内部单线程高效率地维护事件循环队列来实现，没有多线程的资源占用和上下文切换。

面对大规模的 http 请求，Node 凭借高速事件驱动 I/O 和 高速 V8 JavaScript 引擎搞定一切。



#### 2.3 性能和利用率

#### 2.4 服务器利用率、成本



### 四、多核/多进程模式

单核性能出色的 Node.js 也可以运行多个 Node.js 进行再通过某些通信机制来协调各项任务。



