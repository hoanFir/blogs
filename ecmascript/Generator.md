
A function declared as `function*` return a Generator instance.


```javascript

var fibonacci = {
  [Symbol.iterator]: function*() {
    var pre = 0, cur = 1;
    
    for(;;) {
      var temp = pre;
      pre = cur;
      cur += temp;
      
      yield cur;
    }
  }
}

for(var n of fibonacci) {
  if(n > 1000) break;
}

```
