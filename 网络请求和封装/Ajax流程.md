
Ajax，Asynchronous JavaScript And XML.

大致流程：

1. 创建 XMLHTTPRequest 对象，`var xhr = new XMLHttpRequest()`

2. 创建一个新的 http 请求，`xhr.open("get", url, true)`，open 第三个参数 true 为异步，false 为同步

3. 发送 http 请求

4. 设置响应 http 请求状态变化的函数

5. 获取异步请求返回的数据

状态：

- 0: 请求未初始化

- 1: 服务器连接已建立

- 2: 请求已接收

- 3: 请求处理中

- 4: 请求已完成，且响应已就绪



