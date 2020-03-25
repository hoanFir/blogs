
When using `render()` function to create a tree of React elements. On the next state or props update, that `render()` function will return a different tree of React elements. **Then React needs to figure out how to efficiently update the UI to match the most recent tree**.

### 一、O(n^3) algorithms

There are some generic solutions to this algorithmic of generating **the minimum number of operations** to transform one tree into another.

However, many algorithms have a complexity in the order of O(n^3) where n is the number of elements in the tree. If we used this in React, displaying 1000 elements would require in the order of one billion **comparisons**. This is far too expensive.


### 二、O(n) algorithm

React implements a heuristic O(n) algorithm based on two assumptions:

1. two elements of different types will produce different trees

2. the developer can hint at which child elements may be stable across different renders with a `key` prop

In practice, these assumptions are valid for almost all practical use cases.


### 三、The diffing algorithm

When diffing two trees, React first compares the two **root elements**. 

The behavior is different depending on the types of the root elements: 

#### 3.1 the root elements have different types

wheneven the root elements have different types, React will tear down the old tree and build the new tree form scratch. When tearing down a tree, old DOM nodes are destroyed. Component instances receive `componentWillUnmount()`. When building up a new tree, new DOM nodes are inserted into the DOM. Component instances receive `componentDidMount()`. Any state associated with the old tree is lost.

tips: any components below the root will also get unmounted and have their state destroyed.

For example:

```

<div>
  <Other />
</div>

<span>
  <Other />
</span>

//destroy the old Other and remount a new one

```



#### 3.2 dom elements of the same type






