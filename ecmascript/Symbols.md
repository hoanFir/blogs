## 特性

- Symbols allow properties to be keyed by either `string` or `symbol`. 

- Symbols are unique. But not private since they are exposed via reflection features like `Object.getOwnPropertySymbols`.


```javascript

const symbol1 = Symbol();
const symbol2 = Symbol();
 
symbol1 === symbol2; // false
 
const obj = {};
obj[symbol1] = 'Hello';
obj[symbol2] = 'World';
 
obj[symbol1]; // 'Hello'
obj[symbol2]; // 'World'

```

## 场景1：Symbol.iterator

In order to be iterable, an **object** must implement the `@@iterator` method, meaning that the object must have a property with `@@iterator` key which is available via constant `Symbol.iterator`. **为什么是 symbol 而不是 string 类型？**

因为如果只是简单的 string 类型，很容易被覆盖：

```javascript

class MyClass {
  constructor(obj) {
    Object.assign(this, obj);
  }
 
  iterator() {
    const keys = Object.keys(this);
    let i = 0;
    return (function*() {
      if (i >= keys.length) {
        return;
      }
      yield keys[i++];
    })();
  }
}

//故意传递一个同名属性
const obj = new MyClass({ iterator: 'not a function' });

```

symbol 的好处就在于：清楚的分割用户数据和对象内置数据。



## 场景2：私有属性

因为任意两个 symbol 都不相等，symbol 可以方便的模拟私有属性：

```javascript

(function() {

  //module scoped symbol
  const key = Symbol("key");
  
  function MyClass(privateData) {
    this[key] = privateData;
  }
  
  MyClass.prototype.doStuff = function() {
      ... this[key] ...
  };
  
  //limited support from Babel, full support requires native implementation
  typeof key === "symbol"
})();

var c = new MyClass("hello");

c["key"] === undefined
Object.keys(c) === []
c[Symbol('key')] === undefined


const [symbol] = Object.getOwnPropertySymbols(c);
c[symbol] === 'hello'

```

因为 Symbols 不会在 Object.keys() 中出现，没有任何代码可以访问到这个属性，除非使用  Object.getOwnPropertySymbols() 方法。
