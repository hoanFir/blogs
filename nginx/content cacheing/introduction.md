
## 一、what

Caching accelerates content serving by storing request responses to be served again in the future.

## 二、why

Content caching reduces load to upstream servers, caching the full response rather than running computations and queries again for the same request.

Caching increases performance and reduces load, meaning we can server faster with fewer resources. **It’s optimal to host content close to the consumer for the best performance. This is the pattern of content delivery networks, or CDNs.**

So, with NGINX we can create our own CDN, and cache our content wherever we can place an NGINX server.





