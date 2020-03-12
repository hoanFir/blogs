

OAuth，是一个用于授权的安全协议，用于保护全球范围内大量且不断增长的 Web API。

OAuth 用于连接不同的网站，还支持原生应用和移动应用与云服务之间的连接。

OAuth 已经成为当今 Web 占主导地位的安全手段，为开发人员保护应用提供了一种良好的机制。

## 一、OAuth 2.0 是什么

OAuth 2.0 是一个授权协议，允许`应用(如第三方客户端)`代表`资源拥有者(如享有某个服务的用户)`去访问`资源拥有者的资源(如某个服务提供给用户的受保护资源)`。具体点说，对于应用，其向资源拥有者请求授权，授权过程包含向授权服务器请求，获取 `code` 授权码，再通过授权码和客户端凭据，最后获取到 `access_token`，然后用该 `token` 访问指定服务上的受保护资源。

## 二、 具体流程

111

![](https://github.com/hoanFir/blogs/blob/master/%E5%B7%A5%E7%A8%8B/images/%E6%88%AA%E5%B1%8F2020-03-12%E4%B8%8B%E5%8D%884.34.42.png?raw=true)

222

![https://github.com/hoanFir/blogs/blob/master/%E5%B7%A5%E7%A8%8B/images/%E6%88%AA%E5%B1%8F2020-03-12%E4%B8%8B%E5%8D%884.35.52.png?raw=true]

333

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
