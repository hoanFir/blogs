
## 场景：

You need to control how your content is cached and looked up.

## 方案

Use the `proxy_cache_key` directive, along with variables to define what constitutes a cache hit or miss:

```

proxy_cache_key "$host$request_uri $cookie_user";

```

this cache hash key will instruct NGINX to cache pages based on the host and URI being requested, as well as a cookie that defines the user.

With this you can cache dynamic pages without serving content that was generated for a different user.

## description

The default `proxy_cache_key` is "$scheme$proxy_host$request_uri". It will fit most use cases.

Selecting a good hash key is very important and should be thought through with understanding of the application:

- selecting a cache key for **static content** is typically pretty straightforward; using the hostname and URI will suffice

- Selecting a cache key for fairly **dynamic content** like pages for a dashboard application requires more knowledge around how users interact with the application and the degree of variance between user experiences. For security concerns you may not want to present cached data from one user to another without fully understanding the context.

