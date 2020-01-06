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


接着，add函数没法继续拆分，它是一个最基础的`Identifier（标志）对象`，用来作为函数的唯一标志。可以得到如下一个`FunctionDeclaration对象`：

```javascript

{
  name: "add",
  type: "identifier",
  params: ...,
  body: ...,
  ...
}
```


对`params`继续拆分，其实是两个`Identifier`组成的数组：

```javascript

[
  {
    name: 'a',
    type: 'identifier',
    ...
  },
  {
    name: 'b',
    type: 'identifier',
    ...
  }
]
```


继续对`body`进行拆分，它是一个`BlockStatement（Block域）对象`，里面包含一个`ReturnStatement（Return域）对象`，即`return a+b`。

继续拆分`ReturnStatement对象`，里面包含一个`BinaryExpression（二项式）对象`，即`a+b`。

继续拆分`BinaryExpression对象`，分为`left`、`operator`和`right`三部分。left包含`Identifier对象`a，right包含`Identifier对象`b，operator包含+。

最后，我们通过分层树状对形式来展示各部分，就得到抽象语法🌲了。



https://mp.weixin.qq.com/s/EnS22WGKiXnTCdFnqrVahA




