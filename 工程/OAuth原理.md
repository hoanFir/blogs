

## 一、OAuth 2.0 规范

### 1、角色划分：小程序登录获取用户数据场景

咱们先思考一个问题：小程序登录之后如果需要访问用户的数据（比如昵称、地域、性别等）需要得到谁的授权？是微信？还是用户？

答案是用户。用户的数据虽然存放在微信的服务器之上，但是这些数据的所有权属于用户自己，而不是微信。这里其实引出了 OAuth 2.0 规范中的两个基本概念：

Resource Owner：资源所有者，即用户；

Resource Server：资源服务器，即微信。

而小程序在获取用户数据中的角色是作为微信平台的第三方应用程序，在 OAuth 2.0 规范中的术语为 Third-party application。

除了以上三种角色之外，OAuth 2.0规范中还有另外三种角色：

小程序依托于微信提供的底层技术平台，微信为小程序提供了与用户（即Resource Owner）沟通的工具，它在 OAuth 2.0 规范中的角色被称为 User Agent（用户代理）。微信服务器不仅仅作为 Resource Server 保存用户数据，同时在登录授权过程中又提供了HTTP服务以及授权认证功能，这两个功能的角色在 OAuth 2.0 规范中分别被称为 HTTP Service（HTTP服务提供商）和Authorization server（认证服务器）。

以上便是 OAuth 2.0 规范中的所有角色，为了加强了解，我们再梳理一遍：

- Resource Owner（资源所有者）：在小程序场景下代表小程序的用户。

- Resource Server（资源服务器，即存放用户数据、资源的服务器）：在小程序场景下这个角色由微信服务器承担。

- Third-party application（第三方应用程序/又称客户端）：在小程序场景下代表小程序。

- User Agent（用户代理）：在小程序场景下代表微信。

- Authorization server（认证服务器）：在小程序场景下，这个角色由微信服务器承担。

- HTTP Service（HTTP 服务提供商）：在小程序场景下，这个角色由微信服务器承担。

看到这儿，你有没有疑惑为什么 OAuth 2.0 规范中的角色与小程序登录流程中角色不一样？其实，小程序登录流程中的三个角色是按照实体划分的，而 OAuth 2.0 规范的角色是按照功能划分的，同一个实体可以担任一种或多种功能。**在小程序登录流程中的 3 个实体角色中，微信服务器同时担任 Resource Server、Authorization server 和HTTP Service 的功能；开发者服务器比较特殊，它即担任 HTTP Service 的功能，同时在认证流程中由于需要转发和关联 token，所以也充当了客户端的一部分功能。**

## 二、认识OAuth

OAuth，是一个用于授权的安全协议，用于保护全球范围内大量且不断增长的 Web API。

OAuth 用于连接不同的网站，还支持原生应用和移动应用与云服务之间的连接。

OAuth 已经成为当今 Web 占主导地位的安全手段，为开发人员保护应用提供了一种良好的机制。

### 1、OAuth 2.0 是什么

OAuth 2.0 是一个授权协议，允许`应用(如第三方客户端应用)`代表`资源拥有者(如享有某个服务的用户)`去访问`资源拥有者的资源(如某个服务提供给用户的受保护资源)`。具体来说，应用向资源拥有者请求授权，授权过程包含：应用让资源拥有者重定向到授权服务器、资源拥有者登录到授权服务器、向授权服务器请求获取 `code(授权码，表示资源拥有者同意向客户端授权)`、再通过授权码和客户端凭据从授权服务器获取到 `access_token(访问令牌)`。最后，用该 `token` 访问指定服务上的受保护资源。


### 2、具体流程

客户端，即用户访问的第三方客户端应用（注意，不是 Web 浏览器），要向另一个用户使用的服务请求授权，访问相关资源。如用户使用打印服务时，要从照片存储服务上授予相关权限给打印服务，后者才能打印照片。

### 3、

详细过程如下图：

