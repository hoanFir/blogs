🐾 学习JavaScript四种主要继承

🕘 2019.10.10 由 hoanfirst 编辑

JavaScript继承：

- 基于原型链的继承
- 基于构造函数的继承
- 组合式继承
- class继承

### 基于原型链的继承

关键：`子类的原型对象为父类的一个实例`。

```javascript
function Father(name, age) {
  tihs.name = name;
  this.age = age;
  this.arr = [1, 2, 3];
}
Father.prototype.setName = function() {} 
Father.prototype.setAge = function() {}

function Son(price) {
  this.price = price;
  this.setPrice = function ()
}

//继承
Son.prototype = new Father();

//注意，在子类添加方法时必须要放在上面一条语句之后，因为其因为会改变原型对象的指向
Son.prototype.setOther = function() {}

//应用
var son1 = new Son(1000);
var son2 = new Son(2000);

//输出
son1 === {
  price: 1000
  setPrice: f()
  __proto__: Father
    age: undefined
    name: undefined
    arr: [1, 2, 3]
    setOther: f()
    __proto__:
      setAge: f()
      setName: f()
      constructor: f Father(name, age)
      __proto__: Object
}
son2 === {
  price: 2000
  setPrice: f()
  __proto__: Father
    age: undefined
    name: undefined
    arr: [1, 2, 3]
    setOther: f()
    __proto__:
      setAge: f()
      setName: f()
      constructor: f Father(name, age)
      __proto__: Object
}

分析：
Son.__proto__访问到Son.prototype也就是Father的实例，可以获取到父类定义的属性
Son.__proto__.__proto__访问到Father.prototype，可以获取到父类原型对象上的方法

```

存在的问题：
- 来自原型对象的所有引用数据类型的属性被所有实例共享，操作其属性值时会相互影响。

```javascript
son1.arr.push(4);
son1.arr === son2.arr; //true

son1.__proto__ === son2.__proto__
son1.__proto__.__proto__  === son2.__proto__.__proto__
```

- 无法实现多继承

- 创建子类实例时，无法向父类构造函数传参。


### 基于构造函数的继承

关键：`在子类的构造函数中通过call()来调用父类的构造函数`。

```javascript
function Father(name, age) {
  this.name = name;
  this.age = age;
}
Father.prototype.setName = function () {}
Father.prototype.setAge = function () {}

function Son(name, age, price) {
  //继承
  Father.call(this, name, age); //this.name = name, this.age = age 

  this.price = price;
  this.setPrice = function() {}
}

//应用
var son1 = new Son('xx', 20, 1000);

//输出
son1 = {
  age: 20
  name: 'xx'
  price: 1000
  setPrice: f()
  __proto__: Object
}

```

好处：

- 解决了基于原型链的继承中子类实例共享父类引用数据类型的问题。

- 支持多继承，即可以call多个父类

- 支持向父类传递参数

存在的问题：

- 只能继承父类的实例属性和方法，不能继承原型对象上的属性和方法。
- 由于不能继承原型对象上的属性和方法，也导致父类只能将方法定义在this上，即实例上，这样导致无法实现函数共用（定义在原型上共用），每个子类都有父类方法的副本，影响性能。

### 组合式继承

是最完美的继承方法：

- 子类继承了父类所有的属性和方法，而且父类方法可以定义在原型对象上，支持函数复用

```javascript
function Father(name, age) {
  this.name = name;
  this.age = age;
}
Father.prototype.setName = function () {}
Father.prototype.setAge = function () {}

function Son(name, age, price) {
  Father.call(this, name, age);
  this.price = price;
  ths.setPrice = function()
}

//核心代码1
Son.prototype = Object.create(Father.prototype);
//Object.create = function (Father.prototype) {   
//  return { '__proto__': Father.prototype }; 
//} 

//Q：为什么不用 Son.prototype = new Father()
//A：造成调用两次父类构造函数，生成两份实例

//Q：那为什么不用Son.prototype = Father.prototype
//A：虽然不会初始化两次父类构造函数，但没办法辨别对象是子类还是父类实例化，因为子类和父类的构造函数会指向同一个

//核心代码2
//实现了子类constructor指向子类构造函数
Son.prototype.constructor = Son;

var son1 = new Son('xxx', 20, 1000);
son1 instanceof Son //true
son1 instanceof Person //true
son1.constructor //Son
 
//输出
son1 = {
  age: 20
  name: 'xxx'
  price: 1000
  setPrice: f()
  __proto__: Father
    constructor: f Son(name, age, price)
    __proto__:
      setName: f()
      setAge: f()
      constructor: f Father(name, age)
      __proto__: Object
}

```

### ES2015的class extends继承

