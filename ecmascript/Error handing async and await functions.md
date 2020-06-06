
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

### Option2 - Catch every await expression

```javascript


async function doubleAndAdd(a, b) {

  a = await doubleAfterSec(a).catch(e => console.log('"a" is NaN'));
  b = await doubleAfterSec(b).catch(e => console.log('"b" is NaN'));
  
  if(!a || !b) {
    return NaN;
  }
  
  return a+b;
}

doubleAndAdd('string', 2).then(console.log); // NaN, and logs '"a" is NaN'
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

### Option3 - Catch the entire async/await function

```javascript

async function doubleAndAdd(a, b) {

  [a, b] = await Promise.all([doubleAfterSec(a), doubleAfterSec(b)]);
  
  return a+b;
}

doubleAndAdd('string', 2)
  .then(console.log)
  .catch(console.log); //NaN
  
doubleAndAdd(1, 2)
  .then(console.log) // 6
  .catch(console.log);

function doubleAfterSec(param) {
  return new Promise((resolve, reject) => {
    setTimeout(function() {
      let val = param * 2;
      isNaN(val) ? reject(NaN) : resolve(val);
    }, 1000)
  })
}

```

