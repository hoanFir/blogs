

**在 dom 渲染中，通常规定 16 ms 才执行一次。**

这是为什么？

因为，大多数显示器的刷新频率是 60 Hz，这相当于每秒钟会重绘 60 次，大多数浏览器都会对重绘操作加以限制，保证不超过显示器的重绘频率。即使超过了这个频率，用户体验也不会有任何提升。

那么我们就可以知道，1000 ms/60 ≈ 17ms(16ms)，即每隔大概 16ms 重绘一次，是动画效果最平滑的最佳循环间隔。

所以，对于动画，一方面要求变化时间必须足够短（保证动画效果更平滑），另一方面要求变化时间不能太短（保证浏览器有能力渲染发生的变化）。


### rAF

window.requestAnimationFrame()，该方法是用来在页面重绘之前，通知浏览器调用一个指定的函数，其作为该方法的参数。这样，就解决来不知道最佳循环间隔时间或者不知道什么时候绘制下一帧的问题。

requestAnimationFrame 常用于 web 动画的制作，用于准确控制页面的帧刷新渲染，让动画效果更为流畅。

特性：

1. 如果有很多个requestAnimationFrame要执行，浏览器也是通知一次就可以了

2. 如果页面被最小化，因为不会进行重绘，requestAnimationFrame也就不会被触发，提升性能和电池寿命

```javascript

window.requestAnimFrame

window.requestAnimFrame = (funciton() {
  return window.requestAnimationFrame 
         || window.webkitRequestAnimationFrame
         || window.mozRequestAnimationFrame
         || window.oRequestAnimationFrame
         || window.msRequestAnimationFrame
         || function(callback, element) {
              return window.setTimeout(callback, 1000/60);
            }
})();

```
可以发现，widow.requestAnimFrame 是 setTimeout 的加强版。

那么，为什么最好不使用多组 setTimeout 或 setInterval 来实现动画？

因为无论是setTimeout还是setInterval都不是十分精确 ，这我们在 window.setTimeout 也有解释过，它们需要等待，而且更消耗资源。
