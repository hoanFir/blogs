
场景：You need to cache content and need to define where the cache is stored.

```

proxy_cache_path /var/nginx/cache
                 keys_zone=CACHE:60m
                 levels=1:2
                 inactive=3h
                 max_size=20g;
proxy_cache CHCHE


# description

the proxy_cache_path derective to define shared memory cache zones and a location for the content.

```
