

在学习 setTimeout 等定时器的知识时，就会了解到 window.requestAnimationFrame()。

对于兼容高版本浏览器或应用在移动端，又或者对于页面需要追求高精度的效果，可以使用浏览器原生方法 rAF，requestAnimationFrame。

### 一、window.requestAnimationFrame()

该方法是用来在页面重绘之前，通知浏览器调用一个指定的函数，其作为该方法的参数。

requestAnimationFrame 常用于 web 动画的制作，用于准确控制页面的帧刷新渲染，让动画效果更为流畅。

### 二、定时器

开发者可以利用 rAF 的特性将其视为一个定时器。

如，使用 rAF 触发 scroll 事件，相当于做节流，时间间隔为 `1000/60`。

```

//1

throttle(func, 500, 1000/60);


//2

var ticking = false; //rAF触发锁

function onScroll() {
  if(!ticking) {
    requestAnimationFrame(handlerFunc);
    ticking = true;
  }
}

function handlerFunc() {
  console.log("hello");
  
  ticking = false;
}

window.addEventListener("scroll", onScroll, false);

```

上述代码，要求在滚动过程中，保持以 16.7 ms 的频率触发事件。

### 三、优缺点

使用 requestAnimationFrame 优缺点并存。

首先不得不考虑它的兼容问题，其次因为它只能实现以 16.7ms 的频率来触发，代表它的可调节性十分差。

但是相比 throttle(func, xx, 16.7) ，用于更复杂的场景时，rAF 可能效果更佳，性能更好。
