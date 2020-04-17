
## 场景:

You need to cache content and need to define where the cache is stored.

## 方案

```

proxy_cache_path /var/nginx/cache
                 keys_zone=CACHE:60m
                 levels=1:2
                 inactive=3h
                 max_size=20g;
proxy_cache CHCHE

```

## description

This example creates a directory of cached response on the filesystem at /var/nginx/cache, creates a shared memory space named CACHE with 60 megabytes of memory, sets the directory structure levels, defines the release of cached responses after they have not been requested in 3 hours, and defines a maximum size of the cache of 20 gigabytes. 

**To configure caching in NGINX, it’s necessary to declare a path and zone to be used.**

A cache zone in NGINX is created with the directive `proxy_cache_path`. The `proxy_cache_path` designates a location to store the cached information and a shared memory space to store active keys and response metadata.






