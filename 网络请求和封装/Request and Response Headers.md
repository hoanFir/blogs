🐾 Request and Response Headers

🕘 2019.10.31 由 hoanfirst 编辑

**HTTP headers let the client and the server pass additional information with an HTTP request or response**.

---

Headers can be grouped according to their contexts: General headers, Request headers, Response headers, Entity headers.

- **General headers**: a general header is an http header that can be used in *both request and response message but doesn't apply to the content itself*. Depending on the context they are used in, general headers are either request or response headers. However, they are not entity headers.

The most common general headers:

key|value|description|
--|--|--|
Date|Thu, 31 Oct 2019 06:23:45 GMT|...|
Cache-Control|max-age=0|...|
Connection|keep-alive|...|

- **Request headers**：a request header is an http header that can be used in an http request, and that *doesn't relate to the content of the message*. Request headers like Accept, Accept-\*, or if-\* allow to perform conditional requests; other like Cookie, User-Agent or Referer precise the context so that the server can tailor the answer(量身定制答案).

key|value|description|
--|--|--|
Cookie|...|...|
User-Agent|Mozilla/5.0|...|
Referer|https://developer.mozilla.org/testpage.html|...|
Host|developer.mozilla.org|...|
Accept|application/json, text/plain, */*|...|
Accept-Language|zh-CN,zh;q=0.9|...|
Upgrade-Insecure-Request|1|...|
If-Modified-Since|Mon, 18 Jul 2016 02:36:04 GMT|...|
If-None-Match|"c561c68d0ba92bbeb8b0fff2a9199f722e3a621a"|...|


注意，在跨域的情况下，request的定义有些特别：

```

为了提高安全性，浏览器添加了同源策略来限制跨域访问。但与此同时降低了开发的灵活性。为此，浏览器提供了折中方法，即只要遵循 CORS protocol，跨域也可访问。

CORS, cross-origin resource sharing, 即跨域资源共享。

在跨域的情况下，request被分为simple request和preflighted request。

simple request that is always considered authorized and is not explicitly listed in responses to preflight requests，即发送simple request无需先发送OPTIONS请求验证。而在发送preflighted request之前，会先发送OPTIONS请求去验证CORS protocol是否能被understand，来决定是否发送真正的请求。

1. 什么时候称为preflighted request？

满足以下任一条件：

1.1 请求方法为put、delete、connect、options、trace、patch
1.2 请求头不含有CORS-safelisted request header，如Accept, Accept-Language, Content-Language, Content-Type(application/x-www-form-urlencode或multipart/form-data或text/plain)。如某个请求头含有Content-Type: application/xml，因为不是条件里Content-Type指定的三种类型之一，所以这个请求就是一个preflighted request

2. 实现CORS怎么通过服务端来做？

2.1 为response header添加Access-Control-Allow-Origin，表示允许跨域访问。

Access-Control-Allow-Origin: * - 表示任意来源都可以访问
Access-Control-Allow-Origin: http://test.com - 表示来源域名为test.com的可以访问

如果使用了Access-Control-Allow-Credentials: true，就不能使用Access-Control-Allow-Origin: *，即必须指定某个域名。


2.2 如果请求为preflighted request，则还要为response header添加Access-Control-Allow-Methods和Access-Control-Allow-Headers

当由于请求方法的原因，需要设置：
'Access-Control-Allow-Methods': 'OPTIONS, PUT, DELETE, TRACE, PATCH'

当由于request header原因使得请求变成preflighted request，如Content-Type: application/json，需要设置：
'Access-Control-Request-Headers': 'Content-Type'


2.3  如果请求发送了http cookies或者http authentication information，要为response header添加Access-Control-Allow-Credentials：

'Access-Control-Allow-Credentials': true

tips：客户端想要在跨域时发送cookie等信息需要设置withCredentials: true(ajax)或credentials: 'include'(fetch)

```

- **Response headers**：a response header is an http header that can be used in an http response, and that *doesn't relate to the content of the message*. Response headers like Age, Location or Server are used to give a more detailed context of the response.

key|value|description|
--|--|--|
Access-Control-Allow-Origin|\*|...|
Access-Control-Allow-Methods|'OPTIONS, PUT, DELETE, TRACE, PATCH'|...|
Access-Control-Allow-Credentials|true||
Etag|"c561c68d0ba92bbeb8b0f612a9199f722e3a621a"|...|
Keep-Alive|timeout=5, max=997|...|
Last-Modified|Mon, 18 Jul 2016 02:36:04 GMT|...|
Server|Apache|...|
Set-Cookie|...|...|
Transfer-Encoding|chunked|...|
Vary|Cookie, Accept-Encoding|...|
X-Backend-Server|developer2.webapp.scl3.mozilla.com|...|
X-Cache-Info|not cacheable; meta data too large|...|
X-kuma-revision|1085259||
x-frame-options|DENY|...|

- **Entity headers**：an entity header is an http header that describing the content of the body of the message. Entity headers are used in *both request and response message*. Entity headers like Content-Length, Content-language, Content-Encoding, Content-Type.

key|value|description|
--|--|--|
Content-Length|...|...|
Content-language|...|...|
Content-Encoding|gzip|...|
Content-Type|application/json;charset=UTF-8|this entity header is used to indicate the media type of the resource or the data. [Imoportant MIME types for web developers](https://github.com/hoanFir/blogs/blob/master/%E7%BD%91%E7%BB%9C%E8%AF%B7%E6%B1%82%E5%B0%81%E8%A3%85/Important%20MIME%20types%20for%20Web%20developers.md) |

---

### Request Headers示例

```

GET /api/session/getContext HTTP/1.1

Host: uat.xiaozhi.jd.com
Connection: keep-alive
Accept: application/json, text/plain, */*
X-Requested-With: XMLHttpRequest
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/78.0.3904.70 Safari/537.36
Referer: http://uat.xiaozhi.jd.com/
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9
Cookie: ...

```

---

### Response Headers示例

```

HTTP/1.1 200 OK

Server: jfe
Date: Thu, 31 Oct 2019 06:23:45 GMT
Content-Type: application/json;charset=UTF-8
Transfer-Encoding: chunked
Connection: keep-alive
Expires: Thu, 31 Oct 2019 06:23:45 GMT
Cache-Control: max-age=0

```
