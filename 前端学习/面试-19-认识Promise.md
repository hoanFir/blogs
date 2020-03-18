
A `Promise` allows you to associate handlers with an asynchronous action's eventual success value or failure reason. This lets asynchronous methods return values like synchronous methods: **instead of immediately returning the final value, the asynchronous method returns a promise to supply the value at some point in the future**.

A `Promise` is in one of these states:

- pending: initial state
- fulfilled: meaning that the operation completed successfully
- rejected: meaning that the operation failed


### 一、Promise Contructor

The Promise Constructor is primarily used to wrap functions that do not already support promises.

`new Pomise(executor)`

- executor

A function that is passed with the arguments resolve and reject. The executor function is executed immediately by the Promise implementation, passing resolve and reject functions **(the executor is called before the Promise constructor even returns the created object)**. The executor normally initiates some asynchronous work, and then, once that completes, either calls the resolve function to resolve the promise or else rejects it if an error occurred. 

javascript demo:

```javaScript

const promise1 = new Promise(function(resolve, reject) {
  setTimeout(function() {
    resolve('foo');
  }, 300);
})

promise1.then(function(value) {
  console.log(value);
  // expected output: "foo"
});

```


### 二、Promise Properties

- Promise.length

number of constructor arguments(always 1)

- Promise.prototype

represents the prototype for the Promise constructor


### 三、Promise Methods

- Promise.all(iterable)

Wait for all promises to be resolved, or for any to be rejected. 

tips: Promise.all() 常被用于处理多个 promise 对象的状态集合，比如只当所有请求成功才显示页面，否则一直 loading。


- Promise.allSettled(iterable)

Wait until all promises have settled (each may resolve or reject) with an array of objects that each describe the outcome of each promise.

- Promise.race(iterable)

Wait until any of the promises is resolved or rejected.


- Promise.reject(reason)

Returns a new Promise object that is rejected with the given reason. **For debugging purposes and selective error catching, it is useful to make reason an instanceif Error**

```javascript

Promise.reject(new Error('fail')).then(function() {
  // not called
}, function(error) {
  console.error(error); // Stacktrace
});

```

- Promise.resolve(value)

Return a new Promise object that is resolved with the given value. 

value: Argument to be resolved by this Promise, **and also can be a Promise or a thenable to resolve.**

```javascript

//1
Promise.resolve('success').then(function(value) {
  console.log(value); // "Success"
}, function(value) {
  // not called
})


//2
//resolving an array
var p = Promise.resolve([1,2,3]);
p.then(function(v) {
  console.log(v[0]); // 1
});


//3
//resolving another Promise
var original = Promise.resolve(33);
var cast = Promise.resolve(original);
cast.then(function(value) {
  console.log('value: ' + value); //value: 33
});
console.log('original === cast ? ' + (original === cast)); //original === cast ? true


//4
//resolving thenables and throwing Errors


```


