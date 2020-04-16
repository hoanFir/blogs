
## 场景：

You have too much load overwhelming your upstream servers. 

Overloading a server will cause it to **queue connections** in a listen queue. And depending on the upstream server to process its own queue will lead to poor user experience as connections start to timeout, even load the server’s listen queue beyond repair.

So the ideal approach is for the load balancer to be aware of the connection limitations of the server and queue the connections itself so that it can send the connection to the next available server with understanding of load as a whole.



## 方案：

NGINX `max_conns` parameter to limit connections to upstream servers.

```

upstream backend {
  zone backends 64k;
  queue 750 timeout=30s;
  
  server xx1.xx.xx max_conns=25;
  server xx2.xx.xx max_conns=15;  
}

```

## description

If the max number of connections has been reached on each server, the request can be placed into the queue for further processing, provided(前提是) the optional `queue` directive is specified.

The optional `queue` directive sets the maximum number of requests that can be simultaneously in the queue. **When queuing, we must take into consideration a reasonable queue length**. Much like in everyday life, users and applications would much rather be asked to come back after a short period of time than wait in a long line and still not be served. 

The `timeout` parameter of the queue directive specifies how long any given request should wait in queue before giving up, which defaults to 60 seconds. 

A shared memory `zone` allows NGINX Plus worker processes to share information about `how many connections are handled by each server` and `how many requests are queued`.



