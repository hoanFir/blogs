🐾 arrow function

🕘 2019.11.07 由 hoanfirst 编辑


Arrows are a function shorthand using the `=>` syntax.

```javascript

//1
//expression bodies
var odds = evens.map(v => v+1);
var nums = evens.map((v, i) => v + i);

//2
//statement bodies
nums.forEach(v => {
  if(v % 5 === 0) {
    fives.push(v);
  }
});

//3
//lexical this
var bob = {
  _name: "bob",
  _friends: [],
  printFriends() {
    this._friends.forEach(f => {
      console.log(this._name + ' knows ' + f);
    });
  }
}

//4
//lexical arguments
function square () {
  let example = () => {
    let numbers = [];
    for(let number of arguments) {
      numbers.push(number * number);
    }
    
    return numbers;
  };
  
  return example();
}

...

```

### 一、箭头函数第一个用处是使得表达更为简洁

```javascript

var foo = () => 7;
var foo = param => param+1;
var foo = (param1, param2) => param1+param2;
var foo = id => ({ id: id });

var full = ({first, last}) => first+''+last; //结合解构赋值
const person = { first: xing, last: ming }; full(person);

[1, 2, 3].reduce((total, item) => { total += item }); //简化回调函数
[1, 2, 4].map(x => x*x);
var result = [3, 2, 1].sort((a, b) => a-b);

const numbers = (...nums) => nums; //结合rest运算符
numbers(1, 2, 3);

const headAndTail = (head, ...tail) => [head, tail]; //结合rest运算符
headAndTail(1, 2, 3);

```


### 二、箭头函数的第二个是解决this指向问题

在没有箭头函数之前，我们有一种场景，即用`const that = this;`来解决内部函数this的指向问题。

通过箭头函数可以不用那么麻烦，因为它修复了该场景this的指向。Arrows share the same lexical `this` as their surrounding code. 也就是说，箭头函数本身没有 `this`，其继承外部函数。

```javascript

var handler = { //this固定化+封装回调函数

  id: '12';

  init: function() {
    document.addEventListener('click', 
      event => this.dosth(e.type), false); //this总指向handler对象，否则默认情况下回调函数在运行时指向document对象
  },

  dosth: function(type) {
    console.log("handling"+type+' for '+this.id);
  }
}

```

### 三、使用注意

1. 箭头函数体内的this对象指向的是定义时所在的对象，因为已经按照词法作用域绑定了，所以不是执行时所在的作用域。

2. 因为箭头函数this指向是固定的，所以用call()或者apply()调用箭头函数时，无法对this进行绑定，即传入的第一个参数会指向外部函数的this。

3. 箭头函数体内使用arguments对象时获取到的是外部函数的arguments，因为箭头函数不存在arguments对象。如果要获取箭头函数传入的参数可以结合rest运算符使用

4. 箭头函数不可以作为构造函数，即不能通过new调用，会抛出一个错误

5. 箭头函数不可以用作Generator函数，也就不可以在函数体内使用yield


### 四、箭头函数this的固定是什么含义？

固定**①从自己的作用域链的上一层继承this 或者 ②指向定义时所处的对象**。而不能理解为定义时就固定this对象的值。

如下例子中，箭头函数外层普通函数this发生了变化，它也会跟着变化。

```javascript

var obj = {
    a: 1,
    getA: function(){
        //innerGet：普通函数，即不作为对象的属性时，this会指向window
        var innerGet = function(){
            console.log("1", this);
            
            (() => {
            	console.log("2", this)
            })()
        };
      
        innerGet();
      
        return innerGet;
    }
};

obj.getA()() //都是指向window
obj.getA().apply(obj) //都是指向obj

```
