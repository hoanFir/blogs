

webpack 4.6.0+ adds support for prefetching and preloading.

**通过 prefetch 和 preload，可以让客户端快速渲染出页面，然后慢慢加载可能会使用的一些其他组件。**

Using these inline directives while declaring your imports allows webpack to output “Resource Hint” which tells the browser that for:

- prefetch: resource is probably needed for some navigation in the future

- preload: resource might be needed during the current navigation


---


Simple prefetch example: 

```

having a HomePage component, which renders a LoginButton component which then on demand loads a LoginModal component after being clicked.

loginButton.js:
import(/* webpackPrefetch: true  */ 'LoginModal');

This will result in:
<link rel="prefetch" href="login-modal-chunk.js">
tips: appended in the head of the page

```


---

preload has bunch of differences compare to prefetch:






