
ECMAScript 5 有两个缩小数组的方法：reduce() 和 reduceRight().

**1. reduce()**

从数组的第一项开始，逐个遍历。

**1. reduceRight()**

从数组的最后一项开始，向前遍历。


**3. 接收参数**

两个方法都接收两个参数：一个在每一项上调用的函数，另一个作为缩小基础的初始值。

函数参数接收四个参数：前一个值、当前值、项的索引、数组对象。该函数返回值会作为第一个参数自动传给下一项。因此**第一次迭代会发生在数组的第二个项上**。

**4. 用途**

示例1：求数组所有值之和

```javascript

var values = [1,3,2,4,5];

var sum = values.reduce(function(prev, cur, index, array) {
  return prev+cur;
});

sum = 15

```



