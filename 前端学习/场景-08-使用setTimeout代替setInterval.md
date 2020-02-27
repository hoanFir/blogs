- **setTimeout()** ：在指定的毫秒数后调用函数或计算表达式，方法只执行一次。可以在中途使用 clearTimeout() 终止。
- **setInterval()** ：按照指定的周期（以毫秒计）来调用函数或计算表达式。方法会不停地执行，直到 clearInterval() 被调用或窗口被关闭。

>注意，两个方法的回调函数，都是经过指定毫秒数后被添加到**事件/任务待处理队列**中，而不是立即执行。即定时器指定的时间间隔，表示的是**何时将定时器的代码添加到消息队列**中，而不是何时执行代码！

## 1. 及时性问题

```javascript
var d = new Date, count	= 0, f, timer;
timer = setInterval(f = functino() {
	if(new Date - d >1000) clearInterval(timer), console.log(count);
}, 0);
```
在上面代码中，可以发现 1s 中运行的次数大概在 200 次，为什么不是 1000 次呢？？？
这是因为 `setInterval` 和 `setTimeout`	函数在 HTML 规范中指定运转的最短周期是 4/5ms 左右。

**如果需要更加短的周期，可以使用：**
- requestAnimationFrame（允许	JavaScript 以 60+帧/s	的速度处理动画，所以其运行时间间隔是要短很多的 ）
- process.nextTick
- ajax
- 插入节点的 readState 变化
- MutationObserver
- setImmediate

## 2. 阻塞问题

由于 Javascript 是单线程的，即`无论什么时候都只有一个 JS 线程在运行 JS 程序`，所以会存在被阻塞的情况。如下例中我们期望 console 在 1s 之后出结果，可事实上他却是在 2075ms 之后运行。

```javascript
	var d = new Date;
    setTimeout(function() {
        console.log(i);
    }, 1000);
    while(1) if(new Date - d > 2000) break;
```

另外，还有一个经典的例子：

```javascript
for(var i=0; i<5; i++){
    setTimeout(function() {
        console.log(i);
    }, 1000);
}
```
上例的输出结果是：1秒后立即输出5个5。原理如下：

每个 setTimeout 通过定时触发器线程来计时，在计时完毕后，handler 事件被触发，其会被`事件触发线程`添加到待处理队列的队尾，等待`JS引擎线程`空闲后处理。所以上例中首先是`JS引擎线程`运行 for 循环，每次循环都会调用一个 setTimeout 函数，每个setTimeout 计时结束后都会将其回调函数添加到事件队列中，当 for 循环结束，即`JS引擎线程`空闲了，就马上按顺序执行事件队列中的函数，所以表面上结果是1秒后立即输出5个5。

## 3. 捕获问题

try...catch 捕捉不到它的错误


## 4. 与 DOM 事件一起使用

我们知道，浏览器是个多进程应用，包含`Browser进程`、`第三方插件进程`、`GPU进程`和`浏览器内核/浏览器渲染进程`等。

浏览器内核/浏览器渲染进程是多线程的：`javascript 引擎线程`、`GUI 渲染线程`、`事件触发线程`、`定时触发器线程`、`异步 http 请求线程`。
- javascript 引擎线程：javascript 引擎是基于事件驱动单线程执行的，一直等待任务队列中任务的到来，然后处理执行。浏览器无论什么时候都只有一个 js 线程在运行 js 程序。
- GUI 渲染线程：负责渲染浏览器界面，当界面需要**重绘 repaint**或由于某种操作引发**回流 reflow**时，该线程就会执行。GUI 渲染线程和 JS 引擎线程是互斥的，当其中一个执行，另一个会被挂起，GUI 更新会被保存在一个队列中等待 JS 引擎空闲时立即被处理。
- 事件触发线程：当一个事件被触发时，该线程会将其添加到待处理队列的队尾，等待 JS 引擎线程的处理。这些事件往往来自于 `javascript 引擎线程当前执行的代码块如 setTimeout`、`鼠标点击`、`ajax异步请求`等。

其中 **javascript引擎** 和 **GUI 渲染** 的线程是互斥（互斥访问 DOM元素）的。所以如果在一个 DOM 事件中（特别是 onmousexxx和onkeyxxx事件），对另外的 DOM元素 进行操作（如 focus）时，就需要设置 setTimeout 将这些操作添加到事件触发的任务队列中，等待当前的 DOM 操作执行完后执行。

## 5. 使用 setTimeout 递归实现 setInterval

[原文]https://blog.csdn.net/b954960630/article/details/82286486

总所周知，setInterval 有两个缺点：
1. 某些间隔会被跳过
2. 可能多个定时器会连续执行

这是因为每个 setTimeout 产生的任务会直接 push 到任务队列中，而 setInterval 在每次把任务 push 到任务队列时，都要进行一次判断（判断 上次的任务是否仍在队列中，是则跳过）。所以通过用 setTimeout 模拟 setInterval 可以规避上面的缺点。

```javascript
function mySetInterval(fn, millisec){

  function interval(){
    setTimeout(interval, millisec);
    fn();
  }
  
  setTimeout(interval, millisec)
}
```
