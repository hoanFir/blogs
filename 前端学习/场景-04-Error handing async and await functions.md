
### Option1 - Use try catch within the function 🌟🌟🌟

因为 async/await 执行是同步的，因此可以配合 try/catch。

```javascript


async function doubleAndAdd(a, b) {

  try {
    [a, b] = await Promise.all([doubleAfterSec(a), doubleAfterSec(b)]);
  } catch (e) {
    return NaN;
  }
  
  return a+b;
}

doubleAndAdd('string', 2).then(console.log); // NaN
doubleAndAdd(1, 2).then(console.log); // 6

function doubleAfterSec(param) {
  return new Promise((resolve, reject) => {
    setTimeout(function() {
      let val = param * 2;
      isNaN(val) ? reject(NaN) : resolve(val);
    }, 1000)
  })
}

```

### Option2 - Catch every await express

### Option3 - Catch the entire async/await function
