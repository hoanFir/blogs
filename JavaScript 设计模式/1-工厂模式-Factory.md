这种模式抽象了创建具体对象的过程，ECMAScript 实现：用一种函数来封装以特定接口创建对象的细节。

```javascript

function createPerson(name, age, job) {
  var o = new Object();
  
  o.name = name;
  o.age = age;
  o.job = job;
  
  o.sayName = function() {};
  
  return o;
}

var person1 = createPerson('', '', '')
var person2 = createPerson('', '', '')

```

存在的问题：工厂模式虽然解决了创建多个相似对象的问题，但却没有解决对象识别但问题，即怎么样知道一个对象的类型。具体创建对象的优化可以阅读[创建对象的发展历程](https://github.com/hoanFir/blogs/blob/master/JavaScript%20%E5%9F%BA%E7%A1%80/29-%E5%88%9B%E5%BB%BA%E5%AF%B9%E8%B1%A1%E7%9A%84%E5%8F%91%E5%B1%95%E5%8E%86%E7%A8%8B.md)



