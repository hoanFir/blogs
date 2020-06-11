
一般能用 interface 实现的场景就用 interface，type 相比功能会更多。

比如，可以联合多 type/interface，也可以声明元组：

```
//1
type test = {} & Dog;
type pet = Dog | Cat;

//2
type petList = [Dog, Cat];

```
