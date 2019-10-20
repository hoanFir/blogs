🐾 认识new和Object.create()

🕘 2019.10.19 由 hoanfirst 编辑


### new

```javascript

function new(func) {
  //通过Object.create()创建一个新的空对象，该新的对象原型指向构造函数原型对象
  var createObject = Object.create(func.prototype);
  
  //使用创建的空对象作为执行上下文(this)，执行构造函数
  var returnObject = func.prototype.constructor.call(createObject);
  
  if(typeof returnObject === 'object') return returnObject;
  else return createObject;
}

```


### Object.create()

```javascript

Object.create = function (prototypeObj) {
  return { '__proto__': prototypeObj };
}

```
