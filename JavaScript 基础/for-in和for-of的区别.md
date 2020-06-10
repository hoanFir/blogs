
**The main difference between them is in what they iterate over.**

- for-in

```

The for-in statement iterates over the enumerable properties/属性 of an object.

注意，以随机顺序遍历。

```


- for-of

```

The for-of statement creates a loop iterating over iterable objects: built-in String, Array, array-like objects(e.g., arguments or NodeList), TypedArray, Map, Set, and user-defined iterables.

for-in 调用一个custom iteration hook，其中包含针对对象的每个不同属性的 value/值 执行的语句。

```

## 遍历对象

```javascript

var s = { a: 1, b: 2, c: 3 };

for(let prop in s) {
  if(s.hasOwnProperty(prop)) {
    console.log(prop); // a b c
    console.log(s[prop]); // 1 2 3
  }
}

for(let prop of s){
    console.log(prop); // Uncaught TypeError: s is not iterable 
}

for(let prop of Object.keys(s)){
    console.log(prop); // a b c
     console.log(s[prop]); // 1 2 3
}

```




## 遍历数组

```

Object.prototype.objCustom = function() {}; 
Array.prototype.arrCustom = function() {};

const iterable = [3, 5, 7];
iterable.foo = 'hello';

for (const i in iterable) {
  console.log(i); // logs 0, 1, 2, "foo", "arrCustom", "objCustom"
}

for (const i in iterable) {
  if (iterable.hasOwnProperty(i)) {
    console.log(i); // logs 0, 1, 2, "foo"
  }
}

for (const i of iterable) {
  console.log(i); // logs 3, 5, 7
}

```

## 遍历字符串

```

const iterable = 'boo';

for (const value of iterable) {
  console.log(value); // "b" "o" "o"
}

```

