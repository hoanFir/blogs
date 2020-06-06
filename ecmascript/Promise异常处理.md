
### 一、Error propagation

If there's an exception, the browser will look down the chain for .catch() handlers or onRejected.

```javascript

doSomething()
.then(result => doSomethingElse(result))
.then(newResult => doThirdThing(newResult))
.then(finalResult => console.log(`Got the final result: ${finalResult}`))
.catch(failureCallback);

```

This is very much modeled after how synchronous code works:

```javascript

try {
  let result1 = syncDoSomething1(); //同步
  let result2 = syncDoSomething2(result1); //同步
  let finalResult = syncDoSomething3(result2); //同步
  
  /*...*/
  
} catch(error) {   
  failureCallback(error); 
}

```

如果要将 try/catch 用于异步的 Promise，可以引入 ECMAScript 2017 async/await：

```javascript

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



#### 二、Promise rejection events / PromiseRejectionEvent

whenever a promise is rejected, one of two events is sent to the global scope(i.e. window):

- **rejectionhandled**, Sent when a promise is rejected, and rejection function has been handled by the executor's `reject` function.

- **unhandledrejection**, sent when a promise is rejected, but there is no rejection handler available

add a handler for the `unhandledrejection` event:

```javascript

window.addEventListener("unhandledrejection", event => {
  /* You might start here by adding code to examine the
     promise specified by event.promise and the reason in
     event.reason */

  event.preventDefault();
}, false);

```

In both cases, the event (of type PromiseRejectionEvent) has two propertys:

- promise, indicating the promise that was rejected

- reason, provides the reason given for the promise to be rejected.

综上所述：These make it possible to offer fallback error handling for promises. These handlers are global per context, so all errors will go to the same event handlers, regardless of source.




#### 场景：

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

第三种是 try/catch 搭配新特性 async+await

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
