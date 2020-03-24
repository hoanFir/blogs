

webpack 4.6.0+ adds support for prefetching and preloading.

**通过 prefetch 和 preload，可以让客户端快速渲染出页面，然后再加载/并行加载可能会使用的一些其他组件。**

Using these inline directives while declaring your imports allows webpack to output “Resource Hint” which tells the browser that for:

- prefetch

resource is probably needed for some navigation in the future

- preload

resource might be needed during the current navigation


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

1. A preloaded chunk starts loading in parallel to the parent chunk. A prefetched chunk starts after the parent chunk finishes loading.

2. A preloaded chunk has medium priority and is instantly downloaded. A prefetched chunk is downloaded while the browser is idle.

3. A preloaded chunk should be instantly requested by the parent chunk. A prefetched chunk can be used anytime in the future.

4. Browser support is different.


Simple preload example: 

```

having a component, which always depends on a big library that should be in a separate chunk. 

Let's imagine a component ChartComponent which needs huge ChartingLibrary. It displays a LoadingIndicator when rendered and instantly does an on demand import of ChartingLibrary.

ChartComponent.js
import(/* webpackPreload: true */ 'ChartingLibrary');

This will result in:
<link rel="preload" href="charting-library-chunk.js">
tips: appended in the head of the page

When a page which uses the ChartComponent is requested, the charting-library-chunk is also requested via <link rel="preload">. Assuming the page-chunk is smaller and finishes faster, the page will be displayed with a LoadingIndicator, until the already requested charting-library-chunk finishes. 

This will give a little load time boost since it only needs one round-trip instead of two, especially in high-latency environments.

```




