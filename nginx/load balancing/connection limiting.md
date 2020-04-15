
## 场景：

You have too much load overwhelming your upstream servers.

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

The optional `queue` directive sets the maximum number of requests that can be simultaneously in the queue.

A shared memory `zone` allows NGINX Plus worker processes to share information about `how many connections are handled by each server` and `how many requests are queued`.
