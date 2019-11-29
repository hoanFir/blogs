🐾 https优化相关

🕘 2019.11.29 由 hoanfirst 编辑

### 目前存在的问题

黑产攻击方式升级，当用户使用APP时劫持流量注入JS打开友商APP，或者合作企业PC端上用户点击本企业投放的广告跳转时在http流量注入一些跳转，先跳到他们的PC端再跳转到本企业相关网站，这样对本企业广告费用结算造成损失。

另外，各互联网平台的合作伙伴通过劫持流量盗取用户cookie，也会造成巨大的损失。

因此，需要**全站所有对外流量以https进行服务（为了保证https服务的质量，必须每个域名去进行强跳并设置hsts响应头）**。

### https相关介绍

https = http + TLS/SSL，即在http和tcp之间加了一层TLS/SSL。

1. 商用密码的核心：保证数据完整、身份互信

保证通信时彼此身份是可信的，数据是加密的不可篡改的。

```

核心1.1. 对称加密

加密和解密的密钥是同一个，主要是对数据报文做加解密。


核心1.2 非对称加密

密钥是一对的，即通过一个密钥加密的结果，只能用另外一个密钥去解密，主要是做身份验证。因为其中一个密钥（私钥）可以不发出去，这个密钥加密的东西称为签名，这样对方就可以信任请求的用户。


核心1.3 摘要/哈希

将一个数据报文变成哈希值，如MD5和SHA256，主要用来验证数据是否被篡改。

（其实有了上面的三个核心：数据加密、身份验证、数据完整性保证，就足够了）

（为了性能上的考虑，还可以引入密钥协商算法）


核心1.4 密钥协商

安全的协商随机的对称加密密钥。

```

2. 信任逻辑：如何判断持有私钥的一方就是合法的那个一方

使用公开密码学的基础设施：依靠可信机构颁发的证书。

```

2.1 [信任身份]靠[签名]

签名上文已经讲过，非对称加密中私钥加密过的结果就是签名。

2.2 [身份的声明（证书）]靠[可信机构的私钥签名]

可信机构颁发证书。

2.3 [可信机构的信任]靠[系统、浏览器预装或用户手动信任]


```

### 解决方案

![](https://github.com/hoanFir/blogs/blob/master/%E5%B7%A5%E7%A8%8B%E9%97%AE%E9%A2%98/images/%E4%BC%81%E4%B8%9A%E5%92%9A%E5%92%9A%E6%88%AA%E5%9B%BE20191129105040.png?raw=true)


- 注意

当用户第一次使用http域名时，这个过程还是有可能被流量劫持的，为了解决这个问题，可以通过网关给响应加一个HSTS响应头，浏览器会自动换成https进行访问。


- 示例：访问 https://www.jd.com

```

(request headers)
Request URL: http://www.jd.com/
Request Method: GET
Status Code: 307 Internal Redirect

(response headers)
Location: https://www.jd.com/
Non-Authoritative-Reason: HSTS

```

- 拓展：重定向状态码

```

1. 301, Moved Permanently

The requested resource has been permanently moved to the new location, and any future reference to this resource should use one of the several uris returned by this response.

tips:
1.1 the response is cacheable by default
1.2 if this is not a GET or HEAD request, the browser prohibits automatic redirection unless confirmed by the user


2. 302, Moved Temporarily

The client is required to use a temporary redirect, and since such a redirect is temporary, the client should continue sending future requests to the original address.

tips:
2.1 the response is cacheable only if it is specified in cache-control or Expires
2.2 if this is not a GET or HEAD request, the browser prohibits automatic redirection unless confirmed by the user


注意：虽然RFC 1945和RFC 2068规范不允许客户端在重定向时改变请求的方法，但是很多现存的浏览器将302视作303，并且使用GET方式访问在Location中规定的URI，而无视原先请求的方法。因此状态码303和307被引入，用以明确服务器期待客户端进行何种反应。


3. 3.7 Temporary Redirect

Unlike 302, you are not allowed to change the request method when the original request is reissued.

```
