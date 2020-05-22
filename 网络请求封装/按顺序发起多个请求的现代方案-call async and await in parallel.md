
## 方案：sequential async/wait

```javascript

function resolveAfter2Seconds() {
  return new Promise(resolve => {
    setTimeout(function() {
      resolve("slow")
      console.log("slow promise is done")
    }, 2000)
  })
}

function resolveAfter1Second() {
  return new Promise(resolve => {
    setTimeout(function() {
      resolve("fast")
      console.log("fast promise is done")
    }, 1000)
  })
}

//1
//sequential
async function sequentialStart() {
  const slow = await resolveAfter2Seconds()
  console.log(slow) //this runs 2 seconds after 

  const fast = await resolveAfter1Second()
  console.log(fast) //this runs 3 seconds after
}

```

在上述代码中，同时调用两次 await 来获取数据，但是这样需要等待两个请求的时间。

In sequentialStart, the second timer is not created until the first has already fired, so the code finishes after 3 seconds.



## 改进方案A：concurrent async/await

```javascript

//2
//concurrent
async function concurrentStart() {
  const slow = resolveAfter2Seconds() //starts timer immediately
  const fast = resolveAfter1Second() //starts timer immediately
  
  console.log(await slow) //this runs 2 seconds after
  console.log(await fast) //this runs 2 seconds after, immediately because fast is already resolved
}

```

In concurrentStart, both timers are created and then awaited. The timers run concurrently, which means the code finishes in 2 seconds.

However, the await calls still run in series, which means the second await will wait for the first one to finish. 


## 改进方案B：Promise.all()

```javascript

//3
//concurrent
function concurrentPromise() {
  return Promise.all([resolveAfter2Seconds(), resolveAfter1Second()]).then((messages) => {
    console.log(messages[0]) // slow
    console.log(messages[1]) // fast
  })
  //same as concurrentStart
}

```

## 方案：parallel = async/wait + Promise.all()

If we wish to safely perform two or more jobs in parallel, we must await a call to Promise.all, or Promise.allSettled.

```javascript


async function parallel() {
 
  await Promise.all([
      (async ()=>console.log(await resolveAfter2Seconds()))(),
      (async ()=>console.log(await resolveAfter1Second()))()
  ])
  
  //truly parallel: after 1 second, logs "fast", then after 1 more second, "slow"
}

```
