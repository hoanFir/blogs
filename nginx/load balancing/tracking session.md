
Many modern web architectures employ stateless application tiers(应用层), storing state in shared memory or databases. 

However, **session state** is immensely valuable and vast in interactive applications. This state may be stored locally. 

When state is stored locally to an application server, it is extremely important to the user experience that the subsequent requests continue to be delivered to the same server. Another portion of the problem is that servers should not be released until the session has finished.

**So, working with stateful applications at scale, requires an intelligent load balancer.**

NGINX Plus offers multiple ways to solve this problem by tracking cookies or routing. NGINX Plus's `sticky` directive alleviates difficulties of server affinity at the traffic controller.

NGINX tracks session persistence in three ways: by creating and tracking its own cookie, detecting when applications prescribe cookies, or routing based on runtime variables.


## 一、sticky cookie

场景：You need to bind a downstream client to an upstream server.

```

upstream backend {
  server xx1.xx.xx;
  server xx2.xx.xx;
  
  sticky cookie
         affinity
         expires=1h
         domain=.xx.xx
         httponly
         secure
         path=/;
}


# description

this configuration creates and tracks a cookie, "affinity", that ties a downstream client to an upstream server.

the cookie is set for xx.xx, persists an hour, cannot be consumed client-side, can only be sent over HTTPS, and is valid for all paths.

```

NGINX Plus tracks this cookie, enabling it to continue directing subsequent requests to the same server. 


## 二、sticky learn

> \<\<Complete NGINX Cookbook\>\> p23

场景：You need to bind a downstream client to an upstream server by using an existing cookie.

```

upstream backend {
  server xx1.xx.xx:8080; 
  server xx2.xx.xx:8081;
  
  sticky learn 
         create=$upstream_cookie_cookiename
         lookup=$cookie_cookiename 
         zone=client_sessions:2m;
}

```


## 三、sticky routing

> \<\<Complete NGINX Cookbook\>\> p24

场景：You need granular(细粒度) control over how your persistent sessions are routed to the upstream server.

```

map $cookie_jsessionid $route_cookie {
  ~.+\.(?P<route>\w+)$ $route;
}

map $request_uri $route_uri {
  ~jsessionid=.+\.(?P<route>\w+)$ $route;
}

upstream backend {
  server xx1.xx.xx route=a;
  server xx2.xx.xx route=b;
  
  sticky route $route_cookie $route_uri; 
}


```



