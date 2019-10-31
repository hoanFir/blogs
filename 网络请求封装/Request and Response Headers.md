🐾 Request and Response Headers

🕘 2019.10.31 由 hoanfirst 编辑

HTTP headers let the client and the server pass additional information with an HTTP request or response.

Headers can be grouped according to their contexts:

- General headers: a general header is an http header that can be used in both request and response message but doesn't apply to the content itself. Depending on the context they are used in, general headers are either request or response headers. However, they are not entity headers.


### Request Headers示例

```javasript

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

### Response Headers示例

```javascript

HTTP/1.1 200 OK

Server: jfe
Date: Thu, 31 Oct 2019 06:23:45 GMT
Content-Type: application/json;charset=UTF-8
Transfer-Encoding: chunked
Connection: keep-alive
Expires: Thu, 31 Oct 2019 06:23:45 GMT
Cache-Control: max-age=0

```
