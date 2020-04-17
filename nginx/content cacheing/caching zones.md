
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

the proxy_chache derective informs a particular context to use the cache zone.

This example creates a directory of cached response on the filesystem at /var/nginx/cache, creates a shared memory space named CACHE with 60 megabytes of memory, sets the directory structure levels, defines the release of cached responses after they have not been requested in 3 hours, and defines a maximum size of the cache of 20 gigabytes. 


```


To configure caching in NGINX, it’s necessary to declare a path and zone to be used.
