
When using `render()` function to create a tree of React elements. On the next state or props update, that `render()` function will return a different tree of React elements. **Then React needs to figure out how to efficiently update the UI to match the most recent tree**.

### 一、O(n^3) algorithms

There are some generic solutions to this algorithmic of generating **the minimum number of operations** to transform one tree into another.

However, many algorithms have a complexity in the order of O(n^3) where n is the number of elements in the tree. If we used this in React, displaying 1000 elements would require in the order of one billion **comparisons**. This is far too expensive.


### 二、O(n) algorithm

React implements a heuristic O(n) algorithm based on two assumptions:

1. two elements of different types will produce different trees

2. the developer can hint at which child elements may be stable across different renders with a `key` prop

In practice, these assumptions are valid for almost all practical use cases.


### The diffing algorithm











