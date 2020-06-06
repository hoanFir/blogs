
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

### 二、合并数组并去重

```javascript

var a = [1, 2, 3];
var b = [3, 4, 5];

function combine() {
  let arr = [].concat.apply([], arguments);
  
  return Array.from(new Set(arr)); //关键代码
  
  //new Set 用于创建没有重复值的集合
  //from 用于转化成真数组
}

combine(a, b);

```

