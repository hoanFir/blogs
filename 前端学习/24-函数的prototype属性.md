
在 ECMAScript 核心所定义的全部属性中，最耐人寻味的就是 prototype 属性。

对于 ECMAScript 中的引用类型来说，prototype 是保存它们所有实例方法的真正所在。**换句话说，诸如 toString() 和 valueOf() 等方法实际上都保存在 prototype 里，只不过通过各自对象的实例访问罢了**。

在 ECMAScript 5 中，prototype 属性是不可枚举的。

tips：在创建自定义引用类型以及实现**继承**时，prototype 属性的作用时极为重要的。

### 理解构造函数的原型对象

在第29节中，我们谈到`原型模式`来创建自定义对象，其中第一步就是要理解构造函数的原型对象，prototype。

我们创建的每一个函数（一般是构造函数），都有一个 prototype 属性，这个属性是一个指针，指向一个对象。该对象的用途是：保存可以由特定类型的所有实例共享的属性和方法。

换句话说，prototype 就是，通过调用构造函数而创建的那个对象实例的原型对象。

用`原型模式`创建了自定义的构造函数后，其原型对象 prototype，默认会先取得 constructor 属性，该属性指向自定义的构造函数。当调用构造函数创建一个新实例后，该实例的内部将包含一个指针（内部attribute，用双中括号表示，\[\[Prototype\]\]），指向构造函数的原型对象。

关于这个 \[\[Prototype\]\]，虽然在脚本中没有标准的方式访问它，但 Firefox、Chrome 和 Safari 浏览器在每个对象上都支持一个 `__proto__` 属性。而在其他实现中，该属性对脚本⬆️完全不可见的。

上述关系如图：

![](https://github.com/hoanFir/blogs/blob/master/%E5%89%8D%E7%AB%AF%E5%AD%A6%E4%B9%A0/images/%E6%88%AA%E5%B1%8F2020-02-16%E4%B8%8B%E5%8D%887.22.52.png?raw=true)

