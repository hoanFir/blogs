🐾 Request and Response Headers

🕘 2019.10.31 由 hoanfirst 编辑

**HTTP headers let the client and the server pass additional information with an HTTP request or response**.

Headers can be grouped according to their contexts:

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
User-Agent|Mozilla/5.0 ...|
Referer|https://developer.mozilla.org/testpage.html|...|
Host|developer.mozilla.org|...|
Accept|application/json, text/plain, */*|...|
Accept-Language|zh-CN,zh;q=0.9|...|
Upgrade-Insecure-Request|1|...|
If-Modified-Since|Mon, 18 Jul 2016 02:36:04 GMT|...|
If-None-Match|"c561c68d0ba92bbeb8b0fff2a9199f722e3a621a"|...|

- **Response headers**：a response header is an http header that can be used in an http response, and that *doesn't relate to the content of the message*. Response headers like Age, Location or Server are used to give a more detailed context of the response.

key|value|description|
--|--|--|


- **Entity headers**：an entity header is an http header that describing the content of the body of the message. Entity headers are used in *both request and response message*. Entity headers like Content-Length, Content-language, Content-Encoding or MIME type.

key|value|description|
--|--|--|


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
