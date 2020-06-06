
ECMAScript 中用于访问字符串中特定字符的方法是：charAt() 和 charCodeAt()。

### 一、charAt

该方法以**单字符字符串**（ECMAScript 没有字符类型）的形式，返回给定位置的那个字符。

```javascript

var stringValue = "hello world";

stringValue.charAt(1); //"e"

```

### 二、charCodeAt

返回给定位置的字符编码，而非字符。


```javascript

var stringValue = "hello world";

stringValue.charCodeAt(1); //"101" e的字符编码

```





