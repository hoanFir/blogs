
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


# description

1. upstream(module): 

the http upstream module defines a pool of destinations, either a list of Unix sockets, ip addresses, and dns records, or a mix. the upstream module also defines how any individual request is assgined to any of the upstream servers, along with a number of optional parameters.

these parameters give more control over the routing of requests.


2. weight(parameter): 

weight parameter instructs NGINX to pass twice as many connections to the second server, and the weight parameter defaults to 1.

```


## 二、TCP Load Balancing

场景： distribute load between two or more TCP servers.

方案：

使用 NGINX 的 stream module 在 TCP 服务器上使用 upstream block 进行负载平衡:

```

stream {
  upstream mysql_read {
    server read1.xx.xx.xx:3306         weight=5;
    server read2.xx.xx.xx:3306;    
    server blance.example.com:3306  backup;
  }

  server {
    listen 3306;
    proxy_pass mysql_read;
  }
}



# description

the server block instructs NGINX to listen on TCP port 3306 and balance load between two MySQL database read replicas, and lists another as a backup that will be passed traffic if the primaries are down.

the stream module, like the HTTP module, allows you to define upstream pools of servers and configure a listening server.

```

## 三、UDP Load Balancing

...
