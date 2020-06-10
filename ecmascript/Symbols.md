Symbols allow properties to be keyed by either `string` or `symbol`. Symbols are unique, but not private since they are exposed via reflection features like `Object.getOwnPropertySymbols`.

栗子2：私有属性

```javascript

(function() {

  //module scoped symbol
  var key = Symbol("key");
  
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

```

