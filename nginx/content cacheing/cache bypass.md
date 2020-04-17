
## 场景

You need the ability to bypass the caching.

## 方案

Use the proxy_cache_bypass directive with a nonempty or nonzero value. One way to do this is by setting a variable within location blocks that you do not want cached to equal 1:

```

proxy_cache_bypass $http_cache_bypass;

```

The configuration tells NGINX to bypass the cache if the HTTP request header named cache_bypass is set to any value that is not 0.
