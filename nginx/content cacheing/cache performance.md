
## 场景

You need to increase performance by caching on the client side.


## 方案

use client-side cache control headers:

```

location ~* \.(css|js)$ {
  expires 1y;
  add_header Cache-Control "public";
}

```

This location block specifies that the client can cache the content of CSS and JavaScript files. 

The `expires` directive instructs the client that their cached resource will no longer be valid after one year.

The `add_header` directive adds the HTTP response header Cache-Control to the response, with a value of `public`, which allows any caching server along the way to cache the resource.

## descirption

There are many things within the NGINX configu‐ ration you can do to assist with cache performance. 

One option is: 客户端本地缓存，to set headers of the response in such a way that the client actually caches the response and does not make the request to NGINX at all, but simply serves it from its own cache.







