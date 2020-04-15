
Not all requests or packets carry an equal weight. Given this, round robin, or even the weighted(权重) round robin, will not fit the need of all applications or traffic flow. 

因此，NGINX 提供了许多负载平衡算法，用于适应特定的用例。

These load-balancing algorithms or methods can not only be chosen, but also configured. And they are available for upstream HTTP, TCP, and UDP pools:


## 一、Round robin

轮询。

the default load-balancing method and is used if no other algorithm is specified, which distributes requests in order of the list of servers in the upstream pool.

weight can be taken into consideration for a weighted round robin, which could be used if the capacity(容量) of the upstream servers varies. the higher the integer value for the weight, the more favored the server will be in the round robin. the algorithm behind weight 只是加权平均值的统计概率。



## 二、Least connections

最少连接，`least_conn`.

this method balances load by proxying the current request to the upstream server with the least number of open connections proxied through NGINX.

weight can be taken into consideration for a weighted least connections, which could be used to decide to which server to send the connection.

```

upstream backend { 
  least_conn;
  server backend.example.com;
  server backend1.example.com; 
}

```

## 三、Least time

最少时间，`least_time`.

this method balances load by proxying the current request to the upstream server with the lowest average response times.

this method is one of the most sophisticated load-balancing algorithms out there and fits the need of highly performant web applications.



## 四、Generic hash

NGINX distributes the load amongst the servers by producing a `hash` for the current request and placing it against the upstream servers.

this method is very useful when you need more control over `where requests are sent` or `determining what upstream server most likely will have the data cached`.

note that when a server is added or removed from the pool, the hashed requests will be redistributed.

this algorithm has an optional parameter, `consistent`, to minimize the effect of redistribution.



## 五、IP hash

`ip_hash`.

supported for HTTP.

this method uses the client IP address as the hash, ensures that clients get proxied to the same upstream server as long as that server is available, which is extremely helpful when the session state is of concern and not handled by shared memory of the application.

different from using the remote variable in a generic hash, this algorithm uses the first three octets(字节) of an IPv4 address or the entire IPv6 address.

note that this method also takes the weight parameter into consideration when distributing the hash. 

