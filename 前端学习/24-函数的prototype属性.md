
在 ECMAScript 核心所定义的全部属性中，最耐人寻味的就是 prototype 属性。

对于 ECMAScript 中的引用类型来说，prototype 是保存它们所有实例方法的真正所在。**换句话说，诸如 toString() 和 valueOf() 等方法实际上都保存在 prototype 里，只不过通过各自对象的实例访问罢了**。

在 ECMAScript 5 中，prototpe 属性是不可枚举的。

tips：在创建自定义引用类型以及实现**继承**时，prototype 属性的作用时极为重要的。



