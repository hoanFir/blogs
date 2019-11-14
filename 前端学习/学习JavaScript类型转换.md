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
typeof NaN  -  number ⭐⭐⭐
typeof false  -  boolean
typeof Symbol('sym')  -  symbol
typeof BigInt(9007199254740991)  -  bigint
typeof [1,2,3,4]  -  object
typeof {name:'john', age:34}  -  object
typeof new Date()  -  object
typeof function () {}  -  function
typeof null  - object ⭐⭐⭐
typeof someValue  -  undefined ⭐⭐⭐

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
String(NaN)  -  'NaN' ⭐⭐⭐
String(false)  -  "false"
String(Symbol('sym'))  -  "Symbol(sym)"
String(BigInt(9007199254740991))  -  "9007199254740991"
String([])  -  "" ⭐⭐⭐
String([1,2,3,4])  -  "1,2,3,4" ⭐⭐⭐
String({name:'john', age:34})  -  "[object Object]" ⭐⭐⭐
String(new Date())  -  "Thu Nov 14 2019 14:24:39 GMT+0800 (中国标准时间)"
String(function () {})  -  "function () {}"
String(null)  -  "null"
String(undefined)  -  "undefined"

Number(" 3 ")  -  3 
Number(" ")  -  0 ⭐⭐⭐
Number("")  -  0 ⭐⭐⭐
Number(false)  -  0
Number(true)  -  1
Number(Symbol('sym'))  -  Uncaught TypeError: Cannot convert a Symbol value to a number
Number([1,2,3,4])  -  NaN
Number({name:'john', age:34})  -  NaN
Number(new Date())  -  1573715061130 ⭐⭐⭐
Number(function () {})  -  NaN
Number(null)  -  0 ⭐⭐⭐
Number(undefined)  -  NaN ⭐⭐⭐

Boolean(' ')  -  true ⭐⭐⭐
Boolean('')  -  fasle ⭐⭐⭐
Boolean(new String(''))  -  true/基本封装类型 ⭐⭐⭐
Boolean(1)  -  true
Boolean(0)  -  false
Boolean(new Number(0))  -  true/基本封装类型 ⭐⭐⭐
Boolean(NaN)  -  false ⭐⭐⭐
Boolean(-1)  -  true
Boolean(Symbol('sym'))  -  true
Boolean([1,2,3,4])  -  true
Boolean([])  -  true
Boolean({name:'john', age:34})  -  true
Boolean(new Date())  -  true
Boolean(function () {})  -  true
Boolean(null)  -  false ⭐⭐⭐
Boolean(undefined)  -  false ⭐⭐⭐

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

包括 primitive data type 到 object data type的转换。

```txt

Object("text")  - new String ("text")
Object(3)  -  new Number(3)
Object(Symbol('sym'))  - Symbol('sym')
Object([1,2,3,4])  -  new Array(1,2,3,4)
Object([])  -  new Array()
Object(new Date())  -  Thu Nov 14 2019 16:50:42 GMT+0800 (中国标准时间)
Object(null)  -  {} ⭐⭐⭐
Object(undefined)  -  {} ⭐⭐⭐

```

tips:

```txt

我们知道，Except for null and undefined, all primitive values have object equivalents that warp around the primitive values.

所以当null或undefined用在期望是一个对象的地方时会造成一个类型错误异常，但是，使用Object(undefined)或Object(null)得到的是一个新对象：

Object(undefined)  -  {}
Object(null)  -  {}

```

### 隐式的类型转换/自动转换

当 JavaScript 尝试操作一个“错误”的数据类型时，会自动转换为“正确”的数据类型。

```txt

1 + null  -  1

"1" + null  -  "1null"
"5" + 1  -  "51"

"5" - 1  -  4

document.getElementById("demo").innerHTML = something;  -  String(something)

```

- 1)i理解ToPrimitive(input, preferedType?)运行机制 ⭐⭐⭐

想要将object data type转换成primitive data type，必然会调用这个JavaScript内部函数。

《JavaScript权威指南(第6版)》3.8.3  -  `toPrimitive()`

`ToPrimitive`: Converts a JavaScript object to a primitive value. 

1、如果preferedType是number，会执行以下步骤：

```txt

1. 如果input是原始值，直接返回这个值

2. 否则，如果input是对象，调用input.valueOf()，如果结果是原始值，返回结果

3. 否则，调用input.toString()。如果结果是原始值，返回结果

4. 否则，抛出错误

```

