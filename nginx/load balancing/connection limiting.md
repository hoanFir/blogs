
## 场景：

You have too much load overwhelming your upstream servers.

## 方案：

使用 NGINX `max_conns` parameter 来 limit connections to  upstream servers.

```

upstream backend {
  zone backends 64k;
  queue 750 timeout=30s;
  
  server xx1.xx.xx max_conns=25;
  server xx2.xx.xx max_conns=15;  
}

```

## description

