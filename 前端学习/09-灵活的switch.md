
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

ECMAScript 中的 switch 具有自身特色，首先，可以在 switch 语句中使用任何数据类型，无论是字符串或者对象；其次，每个 case 值不一定是常量，可以是变量，甚至是表达式。

