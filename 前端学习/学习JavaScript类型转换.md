🐾 学习JavaScript类型转换

🕘 2019.10.19 由 hoanfirst 编辑

### JavaScript数据类型

1. primitive data type\[7\]

基本类型

A primitive is a data that is not an object and has no methods.

There are 7 primitive data types: `string`, `number`, `boolean`, `null`, `undefined`, `symbol`, `bigint`.

All primitives are immutable, they cannot be altered.

2. primitive wrapper objects\[3\]

基本封装类型

Except for `null` and `undefined`, all primitive values have object equivalents that warp around the primitive values.

`String`, `Number`, `Boolean`, `Symbol`, `BigInt`.

The wrapper's `valueOf()` will returns the primitive value.

3. object data type\[3\]

`Object`, `Array`, `Date`


### 判断数据类型

1. typeof

```txt

typeof "john"  -  string
typeof 3.14  - number
typeof NaN  -  number
typeof false  -  boolean
typeof Symbol('sym')  -  symbol
typeof BigInt(9007199254740991)  -  bigint
typeof [1,2,3,4]  -  object
typeof {name:'john', age:34}  -  object
typeof new Date()  -  object
typeof function () {}  -  function
typeof null  - object
typeof someValue  -  undefined

```

2. constructor.toString()

```txt

"john".constructor.toString()  -  function String() { [native code] }
(3.14).constructor.toString()  -  function Number() { [native code] }
false.constructor.toString()  -  function Boolean() { [native code] }
Symbol('sym').constructor.toString()  -  function Symbol() { [native code] }
BigInt(9007199254740991).constructor.toString()  -  function BigInt() { [native code] }
[1,2,3,4].constructor.toString()  -  function Array() { [native code] }
{name:'john', age:34}.constructor.toString()  -  function Object() { [native code] }
new Date().constructor.toString()  -  function Date() { [native code] }
function () {}.constructor.toString()  -  function Function() { [native code] }

tips: Except for null and undefined, all primitive values have object equivalents that warp around the primitive values.

```


### JavaScript类型转换

JavaScript 类型转换主要有两种方式：

1. 显示的类型转换

2. 隐式的类型转换/自动转换


### 显示的类型转换

1. String()、Number()、Boolean()

当不通过`new`运算符调用这些函数时，它们会作为类型转换函数。

```txt

String(12.1)  -  '12.1'
String(1+2)  -  '3'
String(NaN)  -  'NaN'
String(false)  -  "false"
String(Symbol('sym'))  -  "Symbol(sym)"
String(BigInt(9007199254740991))  -  "9007199254740991"
String([1,2,3,4])  -  "1,2,3,4"
String({name:'john', age:34})  -  "[object Object]"
String(new Date())  -  "Thu Nov 14 2019 14:24:39 GMT+0800 (中国标准时间)"
String(function () {})  -  "function () {}"
String(null)  -  "null"
String(undefined)  -  "undefined"

Number(" 3 ")  -  3
Number(" ")  -  0
Number("")  -  0
Number(false)  -  0
Number(true)  -  1
Number(Symbol('sym'))  -  Uncaught TypeError: Cannot convert a Symbol value to a number
Number([1,2,3,4])  -  NaN
Number({name:'john', age:34})  -  NaN
Number(new Date())  -  1573715061130
Number(function () {})  -  NaN
Number(null)  -  0
Number(undefined)  -  NaN

Boolean(1)  -  true
Boolean(0)  -  false
Boolean(-1)  -  true
Boolean(Symbol('sym'))  -  true
Boolean([1,2,3,4])  -  true
Boolean([])  -  true
Boolean({name:'john', age:34})  -  true
Boolean(new Date())  -  true
Boolean(function () {})  -  true
Boolean(null)  -  false
Boolean(undefined)  -  false

```


tips: Number()的不足

