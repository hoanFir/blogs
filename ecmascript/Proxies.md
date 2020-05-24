
Proxies enable creation of objects with the full range of behaviors available to host objects.

换句话说，Proxies 允许创建对象，这些对象具有宿主对象可以使用的全部行为。

Proxies can be used for interception, object virtualization, logging(记录)/profiling(分析), etc.


```javascript

//1
//Proxying a normal object
var target = {};
var handler = {
  get: function(receiver, name) {
    return `hello, ${name}!`;
  }
};

var p = new Proxy(target, handler);
p.world === "hello, world!"


//2
//Proxying a function object
var target = function () {
  return "i am the target";
};
var handler = {
  apply: function (receiver, ...args) {
    return "i am the proxy";
  }
};

var p = new Proxy(target, handler);
p() === 'i am the proxy'

```

除了 `get` 和 `apply`，there are traps available for all of the runtime-level meta-operations

```javascript

var handler = {

  //1
  //target.prop
  get: ...,
  
  //2
  //target.prop = value
  set: ...,
  
  //3
  //delete target.prop
  deleteProperty: ...,
  
  //4
  //target(...args)
  apply: ...,
  
  //5
  //new target(...args)
  construct: ...,
  
  //6
  //Object.getOwnPropertyDescriptor(target, 'prop')
  getOwnPropertyDescriptor: ...,
  
  //7
  //Object.defineProperty(target, 'prop', descriptor)
  defineProperty: ...,
  
  //8
  //Object.getPrototypeOf(target)
  //Reflect.getPrototypeOf(target)
  //target.__proto__
  //object.isPrototypeOf(target)
  //object instanceof target
  getPrototypeOf: ...,
  
  //9
  //Object.setPrototypeOf(target)
  //Reflect.setPrototypeOf(target)
  setPrototypeOf: ...,
  
  //10
  //for (let i in target) {}
  enumerate: ...,
  
  //11
  //Object.keys(target)
  ownKeys: ...,
  
  //12
  //Object.preventExtensions(target)
  preventExtensions: ...,
  
  //13
  //Object.isExtensible(target)
  isExtensible: ...,
}

```


