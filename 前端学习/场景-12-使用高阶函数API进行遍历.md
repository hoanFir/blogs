### 一、

在eslint里有一条：

```
Don't use iterators. 

Prefer JavaScript's higher-order functions instead of loops like for-in or for-of

```

为什么？

```

this enforces our immutable rule.

dealing with pure functions that return values is easier to reason about than side effects.

即，符合开发者不想改变某些原数据的目的。

```

### 二、

那要用什么？

```

//遍历数组
map()
every()
filter()
find()
findIndex()
reduce()
some()
...

//遍历对象
Object.keys()
Object.values()
Object.entries()

to produce arrays and iterate with functions listed above.

```

示例1：

```javascript

const number = [1,2,3,4,5];

//bad
let sum = 0;
for(let num of numbers) {
  sum+=num;
}

//good
let sum = 0;
numbers.forEach(num => sum+num);

//best
const sum = numbers.reduce((total, num)=>total+num, 0);


```

示例2：

```javascript

const numbers = [1,2,3,4,5];

//bad
let incrementByone = [];
for(let i=0;i<numbers.length;i++) {
  incrementByone.push(numbers[i]+1);
}

//good
let incrementByone = [];
numbers.forEach(num => incrementByone.push(num+1));

//best
const incrementByone = numbers.map(num => num+1);

```


示例3：

```javasript

Object.keys(obj).forEach(key => {})
```

### 三、

Array.prototype.map()

```

map(): create a new array with the results of calling a provided function on every elements.

var newArray = arr.map(function callback(currentValue[, index[, array]]) {
  //return element for newArray 
}[, thisArg]);
```


Array.prototype.every()

```
every(): tests whether all elements in the array pass the test implemented by the provided function. it returns a Boolean value.

arr.every(callback(element[, index[, array]])[, thisArg]);
```

Array.prototype.filter()

```
filer(): creates a new array with all elements that pass the test implemented by the provided function.

var newArray = arr.filter(callback(element[, index[, array]])[, thisArg]);

eg:
const result = words.filter(word => word.length > 6);
```

Array.prototype.find()

```
find(): returns the value of the first element in the provided array that satisfies the provided testing function.

arr.find(callback(element[, index[, array]])[, thisArg]);

eg:
var array1 = [5, 12, 8, 130, 44];
var found = array1.find(function(element) {
   return element > 10;
});

```

Array.prototype.findIndex()

```
findIndex(): returns the index of the first element in the array that satisfies the provided testing function. otherwise, it return -1.

arr.findIndex(callback(element[, index[, array]])[, thisArg]);
```

Array.prototype.reduce()

```
reduce(): executes a reducers function on each element, resulting in a single output value.

arr.reduce(callback(accumulator, currentValue[, index[, array]])[, initialValue]);

eg:
const array1 = [1, 2, 3, 4];
const reducer = (accumulator, currentValue) => accumulator + currentValue;
array1.reduce(reducer)

```


Array.prototype.some()

```
some(): tests whether at least one element in the array passes the test implemented by the provided function. It returns a Boolean value.

arr.some(callback(element[, index[, array]])[, thisArg])
```
