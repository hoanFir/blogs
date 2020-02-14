
### new

大多数引用类型值都是 Object 类型的实例，Object 是 ECMAScript 中使用最多的类型。

创建 Object 实例的方法有另种，一种是使用对象字面量表示法，另一种就是使用 new 操作符后跟 Object 构造函数，如下所示：

```javascript

var person = new Object();

person.name = "test";
person.age = 21;

```

new 操作符的本质是什么？

```javascript

function new(func) {

  //通过 Object.create() 创建一个新的空对象，该新的对象原型指向构造函数原型对象
  var createObject = Object.create(func.prototype);
  
  //使用创建的空对象作为执行上下文(this)，执行构造函数
  var returnObject = func.prototype.constructor.call(createObject);
  
  //返回实例
  if(typeof returnObject === 'object') {
    return returnObject;
  } else {
    return createObject;
  }
  
}

```


### Object.create()

```javascript

Object.create = function (prototypeObj) {
  return { '__proto__': prototypeObj };
}

```