```javascript

class Father {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
  showName() {}
}
class Son extends Father {
  constructor(name, age, price) {
    super(name, age);
    this.price = price;
  }
  showName () {}
}
let son1 = new Son('xx', 18, 1000);
//输出
son1 = {
  age: 18
  name: 'xxx'
  price: 1000
  __proto__: Father
    constructor: class Son
    showName: f showName()
    __proto__:
      constructor: class Father
      showName: f showName()
      __proto: Object
}

class Father {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
  showName() {}
}
class Son extends Father {
  constructor(name, age, price) {
    super(name, age);
    this.price = price;
  }
}
let son1 = new Son('xx', 18, 1000);
//输出
son1 = {
  age: 18
  name: 'xxx'
  price: 1000
  __proto__: Father
    constructor: class Son
    __proto__: 
      constructor: class Father
      showName: f showName()
      __proto__: Object
}

```

很明显，比之前的组合式继承要清晰和方便很多。


### es2015 class => es5

```javascript
//es2015
class Father {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
  showName() {}
}

//es5
"use strict":

var Father = function () {
  function Father(name, age) {
    _classCallCheck(this, Father);
    
    this.name = name;
    this.age = age;
  }
  
  _createClass(Father, [{
    key: 'showName',
    value: function showName() {}
  }])
  
  return Father;
}

//只能通过new来实例化，不能随处调用
function _classCallCheck(instance, Constructor) {
  if (!_instanceof(instance, Constructor)) {
    throw new TypeError('cannot call a class as a function');
  }
}
function _instanceof(instance, Constructor) {
  if(Constructor != null && typeof Symbol !== 'undefined' && Constructor[Symbol.hasInstance]) {
    return !!Constructor[Symbol.hasInstance](instance);
  } else {
    return instance of Constructor;
  }
}

//定义在原型对象上的方法或定义在class上的静态属性/方法
function _createClass(Constructor, protoProps, staticProps) {
  if(protoProps) {
    _defineProperties(Constructor.prototype, protoProps);
  }
  if(staticProps) {
    _defineProperties(Constructor, protoProps);
  }
  return Constructor;
}
function _defineProperties(target, props) {
  for(var i=0;i<props.length;i++) {
    var descriptior = props[i];
    descriptor.enumerable - descriptor.enumerable || false;
    descriptor.configurable = true;
    if("value" in descriptor) {
      descriptor.writable = true;
    }
    
    Object.defineProperty(target, descriptor.key, descriptor);
  }
}


```

### es2015 class extend => es5

```javascript

//es2015
class Son extends Father {
  constructor(name, age, price) {
    super(name, age);
    this.price = price;
  }
  showName () {}
}

=>

//es5
var Son = function(_Father) {
  //将Son.__proto__指向Father()
  _inherits(Son, _Father);
  
  function Son(name, age, price) {    
    _classCallCheck(this, Son);
    
    var _this;
    //执行Father()
    //如果没有执行super()，this参数就不存在，会报错
    _this = _possibleConstructorReturn(this, _getPrototypeOf(Son).call(this, name, age));
    
    //after super();
    _this.price = price;
    return _this;
  }
 
  _createClass(Son, [{
    key: 'showName',
    value: function showName() {}
  }])
  
  return Son;  
}(Father)

function _inherits(subClass, superClass) {
  if(typeof superClass !== function && superClass !== null) {
    throw new TypeError("Super expression must either be null or a function");
  }
  
  subClass.prototype = Object.create(superClass && superClass.prototype, {
    constructor: {
      value: subClass,
      writable: true,
      configurable: true,
    }
  })
 
  if(superClass) {
    _setPrototypeOf(subClass, superClass);
  }
}
function _setPrototypeOf(o, p) {
  _setPrototypeOf = Object.setPrototypeOf || function _setPrototypeOf(o, p) { o.__proto__ = p; return o; }
  
  return _setPrototypeOf(o, p);
}

function _getPrototypeOf(Son) {
  _getPrototypeOf = Object.setPrototypeOf ? Object.getPrototypeOf : function _getPrototypeOf(Son) {
    return Son.__proto__ || Object.getPrototypeOf(Son);
  }
  
  return _getPrototypeOf();
}

function _possibleConstructorReturn(self, call) {
  if(call && (_typeof(call) === "object" || typeof call === 'function')) {
    return call;
  }
  
  return __assertThisInitialized(self);
}
function __assertThisInitialized (self) {
  if(self === void()) {
    throw new ReferenceError("this hasn't been initialised - super() hasn't been called");
  }
  return self;
}

```


### 组合式继承 vs class extends

1、组合式继承

- 借用`Father.call(this, name, age);`，当`new Son(name, age, price)`创建子类实例时，首先会调用父类的构造函数，实现子类继承父类的实例属性和方法

- 再借用`Son.prototype = Object.create(Father.prototype);`，由`Object.create(F.prototype) === return { __proto__ = F.prototype }`，得到子类的__proto__变成`__proto__: { __proto: F.prototype }`，这样就实了现子类原型包含父类原型中的属性和方法

- 最后把子类的__proto__里的constructor指向Son

2、 ES2016 class extends

- 和组合式继承实现的子类结构是一样的，根本上的区别在于es5中子类继承父类原型上的属性和方法是先实例化父类，而es6中是在实例化子类时继承父类原型

