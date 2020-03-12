

OAuth，是一个用于授权的安全协议，用于保护全球范围内大量且不断增长的 Web API。

OAuth 用于连接不同的网站，还支持原生应用和移动应用与云服务之间的连接。

OAuth 已经成为当今 Web 占主导地位的安全手段，为开发人员保护应用提供了一种良好的机制。

## 一、OAuth 2.0 是什么

OAuth 2.0 是一个授权协议，允许`应用(如第三方客户端应用)`代表`资源拥有者(如享有某个服务的用户)`去访问`资源拥有者的资源(如某个服务提供给用户的受保护资源)`。具体来说，应用向资源拥有者请求授权，授权过程包含：应用让资源拥有者重定向到授权服务器、资源拥有者登录到授权服务器、向授权服务器请求获取 `code(授权码，表示资源拥有者同意向客户端授权)`、再通过授权码和客户端凭据从授权服务器获取到 `access_token(访问令牌)`。最后，用该 `token` 访问指定服务上的受保护资源。


## 二、 具体流程

客户端，即用户访问的第三方客户端应用（注意，不是 Web 浏览器），要向另一个用户使用的服务请求授权，访问相关资源。如用户使用打印服务时，要从照片存储服务上授予相关权限给打印服务，后者才能打印照片。

### 111

详细过程如下图：

![](https://github.com/hoanFir/blogs/blob/master/%E5%B7%A5%E7%A8%8B/images/%E6%88%AA%E5%B1%8F2020-03-12%E4%B8%8B%E5%8D%884.34.42.png?raw=true)


### 222

首先，当客户端应用需要获取一个新的 OAuth access_token 时，它会将资源拥有者重定向至授权服务器，并附带一个授权请求，表示其要向资源拥有者请求一些权限，如下图：

![](https://github.com/hoanFir/blogs/blob/master/%E5%B7%A5%E7%A8%8B/images/%E6%88%AA%E5%B1%8F2020-03-12%E4%B8%8B%E5%8D%884.35.52.png?raw=true)

**假如使用的是 Web 客户端应用，即采用 HTTP 重定向方式**将用户代理重定向至授权服务器的授权端点。

假设：

```

用户使用 Web 浏览器进行操作

http://location:9000: 打印服务（第三方客户端应用）

http://location:9001: 授权服务器

http://location:9002: 照片存储服务

```

打印服务应用的响应：

```

HTTP/1.1 302 Moved Temporarily
Location: http://localhost:9001/authorize?response_type=code&scope=foo&client _id=oauth-client-1&redirect_uri=http%3A%2F%2Flocalhost%3A9000%2Fcallback&state=Lwt50DDQKUB8U7jtfLQCVGDL9cnmwHH1
Content-Type: text/html; charset=utf-8 
Content-Length: 444
Connection: keep-alive
...

```

打印应用服务通过在这个响应里包含查询参数，来标识自己的身份和要请求的授权详情，如权限范围。

这个重定向响应，会导致浏览器向授权服务器发送一个 GET 请求，授权服务器会解析携带的参数作出适当的反应：

```

GET /authorize?response_type=code&scope=foo&client_id=oauth-client -1&redirect_uri=http%3A%2F%2Flocalhost%3A9000% 2Fcallback&state=Lwt50DDQKUB8U7jtfLQCVGDL9cnmwHH1 HTTP/1.1
Host: localhost:9001
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8 
Referer: http://localhost:9000/
Connection: keep-alive
...

```



### 333

然后，授权服务器会要求用户进行身份认证，从而确认资源拥有者的身份：

![](https://github.com/hoanFir/blogs/blob/master/%E5%B7%A5%E7%A8%8B/images/%E6%88%AA%E5%B1%8F2020-03-12%E4%B8%8B%E5%8D%884.36.02.png?raw=true)

该过程对能向客户端授予哪些权限来说至关重要。

用户身份认证直接在用户和授权服务器之间进行，对打印应用服务是不可见的。**避免了用户将自己在照片存储服务注册的凭据信息暴露给打印应用服务，对抗这种反模式正是发明 OAuth 的原因**。


### 333-3

另外，因为资源拥有者通过浏览器和授权服务进行交互，即通过浏览器来完成身份认证。OAuth 没有规定应该使用哪种身份认证技术，授权服务可以自由选择，如用户名/密码、加密证书、安全令牌、联合单点登录或其他方式。

这就说明在一定程度上信任 Web 浏览器，尤其是当资源拥有者使用像用户名/密码这样的简单身份认证方式时。当然，OAuth 的设计已经考虑了如何防止多种基于浏览器的攻击。

### 444

接着，用户通过授权服务器批准给打印服务应用授权，如下图：

![](https://github.com/hoanFir/blogs/blob/master/%E5%B7%A5%E7%A8%8B/images/%E6%88%AA%E5%B1%8F2020-03-12%E4%B8%8B%E5%8D%884.36.07.png?raw=true)

在这一步，资源拥有者选择将一部分权限授予客户端应用。授权服务器可以允许用户拒绝一部分或者全部权限范围，也可以让用户批准或拒绝整个授权请求。


### 444-4

此外，很多授权服务器允许将授权决策保存下来，以便以后使用。如果使用了这种方式，那么未来同一个应用客户端请求同样的授权时，用户将不会得到提示。用户仍然会被重定向到授权端点， 并且仍然需要登录，但是会跳过批准授权环节而沿用前一次的授权决策。

授权服务器甚至可以通过像客户端白名单和黑名单这样的内部策略来否决用户的决策。


### 555

再接着，授权服务器将用户重定向回打印服务应用：

![](https://github.com/hoanFir/blogs/blob/master/%E5%B7%A5%E7%A8%8B/images/%E6%88%AA%E5%B1%8F2020-03-12%E4%B8%8B%E5%8D%884.36.15.png?raw=true)

```

HTTP 302 Found
Location: http://localhost:9000/oauth_callback?code=8V1pr0rJ&state=Lwt50DDQKU B8U7jtfLQCVGDL9cnmwHH1

```

从而像打印应用服务发起请求：

```

GET /callback?code=8V1pr0rJ&state=Lwt50DDQKUB8U7jtfLQCVGDL9cnmwHH1 HTTP/1.1 Host: localhost:9000
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.10; rv:39.0) Gecko/20100101 Firefox/39.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8 
Referer: http://localhost:9001/authorize?response_type=code&scope=foo&client_ id=oauth-client-1&redirect_uri=http%3A%2F%2Flocalhost%3A9000%2Fcallback& state=Lwt50DDQKUB8U7jtfLQCVGDL9cnmwHH1
Connection: keep-alive
...

```

即将授权码包含在查询参数里，打印服务应用解析该参数以获取，从而再下一步使用。

授权码，在面


### 666

![](https://github.com/hoanFir/blogs/blob/master/%E5%B7%A5%E7%A8%8B/images/%E6%88%AA%E5%B1%8F2020-03-12%E4%B8%8B%E5%8D%884.36.22.png?raw=true)

### 777

![](https://github.com/hoanFir/blogs/blob/master/%E5%B7%A5%E7%A8%8B/images/%E6%88%AA%E5%B1%8F2020-03-12%E4%B8%8B%E5%8D%884.36.29.png?raw=true)


### 888

![](https://github.com/hoanFir/blogs/blob/master/%E5%B7%A5%E7%A8%8B/images/%E6%88%AA%E5%B1%8F2020-03-12%E4%B8%8B%E5%8D%884.36.36.png?raw=true)
