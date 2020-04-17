
## 场景：

You need to control how your content is cached and looked up.

## 方案

Use the proxy_cache_key directive, along with variables to define what constitutes a cache hit or miss:

```

proxy_cache_key "$host$request_uri $cookie_user";

```
