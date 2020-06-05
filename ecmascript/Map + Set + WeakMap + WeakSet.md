## 一、Set

通过Set，可以创建没有重复值的集合。使用场景：实现数组合并 —— Array.from(new Set(arr))


## 二、WeakMap

The WeakMap object is a collection of key/value pairs in which **the keys are weakly referenced/弱引用**.

对于 WeakMap，其 key 必须是对象，value 可以是任意值。


Maps vs WeakMaps and why WeakMap？

```

在 JavaScript 里，Map API 可以通过共用两个数组(一个存放键,一个存放值)来实现。给这种 Map 设置值时会同时将键和值添加到这两个数组的末尾，从而使得键和值的索引在两个数组中相对应。当从该 Map 取值的时候，需要遍历所有的键，然后使用索引从存储值的数组中检索出相应的值。

但这样会有两个缺陷：

1. 查找键、更新指定键值等操作时间复杂度都为O(n)，因为需要遍历整个数组进行匹配

2. 可能导致内存泄漏。因为数组会一直引用每个键值对，这种引用使得垃圾回收算法不能回收处理他们



引入 WeakMap 之后，原生的 WeakMap 持有的是每个键对象的“弱引用”，这意味着在没有其他引用存在时垃圾回收能正确进行。原生 WeakMap 的结构是特殊且有效的，其用于映射的 key 只有在其没有被回收时才是有效的。正由于这样的弱引用，WeakMap 的 key 是不可枚举的 (没有方法能给出所有的 key)。如果key 是可枚举的话，其列表将会受垃圾回收机制的影响，从而得到不确定的结果。因此，如果你想要这种类型对象的 key 值的列表，你应该使用 Map。

基本上，如果你要往对象上添加数据，又不想干扰垃圾回收机制，就可以使用 WeakMap。

```


## 三、WeakSet
