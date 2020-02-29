### 一、基本语法：

```

1. new Promise(executor);

executor: function((resolve, reject) {}
tips: the executor is called before the Promise constructor returns the created promise object.

resolve: calls the resolve function to resolve the promise(fulfilled status)
reject: calls the reject if an error occurred(rejected status)

2. 
promise.then(onfulfilled, onrejected) 
promise.catch(onrejected) 
promise.finally(onfinally)
```

### 二、基本概念：

Promise 对象使得异步方法可以像同步方法一样返回值，而不是通过传统的回调模式来执行异步逻辑。其不会立即返回执行结果，而是返回一个promise，其能代表未来出现的结果。

一个 Promise 有三种状态：pending，fulfilled，rejected，根据执行结果转化成对应状态，并执行 .then(onfulfilled, onrejected) 或者 .catch(onrejected) 和 .finally(onFinaly)

注意，一个promise处于fulfilled或rejected状态时，可以被称为settled状态或resolved状态。


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

### 五、理解 Promise constructor 的实现

Promise 构造函数用于包装还未支持 Promise 的函数。

```javascript

//new Promise(executor); 

function Promise(fn) {
  if(typeof this !== 'object') {
    throw new TypeError("Promie must be constructed via new")
  }
  if(typeof fn !== 'function') {
    throw new TypeError("Promie constructor\'s argument is not a function") 
  }

  this._deferredState = 0;
  this._state = 0; //0: pending, 1: fufilled, 2: rejected 
  this._value = null; //执行结果
  this._deferreds = []; //then() handlers arr
  if(fn === noop) return;

  doResolve(fn, this);
}

function doResolve(fn, this) {
  a = (value) => {
    resolve(this, value)
  }
  b = (reason) => {
    reject(this, reason)
  } 
  tryCallTwo(fn, a, b, this); //将处理resolve状态和rejected状态的回调函数传递到fn中
}

function tryCallTwo(fn, a, b, this) {
  try {
    fn(a, b);
  } catch (err) {
    b(this, err);
  }
}
```

...
更详细将在以后展开。
