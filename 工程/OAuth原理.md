

OAuth，是一个用于授权的安全协议，用于保护全球范围内大量且不断增长的 Web API。

OAuth 用于连接不同的网站，还支持原生应用和移动应用与云服务之间的连接。

OAuth 已经成为当今 Web 占主导地位的安全手段，为开发人员保护应用提供了一种良好的机制。

## 一、OAuth 2.0 是什么

OAuth 2.0 是一个授权协议，允许`应用(如第三方客户端应用)`代表`资源拥有者(如享有某个服务的用户)`去访问`资源拥有者的资源(如某个服务提供给用户的受保护资源)`。具体来说，对于应用，其向资源拥有者请求授权，授权过程包含：应用重定向到授权服务器、资源拥有者登录到授权服务器、向授权服务器请求获取 `code(授权码，表示资源拥有者同意向客户端授权)`、再通过授权码和客户端凭据，从授权服务器获取到 `access_token(访问令牌)`。最后，用该 `token` 访问指定服务上的受保护资源。


## 二、 具体流程

客户端，即用户访问的第三方客户端应用，要向另一个用户使用的服务请求授权，访问相关资源。如用户使用打印服务时，要从照片存储服务上授予相关权限给打印服务，后者才能打印照片。

### 111

详细过程如下图：

![](https://github.com/hoanFir/blogs/blob/master/%E5%B7%A5%E7%A8%8B/images/%E6%88%AA%E5%B1%8F2020-03-12%E4%B8%8B%E5%8D%884.34.42.png?raw=true)


### 222

当客户端应用需要获取一个新的 OAuth access_token 时，它会将资源拥有者重定向至授权服务器，并附带一个授权请求，表示其要向资源拥有者请求一些权限，如下图：

![](https://github.com/hoanFir/blogs/blob/master/%E5%B7%A5%E7%A8%8B/images/%E6%88%AA%E5%B1%8F2020-03-12%E4%B8%8B%E5%8D%884.35.52.png?raw=true)

**假如使用的是 Web 客户端应用，即采用 HTTP 重定向方式**将用户代理重定向至授权服务器的授权端点。

`http://location:9000`: 打印服务

`http://location:9001`: 照片服务

打印应用的响应：

```

HTTP/1.1 302 Moved Temporarily
Location: http://localhost:9001/authorize?response_type=code&scope=foo&client _id=oauth-client-1&redirect_uri=http%3A%2F%2Flocalhost%3A9000%2Fcallback& state=Lwt50DDQKUB8U7jtfLQCVGDL9cnmwHH1
Content-Type: text/html; charset=utf-8 
Content-Length: 444
Connection: keep-alive
...

```



### 333

![](https://github.com/hoanFir/blogs/blob/master/%E5%B7%A5%E7%A8%8B/images/%E6%88%AA%E5%B1%8F2020-03-12%E4%B8%8B%E5%8D%884.36.02.png?raw=true)


444

![](https://github.com/hoanFir/blogs/blob/master/%E5%B7%A5%E7%A8%8B/images/%E6%88%AA%E5%B1%8F2020-03-12%E4%B8%8B%E5%8D%884.36.07.png?raw=true)


555

![](https://github.com/hoanFir/blogs/blob/master/%E5%B7%A5%E7%A8%8B/images/%E6%88%AA%E5%B1%8F2020-03-12%E4%B8%8B%E5%8D%884.36.15.png?raw=true)

666

![](https://github.com/hoanFir/blogs/blob/master/%E5%B7%A5%E7%A8%8B/images/%E6%88%AA%E5%B1%8F2020-03-12%E4%B8%8B%E5%8D%884.36.22.png?raw=true)

777

![](https://github.com/hoanFir/blogs/blob/master/%E5%B7%A5%E7%A8%8B/images/%E6%88%AA%E5%B1%8F2020-03-12%E4%B8%8B%E5%8D%884.36.29.png?raw=true)


888

![](https://github.com/hoanFir/blogs/blob/master/%E5%B7%A5%E7%A8%8B/images/%E6%88%AA%E5%B1%8F2020-03-12%E4%B8%8B%E5%8D%884.36.36.png?raw=true)
