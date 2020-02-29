
A Promise is an Object representing the eventual completion or failure of an asynchronous operation. Essentially, a promise is a returned object to which you attach callbacks, instead of passing callbacks into a funciton.

### 一、使用Promise

#### 1.1 使用 promise 的约定

老式：使用传入回调

```javascript

function successCallback(result) {}
function failureCallback(error) {}
createAsync(settings, successCallback, faliureCallback)
```

新式：返回一个promise对象

```javascript
createAsync(settings).then(successCallback, failureCallback)

//是以下的缩写
const promise = createAsync(settings)
promise.then(successCallback, failureCallback)

```

1、在使用Promise时，有如下约定：

- 回调在JavaScript 事件循环运行完成之前不会被调用（因此在循环里执行异步常常要搭配匿名闭包自执行来传入每次循环中不同的状态）

- 通过then()添加的回调总会被调用，即使它是在 success or failure of the asynchronous operation 完成之后才被添加

- 可以通过链式then()实现多个回调调用，这些回调会按照顺序一个个执行，因为then()会返回一个新promise（因此Promise最直接的好处是支持链式调用）

- 使用Promise链式编程要保持扁平化，要避免嵌套

#### 1.2 链式调用

老式：回调地狱

```javascript
dosomething(function(result) {
  dosomething2(result, function(result2) {
    dosomething3(result2, function(finalresult) {
      //...
    }, failureCallback)
  } , failureCallback)
} , failureCallback)
```


新式：Promise链式

```javascript
dosomething().then(result => dosomething2(result))
  .then(result2 => dosomething3(result2))
  .then(finalresult => { /*...*/ })
  .catch(failureCallback) //.then(null, failureCallback)
//通常在链中一遇到异常，promise链就会停下来，直接调用catch回调来处理异常。

//上面捕获异常的方法和以下代码类似：
try {   
  let result = syncDoSomething();   
  let newResult = syncDoSomethingElse(result);   
  let finalResult = syncDoThirdThing(newResult);   
  console.log(`Got the final result: ${finalResult}`); 
} catch(error) {   
  failureCallback(error); 
}

//因为 try...catch不能用于异步操作，所以我们在promise异常处理中提到了使用async+await搭配try...catch来捕获异常。代码如下：
async function foo() {
  try {
    let result = await dosomething();
    let result2 = await dosomething(result); 
    let finalresult = await dosomething(result2); 
  } catch(error) {
    failureCallback(error);
  }
}
```

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

#### 1.3 在旧式回调API外结合promise

混用旧式回调和promise可能会造成时序问题。而且对于旧式回调，如果失败或包含编程错误，没办法捕获。

请看下面代码：

```javascript
setTimeout(()=>console.log('1'), 0);
new Promise((resolve, reject)=>{
  console.log('2');
  resolve();
}).then(()=>console.log('3'))
  .then(()=>console.log('4'))
process.nextTick(console.log('5'));
console.log('6'); 

//输出结果：2 6 5 3 4 1
```

可以使用promise包裹旧式回调，不直接调用它。代码如下：

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

//示例2
const wait = ms => new Promise(resolve => setTimeout(resolve, ms));
//通过promise的构造器函数接受一个执行函数，并在执行函数内部手动resolve。

wait(10000)
  .then(()=>saysomething("10 seconds"))
  .catch(failureCallback)
  
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
