Map + Set + WeakMap + WeakSet

```javascript

//1
//Sets
var s = new Set();
s.add("hello").add("goodbye").add("hello");
s.size === 2;
s.has("hello") === true;

s === Set(2) {"hello", "goodbye"}

//2
//Maps
var m = new Map();
m.set("hello", 21);
m.set(s, 21);
m.get(s) === 21

//3
//Weak Maps
var wm = new WeakMap();
vm.set(s, { extra: 42 });
vm.size === undefined
vm.get(s) === { extra: 42 }

//WeakMaps provides leak-free object-key'd side tables
//WeakMaps 提供无泄漏的对象键的副表

//4
//Weak Sets
var ws = new WeakSet();
ws.add({ data: 21 });
//because the added object has no other references, it will not be held in the set

```

## Set

通过Set，

1. 创建
