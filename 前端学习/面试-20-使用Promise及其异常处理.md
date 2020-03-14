
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
  
  
#### 1.4 组合式

1、Promise.all() Promise.race()

这两种都是用于running asynchronous operations in parallel/并行执行异步操作的方法，

```javascript
//发起并行然后等所有操作全部结束才进行下一步操作
Promise.all([func1(), func2(),func3()])
  .then((arr)=>{})
//arr === [result1, result2, result3]

//当任意一个promise成功或失败时就返回
Promise.race()

```


2、Promise.resolve Promise.reject

这两种是手动创建一个已经resolve或reject的promise的快捷方式。

```javascript

//Sequential composition is possible using some clever JavaScript: 
//使用一些聪明的JavaScript写法来实现时序组合
//arr.reduce(function(total, currentValue), initialValue);
[func1, func2, func3].reduce((p, f) => p.then(f), Promise.resolve())
  .then(result3 => {})
//reduce an array of asynchronous functions down to a promise chain equivalent to：
Promise.resolve().then(func1).then(func2).then(func3);

//make it a reusable compose function, 这在函数式编程极为普遍
const applyAsync = (acc, val) => acc.then(val);
const composeAsync = (...funcs) => x => funcs.reduce(applyAsync, Promise.resolve(x));
const transformData = composeAsync(func1, func2, func3);
const result3 = transformData(data);

//时序组合还可以搭配asyn+await更简洁
let result;
for(const f of [func1, func2, func3]) {
  result = await f(result);
};
/* use last result(i.e. result3) */

```


注意，在使用Promise.resolve()或Promise.reject()时，即使是一个已经变成resolve状态的Promise，传递给then()的函数也总是会被异步调用：

```javascript

Promise.resolve().then(()=>console.log('1'));
console.log('2');
//输出：2 1

const wait = ms => new Promise(resolve => setTimeout(resolve, ms));
wait().then(() => cosole.log('1'));
Promise.resolve().then(()=>console.log('2')).then(()=>console.log('3'));
console.log('4');
//输出：4 1 2 3

```


### 二、异常处理

通常在 Promise 链式调用中一遇到异常，promise 链就会停下来，直接调用catch回调来处理异常。

```
dosomething1()
  .then(result1 => dosomething2(result1))
  .then(result2 => dosomething3(result2))
  .then(finalresult => { /*...*/ })
  .catch(failureCallback)
  
//上面捕获异常的方法和以下代码类似：

try {
  let result1 = syncDoSomething1(); //同步
  let result2 = syncDoSomething2(result1); //同步
  let finalResult = syncDoSomething3(result2); //同步
  
  /*...*/
} catch(error) {   
  failureCallback(error); 
}

//注意，上述代码中 try...catch 捕获的是同步操作的异常

//如果要用于异步的 Promise，可以引入 async+await：

async function foo() {
  try {
    let result1 = await dosomething1();
    let result2 = await dosomething2(result1); 
    let finalresult = await dosomething3(result2); 
  } catch(error) {
    failureCallback(error);
  }
}

```


#### 1、认识 promise rejection events/promise 拒绝事件：

whenever a promise is rejected, one of two events is sent to the global scope（generally, this is either the window or, if being used in a web worker, it’s the Worker or other worker-based interface）.

1. rejectionhandled: Sent when a promise is rejected, and rejection function has been handled(在reject函数处理该rejection之后才派发)

2. unhandledrejection: Sent when a promise is rejected, but there is no rejection handler available

对于以上两种情况，the event（of type PromiseRejectionEvent）有两个属性：promise和reason。前者指向被reject的promise，后者说明被reject的原因。

而且，这两个处理是全局的（window.addEvnetListener(‘a/b’)），因此所有的错误都会调用相同的event handlers。因此make it possible to offer fallback error handling for promises/为promise失败时提供补偿处理.

```javascript
window.addEventListener('unhandledrejection', e => {
  //e.promise
  //e.reason
  e.preventDefault();
})
```

#### 2、普通场景：

```javascript
try {
  //抛出
} catch(e) {
  //捕获和处理
} finally {}
try…catch在异步场景中不能使用。
```

#### 3、Promise 中的异常处理：

第一种直接通过onrejected，适用于单个请求

```javascript

getData = (url, option) => {
  return new Promise(function(resolve, reject) {
    axios.get(url, {params: option}).then(function(res) {
      if(200) resolve(res); 
      else reject(res.message);
    })
  })
}

function app () {
  getData.then((res)=> {
    console.log("data", res);
  }, (err)=>{
    alert(err);
  })
}
```

第二种是多个请求的场景

```javascript
tips：catch(onrejected) == then(null, onrejected)
```
```javascript

//promise.all
Promise.all([fetch1, fetch2]).then(res=>{
  cosole.log(res);
}).catch(err=>{
  console.log(err)
})

//promise chain
getData().then(res=>{
  return getData2();
}).then(res=>{
  return getData3();
}).then(res=>{
  console.log(res)
}).catch(err=>{
  console.log(err)
})
```

第三种是try…catch搭配新特性async+await使用

```javascript
const Axios = {
  get: (url, option) => {
    return new Promise(function(resolve, reject) {
      axios.get(url, {params: option}).then(function(res) {
        resolve(res);
      }).catch(function() {})
    })
  }
}

async loadData() {
  try {
    const data = await Axios.get(url, option);
    //...
  } catch (err) {
    console.log(err);
  }
}
```

### 三、提一下promise嵌套

嵌套 Promise 是一种可以限制 catch 语句的作用域的控制结构写法。

嵌套的 catch 仅捕捉在其之前同时还必须是其作用域内的 failureres，而捕捉不到在其链式以外或者其嵌套域以外的 error。

如果使用正确，那么可以实现高精度的错误修复。

```javascript

dosthCritical()
  .then(res => dosthOptional()
    .then(optionalRes => dosthExtra(optionalRes))
    .catch(e => { console.log(e.message) }) //即使有异常也会继续执行，最后会输出。该catch仅能捕获dosthOptional和dosthExtra的失败，而且之后会回复到dosthCritical的执行。
  )
  .then(() => dosthCritical2())
  .catch(e => console.log("critical failure: " + e.message)) //当嵌套的内部失败，不会有输出；如果是dosthCritical失败，会捕获到。
  
```
