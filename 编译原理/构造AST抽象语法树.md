🐾 构造AST抽象语法树

🕘 2020.01.07 由 hoanfirst 编辑

:cupid: 原作：@刘羽冲

抽象语法树，Abstract Syntax Tree。

当你不止于想做一个前端工程师，而是希望自己也能写出像vue、react之类的大型框架，或像webpack、vue-cli、react-create-app等前端自动化的工具，或者其他工程需求，那你必须懂得AST。


### 接触JavaScript底层

通过学习AST和编译原理，可以深入理解JavaScript的运转远离。

首先，从一个简单的add函数开始。

```javascript

function add(a, b) {

  return a+b;
}

```

add函数没法继续拆分，它是一个最基础的`Identifier（标志）对象`，用来作为函数的唯一标志。可以得到如下一个`FunctionDeclaration对象`：

```javascript

{
  name: "add",
  type: "identifier",
  params: ["a", "b"],
  body: ...
  ...
}
```
