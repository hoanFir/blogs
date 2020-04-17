
## 场景

You need to invalidate an object from the cache.

## 方案

Use NGINX Plus’s purge feature, the `proxy_cache_purge` directive, and a nonempty or zero value variable:

```

map $request_method $purge_method {
  PURGE 1;
  default 0;
}

server {
  ...
  location / {
    ...
    proxy_cache_purge $purge_method;
  }
}

```
