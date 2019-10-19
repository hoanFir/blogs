🐾 学习JavaScript类型转换

🕘 2019.10.19 由 hoanfirst 编辑

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
[]|""|0|true|-|
[9](1个数字元素的数组)|“9”|9|true|-
[‘a’](1个字符元素的数组)|使用join()|NaN|true|-
function(){}(任意函数)|参考下一节|NaN|true|-

- null和undefeated用在期望是一个对象的地方会造成一个类型错误异常

- 字符串中如果含有数字，且在开始和结尾处的任意非空字符，会转换成NaN

- true转换为1，false转换为0

- 原始值到对象的转换非常简单，通过调用String() Number() Boolean()构造函数转化成各自的包装对象

- 对象到原始值到转换（重点）


### 对象到原始值到转换

