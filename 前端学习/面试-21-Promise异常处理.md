

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
