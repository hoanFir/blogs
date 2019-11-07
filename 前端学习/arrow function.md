🐾 arrow function

🕘 2019.11.07 由 hoanfirst 编辑

### 一、箭头函数第一个用处是使得表达更为简洁

```javascript

var foo = () => 7;
var foo = param => param+1;
var foo = (param1, param2) => param1+param2;
var foo = id => ({ id: id });

var full = ({first, last}) => first+''+last; //结合解构赋值
const person = { first: xing, last: ming };
full(person);

[1, 2, 3].reduce((total, item)=> { total += item }); //简化回调函数
[1, 2, 4].map(x => x*x);
var result = [3, 2, 1].sort((a, b) => a-b);

const numbers = (...nums) => nums; //结合rest运算符
numbers(1, 2, 3);
const headAndTail = (head, ...tail) => [head, tail];
headAndTail(1, 2, 3);

```


### 箭头函数的第二个是解决this指向问题

在没有箭头函数之前，我们有一种场景，即用const that = this;来解决内部函数this的指向问题。

而箭头函数解决了上述问题，即完全修复了this的指向，this总是指向词法作用域，也就是定义时所处的作用域。

```javascript

var handler = { //this固定化+封装回调函数

  id: '12';

  init: function() {
    document.addEventListener('click', 
      event => this.dosth(e.type), false); //this总指向handler对象，否则回掉函数在运行时指向的是document对象
  },

  dosth: function(type) {
    console.log("handling"+type+' for '+this.id);
  }
}

```

### 使用注意

1. 箭头函数体内的this对象指向的是定义时所在的对象，因为已经按照词法作用域绑定了，所以不是执行时所在的作用域。

2. 因为箭头函数this指向是固定的，所以用call()或者apply()调用箭头函数时，无法对this进行绑定，即传入的第一个参数会指向外部函数的this。

3. 箭头函数是一个匿名函数，不可以作为构造函数，即不能通过new调用，会抛出一个错误。

4. 箭头函数体内使用arguments对象时获取到的是外部函数的arguments，因为箭头函数不存在arguments对象。如果要获取箭头函数传入的参数可以结合rest使用。

5. 箭头函数不可以用作Generator函数，也就不可以在函数体内使用yield。
