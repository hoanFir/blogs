
```javascript

switch (expression) {
  case value: 
    /* 合并情形 */
  case value: 
    statement
    break;
  case value: 
    statement
    break;
  default: 
    statement
    break;    
}

```

ECMAScript 中的 switch 具有自身特色，首先，可以在 switch 语句中使用任何数据类型，无论是字符串或者对象；其次，每个 case 值不一定是常量，可以是变量，甚至是表达式。

注意，switch 语句在比较时使用的是恒等操作符，仅比较不转换。

示例：

```javascript

1）

switch ("hello world") {
  case "hello" + "world": //表达式
    ...
    break;
  default:
    ...
    break;
}

2）

var num = 15;
switch (true) {
  case num < 0: //表达式
    ...
    break;
  case number >= 0 && num <= 10: //表达式
    ...
    break;
  default:
    ...break;
}



```
