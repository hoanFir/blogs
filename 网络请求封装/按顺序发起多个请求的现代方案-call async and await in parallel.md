

```javascript

//Async functions themselves return a Promise
async function doubleAndAdd(a, b) {

  a = await doubleAfterSec(a);
  b = await doubleAfterSec(b);
  
  return a+b;
}

doubleAndAdd(1, 2).then(console.log);

```

在上述代码中，同时调用两次 await 来获取 a 和 b，但是这样需要等待双倍时间。

**方案：async/await + Promise.all()**

```javascript

async function doubleAndAdd(a, b) {

  [a, b] = await Promise.all([doubleAfterSec(a), doubleAfterSec(b)]);

  return a+b;
}

doubleAndAdd(1, 2).then(console.log);


```