2、如果preferedType是string，会执行以下步骤：

```txt

1. 如果input是原始值，直接返回这个值

2. 否则，调用input.toString()。如果结果是原始值，返回结果

3. 否则，如果input是对象，调用input.valueOf()，如果结果是原始值，返回结果

4. 否则，抛出错误

```

3、如果缺省preferedType

```txt

1. 如果是日期，会被认为是preferedType是string

2. 其他值，会被认为是preferedType是number

```


- 2)双目运算符+和单目运算符+的类型转换

1、+双目运算符

转换原则：

```txt

1. 如果操作数有一个是string（是对象的话得在对象通过ToPrimitive()转换之后），则将第二个操作数也转换成string。

2. 如果操作数都不是string，则两个操作数都转换成number。

示例：

> "1" + null
> "1null"
分析：因为有一个操作数为string，则第二个操作数也会转换成string，由于null是基本类型，则执行"1"+String(null)="1null"。

> "1"+{}
>"1[object Object]"
分析：因为有一个操作数是string，则第二个操作数也会转换成string，又因为{}是对象，则执行"1"+ToPrimitive({})='1[Object object]'。根据上面关于ToPrimitive()的知识，我们知道ToPrimitive({})会先求{}.valueOf()={}，再求{}.toString()="[Object object]"。

> 1+{}
> "1[object Object]"
分析：因为操作数{}经过ToPrimitive()转换后是string，所以另一个操作数1也要转换成string，即执行String(1)+ToPrimitive({})='1[Object Object]'。

> 1+[]
> "1"
分析：因为操作数[]经过ToPrimitive()转换后是string，所以另一个操作数1也要转换成string，即执行String(1)+ToPrimitive([])='1'。根据上面关于ToPrimitive()的知识，我们知道ToPrimitive([])会先求[].valueOf()=[]，再求[].toString()=""。

>[] + []
>ToPrimitive([])+ToPrimitive([])
>''+''
>''

>[]+{}
>ToPrimitive([])+ToPrimitive({})
>''+'[Object object]'
>'[Object object]'

>{}+[] ⭐⭐⭐
>0
疑问：为什么不是'[Object object]'+''???
分析：因为js引擎将第一个 {} 解释成一个空的代码块并忽略了它，所以这里的+会被看成单目运算符，即{}+[] = +[] = Number([]) = 0。


>{}+{} ⭐⭐⭐
>"[object Object][object Object]" 或 NaN
疑问：怎么又有两种结果???
分析："{}+{}"在Firefox运行会输出"NaN"。这是因为，js引擎将第一个 {} 解释成一个空的代码块并忽略了它，后面的+变成了一元运算符，即{}+{} = +{} = Number({}) = NaN。而在Chrome运行会输出"[object Object][object Object]"，因为它会自动补成({}+{})进行运算。

> 1 + null
> 1
分析：因为null和1都不是string，则执行Number(1)+Number(null)=1+0=1。

```

2、+单目运算符

将操作数转换为数字。

+x => Number(x)


- 3)比较运算符==的类型转换

《JavaScript权威指南(第6版)》4.9.1

转换原则：

```txt

1. 如果操作数有一个是对象，则会先通过ToPrimitive()转换之后，再跟另一操作数进行比较

2. 如果两个操作数分别String或Number或Boolean（包括对象转换成基本类型的操作数），都转换为Number再进行比较

3. 如果某个操作数为NaN，则比较结果必为false ⭐⭐⭐

4. 如果两个操作数都是null，则比较结果为true ⭐⭐⭐

5. 如果两个操作数都是undefined，则比较结果为true ⭐⭐⭐


示例：

> ''==[]
> ''==ToPrimitive([])
> ''==''
> true

>'[Object object]'=={}
>'[Object object]'==ToPrimitive({})
>'[Object object]'=='[Object object]'
>true

> 0==false
> 0==Number(false)
> 0==0
> true

> []==false ⭐⭐⭐
> ToPrimitive([])==false
> ''==false
> Number('')==Number(false)
> 0

```

注意，对于===恒等运算符，只有类型一致值也一致才会相等，并且在判断相等时不会做任何类型转换。


- 4)单目运算符!的类型转换

将操作数转换为布尔值并取反。


!x => Boolean(x)






