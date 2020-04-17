
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


## description

A common concept for static files is to put a hash of the file in the filename. This ensures that as we roll out new code and content, our CDN recognizes this as a new file because the URI has changed.

Howerver, this does not exactly work for dynamic content to which we've set cache keys.

In every caching scenario, we must have a way to purge the cache. Nginx Plus has provided a simple method of purging cached responses.

```

The proxy_cache_purge directive, when passed a nonzero or non‐ empty value, will purge the cached items matching the request. 

A simple way to set up purging is by mapping the request method for PURGE. 

However, you may want to use this in conjunction with the geo_ip module or a simple authentication to ensure that not anyone can purge your precious cache items. NGINX has also allowed for the use of *, which will purge cache items that match a common URI prefix. To use wildcards(通配符) you will need to configure your proxy_cache_path directive with the purger=on argument.

```




