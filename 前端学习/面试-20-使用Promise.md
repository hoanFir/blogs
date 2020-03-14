
A Promise is an Object representing the eventual completion or failure of an asynchronous operation. 

This guide will explain consumption of returned promises before explaining how to create them.

Essentially, a promise is a returned object to which you attach callbacks, instead of passing callbacks into a funciton.


### 一、using Promises

#### 1.1 replace callback

在引入 promise 之前，基于向函数传入回调函数来处理执行异步任务。如：

```javascript

function successCallback(result) {}
function failureCallback(error) {}

createAsync(settings, successCallback, faliureCallback)

```

…modern functions return a promise you can attach your callbacks to instead:


```

createAsync(settings).then(successCallback, failureCallback);

```

That's shorthand for:

```javascript

const promise = createAsync(settings);

promise.then(successCallback, failureCallback);

```

#### 1.2 a promise comes with some guarantees

- Callbacks will never be called before the completion of the current run of the js event loop.（因此，当在循环里执行 promise 时常常要搭配闭包来每次传给 promise 不同的状态）

- Callbacks added with `then()` even after the success or failure of the asynchronous operation, will be called, as above.

- Multiple callbacks may added by calling `then()` several times. Each callback is executed on after another, in the order in which they were inserted.（使用 Promise 最直接的好处是支持链式调用）


#### 1.3/1 chaining: the great things about using promises

A common need, is to execute two or more asynchronous operations back to back.

In the old days, lead to the classic callback pyramid of doom：

```javascript

dosomething1(function(result1) {

  dosomething2(result1, function(result2) {
  
    dosomething3(result2, function(finalresult) {
    
      //...
    }, failureCallback)
    
  }, failureCallback)
  
}, failureCallback)

```


With modern functions, attach callbacks to the returned promises instead, foming a promise chain：

```javascript

dosomething1()
  .then(result1 => dosomething2(result1))
  .then(result2 => dosomething3(result2))
  .then(finalresult => { /*...*/ })
  .catch(failureCallback)
  
```

tips: `catch(failureCallback)` is short for `then(null, failureback)`



支持 chaining after a failure catch, which is useful to accomplish new actions even after an action failed in the chain：

```javascript
new Promise((resolve, reject) => {
  console.log('初始化')
  resolve();
})
.then(()=>{
  throw new Error('报错');
})
.catch(() => {
  console.log('捕获');
})
.then(() => {
  console.log('无论前面发生了什么都会执行该then');
})

```

#### 1.3/2 creating a Promise around an old callback API

In an ideal world, all asynchronous functions would already return promises. 

Unfortunately, some APIs still expect success and/or failure callbacks to be passed in the old way.

The most obvious example is the setTimeout() function：

```

setTimeout(() => saySomething("..."), 10*1000)

```

And if `saySomething()` fails or contains a programming error, nothing catches it(`setTimeout` is to blame for this)!


Luckily we can wrap `setTimeout` in promise:

```

const wait = ms => new Promise(resolve => setTimeout(resolve, ms));

wait(10*1000).then(() => saySomething('...')).catch(failureCallback);

```
  
  
#### 1.3/3 Composition

- 1、Promise.all()

For running asynchronous operations in parallel:

```javascript

//start operations in parallel and wait for them all to finish
Promise.all([func1(), func2(),func3()]).then(([result1, result2, result3])=>{})

```

- 2、Sequential composition

using some clever JavaScript:

```javascript

array.reduce(function(total, currentValue, currentIndex, arr), initialValue)

[fun1, fun2, fun3].reduce((p, f) => p.then(f), Promise.resolve()).then(result3 => { /* ... */})

```

在上述代码中，we reduce an array of asynchronous functions down to a promise chain equivalent to：`Promise.resolve().then(func1).then(func2).then(func3);`

In ECMAScript 2017, sequential composition can be done more simply with async/await:

```

let result;
for (const f of [func1, func2, func3]) {
  result = await f(result);
}

/* use last result... */

```

- 3、Promise.race()

For running asynchronous operations in parallel.

Different from Promise.all(), The Promise.race() method returns a promise that fulfills or rejects as soon as one of the promises in an iterable fulfills or rejects, with the value or reason from that promise.

```javascript

const promise1 = new Promise(function(resolve, reject) {
    setTimeout(resolve, 500, 'one');
});

const promise2 = new Promise(function(resolve, reject) {
    setTimeout(resolve, 100, 'two');
});

Promise.race([promise1, promise2]).then(function(value) {
  console.log(value);
  // Both resolve, but promise2 is faster
});
// expected output: "two"

```

#### 1.3/4 timing

To avoid surprises, functions passed to then() will never be called synchronously, even with an already-resolved promise:

```javascript

Promise.resolve().then(() => console.log(2));
console.log(1); // 1, 2

```

另外，在理解 event-loop 时我们知道异步任务分为任务（宏任务）和微任务，而 Promise 属于微任务。

具体来说：Instead of running immediately, the passed-in function is put on a microtask queue, which means it runs later when the queue is emptied at the end of the current run of the JavaScript event loop.

示例：

```javascript

const wait = ms => new Promise(resolve => setTimeout(resolve, ms));
wait().then(() => console.log(4));
Promise.resolve().then(() => console.log(2)).then(() => console.log(3));
console.log(1); // 1, 2, 3, 4

```

在上述代码中，Promise 是微任务，会在宏任务 settimeout 之前，js 同步任务之后执行。


#### 1.3/5 nesting: to limit the scope of catch statements

**Simple promise chains are best kept flat without nesting, because nesting can be a result of careless composition.**

In another way, a nested `catch` only catches failures in its scope and below, not errors higher up in the chain outside the nested scope. When used correctly, this gives greater precision in error recovery:

```

doSomethingCritical()
  .then(
    result => doSomethingOptional(result))
      .then(optionalResult => doSomethingExtraNice(optionalResult))
      .catch(e => {})
  )
  .then(() => moreCriticalStuff())
  .catch()

```

在上述代码中，The inner neutralizing catch statement only catches failures from doSomethingOptional() and doSomethingExtraNice(), after which the code resumes with moreCriticalStuff(). Importantly, if doSomethingCritical() fails, its error is caught by the final (outer) catch only.


#### 1.3/6 chaining after a catch

It's possible to chain after a failure, which is useful to accomplish new actions even after an action failed in the chain:

```

new Promise((resolve, reject) => {
    console.log('Initial');

    resolve();
})
.then(() => {
    throw new Error('Something failed');
        
    console.log('Do this');
})
.catch(() => {
    console.error('Do that');
})
.then(() => {
    console.log('Do this, no matter what happened before');
});


Initial
Do that
Do this, no matter what happened before

```

