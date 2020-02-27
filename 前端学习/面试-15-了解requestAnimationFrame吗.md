

在学习 setTimeout 等定时器的知识时，就会了解到 window.requestAnimationFrame()。

对于兼容高版本浏览器或应用在移动端，又或者对于页面需要追求高精度的效果，可以使用浏览器原生方法 rAF，requestAnimationFrame。

### 一、window.requestAnimationFrame()

该方法是用来在页面重绘之前，通知浏览器调用一个指定的函数，其作为该方法的参数。

requestAnimationFrame 常用于 web 动画的制作，用于准确控制页面的帧刷新渲染，让动画效果更为流畅。

### 二、定时器

开发者可以利用 rAF 的特性将其视为一个定时器。

