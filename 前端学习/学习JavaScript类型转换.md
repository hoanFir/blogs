🐾 学习JavaScript类型转换

🕘 2019.10.19 由 hoanfirst 编辑

### 一、灵活的类型转换

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



### 二、对象到原始值到转换

《JavaScript权威指南(第6版)》3.8.3
toPrimitive()

1、对象->布尔值

所有的对象（包括数组和函数、包装对象）都转换为true。

2、对象->字符串/对象->数字

通过调用`toPrimitive()`来转换。该方法接受两个参数，指定要转换成原始值的对象和指定转换顺序。

转换顺序：因为JavaScript对象有两个不同的方法来执行转换，`toString()`和`valueOf()`，通过不同参数值，可以指定先调用执行转换，当对象执行方法后返回的还不是原始值，则调用另一个方法。


### 三、==运算符的类型转换

《JavaScript权威指南(第6版)》4.9.1

注意，===恒等运算符在判断相等时并未做任何类型转换。


### 四、+双目运算符、+单目运算符的类型转换

1、+双目运算符

如果一个操作数是字符串，将其另一个操作数转换为字符串。

x+"test" => String(x)+"test"

2、+单目运算符

将操作数转换为数字。

+x => Number(x)


### 五、!单目运算符的类型转换

将操作数转换为布尔值并取反。

!!x => Boolean(x)


### 六、显示类型转换

1、使用Boolean()、Number()、String()

当不通过new运算符调用这些函数时，它们会作为类型转换函数。

如：Number("3")=>3、String(false)=>"false"、Boolean(\[\])=>true

注意，使用Number()只能基于十进制进行转换，并且不能出现开头和结尾含非空字符(非法的数字直接量)，返回NaN。

此时更推荐使用parseInt和parseFloat。第二个参数指定数字转换的基数，开头为非空字符(非法的数字直接量)，返回NaN，结尾的非空字符会忽略。

parseInt()：只解析整数。如果字符串前缀是“0x”或“0X”，将其解析为16进制数。
parseFloat()：解析整数和浮点数。

2、Object()

Object(3)=>new Number(3)；

注意，null和undefined用在期望是一个对象的地方会造成一个类型错误异常，但是使用Object(undefined)或Object(null)得到的是一个新对象，不会抛出异常。

3、使用toString()

除了null或undefined之外的任何值，都具有toString()方法;

toString()执行结果和String()一样；

