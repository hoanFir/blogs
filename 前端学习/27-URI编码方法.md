
Global 对象的 encodeURI() 和 encodeURIComponent() 方法，可以对 Uniform Resource Identifiers，通用资源标识符，进行编码，以便发送给浏览器。

由于有效的 URI 中不能包含某些字符，例如空格，通过使用 encodeURI() 和 encodeURIComponent() 方法可以对 URI 无效字符用 UTF-8 编码替换，从而让浏览器能够接受和理解。

### 一、encodeURI

主要用于整个 URI。

该方法不会对本身属于 URI 对特殊字符进行编码，如冒号、正斜杠、问号和井字号。

### 二、encodeURIComponent

主要用于对 URI 中某一个段进行编码。

该方法对发现对任何非标准字符进行编码。
