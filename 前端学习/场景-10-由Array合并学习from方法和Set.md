
### 一、合并数组

```javaScript

var a = [1, 2, 3];
var b = [3, 4, 5];

//1
//扩展运算符(注意，不是rest运算符)
var c = [...a, ...b];


//2
//concat
var c = a.concat(b);
var c = [].concat.apply([], [a, b]);

//3
//push(影响原数组a)
a = [].push.apply(a, b);

```

