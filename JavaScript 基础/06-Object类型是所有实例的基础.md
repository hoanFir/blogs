
Object 类型是所有它的实例的基础。

换句话说，Object 类型所具有的任何属性和方法也同样存在于更具体的对象中。

但是注意，从技术角度上，ECMA-262 中对象的行为不一定适用于 JavaScript 中的其他对象，如浏览器环境中的 BOM 和 DOM 中的对象，这些都属于宿主对象，由宿主实现和定义。

Object 的每个实例都具有以下属性和方法：

**1. Constructor**

保存用于创建当前对象的函数。


**2. hasOwnProperty(propertyName)**

检查给定的属性在当前对象实例中是否存在。注意，不会检查在实例的原型中是否存在。


**3. isPrototypeOf(object)**

检查给定的对象是否是当前对象实例的原型。


**4. propertyIsEnumerable(propertyName)**

检查给定的属性是否能够被枚举到。


**5. toLocalString()和toString()**

toLocalString 返回对象的字符串表示。注意，该字符串与执行环境的地区相对应。

toString 返回对象的字符串表示。


**6. valueOf()**

返回对象的字符串、数值或布尔值表示。注意，该方法的返回值通常与 toString() 相同。