![](https://github.com/hoanFir/blogs/blob/master/%E5%B7%A5%E7%A8%8B/images/%E6%88%AA%E5%B1%8F2020-03-12%E4%B8%8B%E5%8D%884.34.42.png?raw=true)


### 4、

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



### 5、

然后，授权服务器会要求用户进行身份认证，从而确认资源拥有者的身份：

![](https://github.com/hoanFir/blogs/blob/master/%E5%B7%A5%E7%A8%8B/images/%E6%88%AA%E5%B1%8F2020-03-12%E4%B8%8B%E5%8D%884.36.02.png?raw=true)

该过程对能向客户端授予哪些权限来说至关重要。

用户身份认证直接在用户和授权服务器之间进行，对打印应用服务是不可见的。**避免了用户将自己在照片存储服务注册的凭据信息暴露给打印应用服务，对抗这种反模式正是发明 OAuth 的原因**。


### 6、

另外，因为资源拥有者通过浏览器和授权服务进行交互，即通过浏览器来完成身份认证。OAuth 没有规定应该使用哪种身份认证技术，授权服务可以自由选择，如用户名/密码、加密证书、安全令牌、联合单点登录或其他方式。

这就说明在一定程度上信任 Web 浏览器，尤其是当资源拥有者使用像用户名/密码这样的简单身份认证方式时。当然，OAuth 的设计已经考虑了如何防止多种基于浏览器的攻击。

### 7、

接着，用户通过授权服务器批准给打印服务应用授权，如下图：

![](https://github.com/hoanFir/blogs/blob/master/%E5%B7%A5%E7%A8%8B/images/%E6%88%AA%E5%B1%8F2020-03-12%E4%B8%8B%E5%8D%884.36.07.png?raw=true)

在这一步，资源拥有者选择将一部分权限授予客户端应用。授权服务器可以允许用户拒绝一部分或者全部权限范围，也可以让用户批准或拒绝整个授权请求。


### 8、

此外，很多授权服务器允许将授权决策保存下来，以便以后使用。如果使用了这种方式，那么未来同一个应用客户端请求同样的授权时，用户将不会得到提示。用户仍然会被重定向到授权端点， 并且仍然需要登录，但是会跳过批准授权环节而沿用前一次的授权决策。

授权服务器甚至可以通过像客户端白名单和黑名单这样的内部策略来否决用户的决策。


### 9、

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

授权码，在上面已经说明，表示用户授权决策的结果。它是一次性的凭据。


### 10、

现在客户端已经得到授权码，它可以将其发送给授权服务器的**令牌端点**获取照片存储服务的访问令牌：

![](https://github.com/hoanFir/blogs/blob/master/%E5%B7%A5%E7%A8%8B/images/%E6%88%AA%E5%B1%8F2020-03-12%E4%B8%8B%E5%8D%884.36.22.png?raw=true)

这一步，打印服务应用会发送一个 POST 请求给授权服务器，在 HTTP 主体中以表单格式传递参数，并在 HTTP 基本认证头部设置 client_id 和 client_secret。

```

POST /token
Host: localhost:9001
Accept: application/json 10 
Content-type: application/x-www-form-encoded

Authorization: Basic b2F1dGgtY2xpZW50LTE6b2F1dGgtY2xpZW50LXNlY3JldC0x
grant_type=authorization_code&redirect_uri=http%3A%2F%2Flocalhost%3A9000%2Fcallback&code=8V1pr0rJ

...

```


### 11、

授权服务器接收以上请求，如果请求有效，则颁发令牌：

![](https://github.com/hoanFir/blogs/blob/master/%E5%B7%A5%E7%A8%8B/images/%E6%88%AA%E5%B1%8F2020-03-12%E4%B8%8B%E5%8D%884.36.29.png?raw=true)


在这一步，授权服务器需要执行多个步骤以确保请求是合法的：

1. 首先，验证客户端凭据，即Authorization头部。确定是哪个客户端发起的
2. 然后，读取请求 body 中 code，并从中获取关于授权码的信息，包括发起初始授权请求的是哪个客户端，执行授权的是哪个用户，授权的内容是什么...

如果授权码有效且尚未使用过，而且发起该请求的客户端与最初发起授权请求的客户端相同，则授权服务器会生成一个新的访问令牌并返回给客户端：

```

HTTP 200 OK
Date: Fri, 31 Jul 2015 21:19:03 GMT Content-type: application/json
{
  "access_token": "987tghjkiu6trfghjuytrghj", 
  "token_type": "Bearer"
}

```

这里，我们使用了 OAuth **bearer 令牌**，这是通过响应中的 token_type 字段描述的。令牌响应中还可以包含一个刷新令牌(用于获取新的访问令牌而不必重新请求授权)，以及一些关于访问令牌的附加信息，比如令牌的权限范围和过期时间。客户端可以将访问令牌存储在一个安全的地方，以便以后在用户不在场时也能够随时使用。

OAuth 核心规范对 bearer 令牌的使用做了规定，无论是谁，只要持有 bearer 令牌就有权使用它。


### 12、

最后，有了令牌，打印服务应用就可以在访问受保护资源时出示令牌：

![](https://github.com/hoanFir/blogs/blob/master/%E5%B7%A5%E7%A8%8B/images/%E6%88%AA%E5%B1%8F2020-03-12%E4%B8%8B%E5%8D%884.36.36.png?raw=true)


打印服务客户端出示令牌的方式有很多种，备受推荐的是：**使用 Authorization 头部**

```

GET /resource HTTP/1.1
Host: localhost:9002
Accept: application/json
Connection: keep-alive

Authorization: Bearer 987tghjkiu6trfghjuytrghj

...

```

受保护资源可以从头部中解析出令牌，判断它是否有效，从中得知授权者是谁以及授权内容，然后返回响应。

受保护资源检查令牌的方式有多种，最简单的方式是：让授权服务器和资源服务器共享存储令牌信息的数据库。授权服务器在生成新的令牌时将其写入数据库，资源服务器在收到令牌时从数据库中读取它们。

