🐾 event-loop理解一

🕘 2019.10.25 由 hoanfirst 编辑

### 概述

对于，event-loop，涉及到许多知识，个人总结如下：

- 浏览器组成中的进程和线程

- event-loop


### 一、浏览器组成中的进程和线程

1）首先，浏览器由`用户界面`、`浏览器引擎`、`渲染引擎（网络、display Backend、JS解释器）`、`数据存储`组成

![](https://github.com/hoanFir/blogs/blob/master/%E5%89%8D%E7%AB%AF%E5%AD%A6%E4%B9%A0/images/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE20191025103315.png?raw=true)

具体查看[浏览器组成和缓存机制](https://github.com/hoanFir/blogs/blob/master/%E5%89%8D%E7%AB%AF%E5%AD%A6%E4%B9%A0/%E9%9D%A2%E8%AF%95-08-%E6%B5%8F%E8%A7%88%E5%99%A8%E7%BB%84%E6%88%90%E5%92%8C%E7%BC%93%E5%AD%98%E6%9C%BA%E5%88%B6.md)

2）理解进程和线程

**进程是CPU资源分配的最小单位，线程是CPU调度的最小单位。**

进程之间不共享资源，所以不会存在太多的安全问题，如浏览器中一个tab页或一个插件崩溃，也不会影响其他tab页应用和浏览器的运行。

线程是建立在进程的基础上的一个程序运行单位，一个进程可以有多个线程，同一个进程下的线程共享进程拥有的资源和地址空间。

3）对于谷歌浏览器，每个Tab页就是一个"渲染引擎"实例，也就是一个独立进程。

对于个现代浏览器，主要包括5类进程：

- 浏览器引擎实例/浏览器进程：一个浏览器应用程序只有一个浏览器进程，主要负责浏览器的标签页管理，书签管理和前进后退刷新管理等。

- GPU进程：一个浏览器应用程序只有一个GPU进程，主要负责GPU渲染。

- 网络进程/network service。

- 渲染引擎实例/渲染进程：对于谷歌浏览器，每个Tab页就是一个"渲染引擎"实例，也就是一个渲染进程。主要负责页面渲染、js解释和执行等。

- 插件进程：浏览器应用程序中每一个插件会创建一个进程。

**对于一个“渲染引擎”实例，有一些常驻线程相互配合以保持同步：**

1. GUI渲染线程：负责渲染HTML、css。当页面需要重绘(Repaint)或由于某种操作引发回流(reflow)时，该线程就会执行。注意，该线程在Javascript引擎执行脚本期间会处于挂起状态。

2. javascript引擎线程：负责解析和执行js代码。主要支持页面用户的交互，以及操作DOM树、CSS样式树。因为javascript引擎线程是可操作DOM的，如果在修改这些元素的同时渲染界面，那么GUI渲染线程前后获得的元素数据可能不一致，所以两者是互斥的，如果js执行时间过长，会导致页面加载渲染的阻塞。另外，js引擎采用单线程机制，因为多线程就需要引入锁机制来控制临界资源的访问，会更加复杂。

3. 定时器线程：浏览器定时计数器并不是由JS引擎计数的, 因为JS引擎是单线程的, 如果处于阻塞线程状态就会影响记计时的准确, 因此需要通过单独的线程来计时并触发定时。当检测到计时完成时，如果设置有回调事件，定时器线程就产生计时完成事件，事件触发线程会将其放到对应的`事件处理队列`中，再等待js引擎线程空闲时处理。

4. **事件触发线程：控制event-loop，这就是本文的重点。**

5. 异步请求线程：对于XMLHttpRequest连接，会生成一个单独的线程进行请求。当检测到状态变更时，如果设置有回调事件，异步请求线程就产生状态变更事件，事件触发线程会将其放到对应的`事件处理队列`中，再等待js引擎线程空闲时处理。


### 二、javascript引擎线程和event-loop

要理解js引擎单线程的工作过程，就需要理解event-loop。

1）在理解什么是event-loop之前，需要先理解同步任务和异步任务的概念。

同步任务：在主线程（js引擎线程）上排队执行的任务。

异步任务：任务不进入主线程，而是由事件触发线程调度，在满足特定条件时将任务放入事件队列中，等待主线程空闲时处理。

2）为什么要有event-loop的概念

由于JavaScript代码的解析和执行只能依靠js引擎线程，而js引擎线程是单线程的，又因为js中存在一些异步操作，如定时器回调事件和异步请求事件，这些事件怎么才能有序地被js引擎线程处理呢？

答案是需要通过一个事件处理队列，当js引擎执行完当前同步任务后，从事件处理队列中取出异步任务的回调事件进行处理。

3）有哪些常见异步任务

- setTimeout

- setInterval

- Promise

- 异步请求

4）**在ES2015之后，引入了微任务的概念**

如下面一段代码

```javascript

setTimeout(function(){}, 0);
Promise.resolve().then();

```

对于上面代码，会先执行then里面的内容，js中将异步任务插入事件处理队列中并不是简单的按照它们代码中的定义顺序，为什么？

因为在ES2015之后对于异步任务，分为task和microtask。相对microtask，原来的task就是宏任务。

宏任务：setTimeout、setTimeout、异步请求。

微任务：promise。

**微任务：promise。microtask 特点是会在当前的同步任务执行完成后立即执行。**


settimeout 和 promise 相关面试题 - 1：

```javascript

setTimeout(()=>console.log('1'), 0);
new Promise((resolve, reject) => {
  console.log('2');
  resolve();
}).then(()=>console.log('3'))
  .then(()=>console.log('4'))
process.nextTick(console.log('5'));
console.log('6'); 

//输出结果：2 6 5 3 4 1

```

settimeout 和 promise 相关面试题 - 2：

```javascript

setTimeout(()=>console.log('1'), 0);
new Promise((resolve, reject)=>{
  console.log('2');
  setTimeout(()=>resolve(), 0);
}).then(()=>console.log('3'))
  .then(()=>console.log('4'))
process.nextTick(console.log('5'));
console.log('6');

//输出结果：2 6 5 1 3 4 

```

5）js渲染引擎的执行过程

执行过程中先将同步任务执行完，空闲时读取当前微任务的事件处理队列，执行完所有当前微任务，再读取当前宏任务的事件处理队列，执行完所有当前宏任务。

