
A `Promise` allows you to associate handlers with an asynchronous action's eventual success value or failure reason. This lets asynchronous methods return values like synchronous methods: **instead of immediately returning the final value, the asynchronous method returns a promise to supply the value at some point in the future**.

A `Promise` is in one of these states:

- pending: initial state
- fulfilled: 
- rejected:


### 三、properties of Promise

- Promise.length: the number of constructor arguments(always 1)

- Promise.prototype: the Promise constructor

着重理解 Promise.prototype：

1. Promise.prototype.constructor

2. Promise.prototype.then(onfulfilled, onrejected) 返回一个新 Promise，并以回调的返回值来执行新Promise的onresolve

3. Promise.prototype.catch(onrejected) = then(null, onrejected)

4. Promise.prototype.finally(onfinally) 该handler会在promise运行完毕后调用，不管状态是fulfilled还是rejected

另外，由于Promise.prototype.then和Promise.prototype.catch方法返回的是promise，所以可以被链式调用。

```javascript

doSomething1().then(function(result) {
  return doSomething2();
}).then(function(newResult) {
  return doSomething3(newResult);
}).then(function(finalResult) {
 //...
}).catch(failureCallback);
```

### 四、methods of Promise

- Promise.all(iterable)
- Promise.allSettled(iterable)：当所有promise都执行完，即使失败
- Promise.race(iterable)：当任意一个promise成功或失败时就返回
- Promise.reject(reason)
- Promise.resolve(value)

着重理解 Promise.all：

Promise.all常被用于处理多个promise对象的状态集合，如只有所有请求正常才成功显示页面，否则只显示loading。

```javascript

Promise.all([fetch1, fetch2]).then((res) => {
  console.log(res); //[res1, res2]
}).catch((err) => {
  console.log(err);
})
```

该方法会返回一个新的Promise对象，该Promise对象在iterable参数中所有promise对象都成功的时候才会触发成功，一旦任何一个promise对象失败就会触发该Promise对象的失败。

当触发成功状态，该Promise会把一个包含iterable里所有promise返回值的数组作为成功回调的返回值，顺序跟iterable里的所有promise一致。

当触发失败状态，该Promise会把iterable里第一个触发失败的promise对象的凑无信息作为失败回调的返回值。

