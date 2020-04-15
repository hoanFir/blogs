
NGINX fills this need in a number of ways, such as HTTP, TCP, and UDP load balancing.

## 一、HTTP Load Balancing

场景： distribute load between two or more HTTP servers.

方案：

使用 NGINX 的 HTTP module 在 HTTP 服务器上使用 upstream block 进行负载平衡:

```

upstream backend {
  server xx.xx.xx.xx:80         weight=1;
  server blance.example.com:80  weight=2;
}

server {
  location / {
    proxy_pass http://backend
  }
}


//description


upstream: 

the http upstream module defines a pool of destinations, either a list of Unix sockets, ip addresses, and dns records, or a mix. the upstream module also defines how any individual request is assgined to any of the upstream servers.

weight: 

instructs NGINX to pass twice as many connections to the second server, and the weight parameter defaults to 1.

```

