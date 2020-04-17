
## 场景

You need the ability to bypass the caching.

## 方案

Use the proxy_cache_bypass directive with a nonempty or nonzero value. One way to do this is by setting a variable within location blocks that you do not want cached to equal 1:

```

proxy_cache_bypass $http_cache_bypass;

```

The configuration tells NGINX to bypass the cache if the HTTP request header named cache_bypass is set to any value that is not 0, so **the request will be sent to an upstream server rather than be pulled from cache**.


## description

There are many scenarios that demand that the request is not cached. One important scenario is troubleshooting and debugging, because reproducing issues(复现问题) can be hard if you’re consistently pulling cached pages or if your cache key is specific to a user identifier. 

Options include but are not limited to bypassing cache when a particular cookie, header, or request argument is set. We can also turn off cache completely for a given context such as a location block by setting `proxy_cache off`.