```txt

1. Number(value)

对于Integer，Number()只能基于十进制进行转换；不能出现非数字内容（空字符除外）以及不能在数字中间出现空字符（如"88 99"），否则返回NaN。

推荐使用parseInt()或parseFloat()，只有在数字之前有非数字内容（空字符除外）才会返回NaN（如" $23"），而且只会转换第一次找到的数值内容，之后的字符会自动忽略（如" 12 $33"=12）。

2. parseInt(string, radix)

解析一个字符串，返回一个整数。

如果字符串前缀是“0x”或“0X”，自动以16进制数进行解析。

return a integer that is the string argument taken as a number in the specified radix. (For example, a radix of 10 converts from a decimal number, 8 converts from octal, 16 from hexadecimal, and so on. 

3. parseFloat(string)

解析一个字符串，返回一个浮点数。

return a floating point number.

```

2. toString()

toString()执行结果和String()一样。

除了`null`或`undefined`之外的任何值，都具有toString()方法。

3. Object()

当不通过`new`运算符调用Object()时，它会作为类型转换函数。

```txt

Object("text")  - new String ("text")
Object(3)  -  new Number(3)
Object(Symbol('sym'))  -  new Symbol('sym')
Object([1,2,3,4])  -  true
Object([])  -  true
Object({name:'john', age:34})  -  true
Object(new Date())  -  true
Object(function () {})  -  true

Object(null)  -  {}
Object(undefined)  -  {}

```


tips:

```txt

我们知道，Except for null and undefined, all primitive values have object equivalents that warp around the primitive values.

所以当null或undefined用在期望是一个对象的地方时会造成一个类型错误异常，但是，使用Object(undefined)或Object(null)得到的是一个新对象：

Object(undefined)  -  {}
Object(null)  -  {}

```


### 灵活的类型转换

下面简要说明了再JavaScript中如何进行类型转换('-'表示不必要也没有执行转换)：

值|转换为字符串|转换为数字|转换为布尔值|转换为对象|
-|-|-|-|-|
undefined|"undefined"|NaN|false|throws TypeError|
null|"null"|0|false|throws TypeError|
true|"true"|1|-|new Boolean(true)|
false|"false"|0|-|new Boolean(false)|
""|-|0|false|new String("")|
"1.11"|-|1.11|true|new String("1.11")|
"string"|-|NaN|true|new String("string")|
0|"0"|-|false|new Number(0)|
-0|"0"|-|false|new Number(-0)|
NaN|"NaN"|-|false|new Number(NaN)|
Infinity|"Infinity"|-|true|new Number(Infinity)|
-Infinity|"-Infinity"|-|true|new Number(-Infinity)|
1|"1"|-|true|new Number(1)|
{}(任意对象)|参考下一节|参考下一节|true|-|
\[\](空数组)|""|0|true|-|
\[9\](1个数字元素的数组)|“9”|9|true|-
\[‘a’\](其他数组)|参考下一节|参考下一节|true|-
function(){}(任意函数)|参考下一节|参考下一节|true|-

- null和undefined用在期望是一个对象的地方会造成一个类型错误异常
- 字符串中含有数字，但在开始和结尾处的任意非空字符，会转换成NaN
- true转换为1，false转换为0
- 转换为false：undefined、null、0、-0、NaN、“”
- 原始值到对象的转换非常简单，通过调用String()、Number()、Boolean()构造函数转化成各自的包装对象
- **对象到原始值到转换**（重点）



### 对象到原始值到转换

《JavaScript权威指南(第6版)》3.8.3
toPrimitive()

1、对象->布尔值

所有的对象（包括数组和函数、包装对象）都转换为true。

2、对象->字符串/对象->数字

通过调用`toPrimitive()`来转换。该方法接受两个参数，指定要转换成原始值的对象和指定转换顺序。

转换顺序：因为JavaScript对象有两个不同的方法来执行转换，`toString()`和`valueOf()`，通过不同参数值，可以指定先调用执行转换，当对象执行方法后返回的还不是原始值，则调用另一个方法。


### ==运算符的类型转换

《JavaScript权威指南(第6版)》4.9.1

注意，===恒等运算符在判断相等时并未做任何类型转换。


### +双目运算符、+单目运算符的类型转换

1、+双目运算符

如果一个操作数是字符串，将其另一个操作数转换为字符串。

x+"test" => String(x)+"test"

2、+单目运算符

将操作数转换为数字。

+x => Number(x)


### !单目运算符的类型转换

将操作数转换为布尔值并取反。

!!x => Boolean(x)




