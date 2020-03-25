
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

#### 3.1 elements of different types

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

When comparing two React DOM elements of the same type, React looks at the attibutes of both, keeps the same underlying DOM node, and only updates the changed attributes.

For example:

```

<div className="hide">
</div>

<div className="show">
</div>

//only modify the className on the underlying DOM node


<div style={{ color: "blue", fontWeight: 'bold' }}>
</div>

<div style={{ color: "grey", fontWeight: 'bold' }}>
</div>

//only modify the color style, not the fontWeight


```

#### 3.3 component elements of the same type

When a component updates, the instance stays the same, so that state is maintained across renders.

React updates the props of the underlying component instance to match the new element, and calls `componentWillReceiveProps()` and `componentWillUpdate()` on the underlying instance. Next, the `render()` is called and **the diff algorithm recurses on the previous result and the new result**.


#### 3.4 recursing on children

- by default

when recursing on the children of a DOM node, React just **iterates over both lists of children at the same time and generates a mutation** whenever there's a difference.

For example:

```

//before
<ul>
  <li>1</li>
  <li>2</li>
</ul>


//after
<ul>
  <li>1</li>
  <li>2</li>
  <li>3</li>  
</ul>

//add an element at the end of the children after converting between these two trees

//React will match the two <li>1</li> trees, match the two <li>2</li> trees, and then insert the <li>3</li> tree.

```

But, when inserting an element at the  beginning, converting between these two tress works poorly:

```

//before
<ul>
  <li>one</li>
  <li>two</li>
</ul>


//after
<ul>
  <li>three</li>
  <li>one</li>
  <li>two</li>  
</ul>

//add an element at the start of the children

//React will mutate every child, because it can not realize it can keep the <li>one</li> and <li>two</li>

```

- 改进 / keys

React supports a `key` attribute.

When children have keys, React uses the key to match children in the original tree with children in the subsequent tree.

For example:

```

//before
<ul>
  <li key="one">one</li>
  <li key="two">two</li>
</ul>


//after
<ul>
  <li key="three">three</li>
  <li key="one">one</li>
  <li key="two">two</li>  
</ul>

//add an element at the start of the children

//React knows that the element with key 'three' is the new one, and the elements with key 'one' and 'two' have just moved

```

tips: As a last resort, you can pass an item'index in the array as a key. This can work well if the items are never reordered, but renders will be slow.


### 四、总结

在当前的实现中，一旦组件类型不同，就会重新生成一个组件；组件类型相同时，状态不会丢失，会进行比较更新部分内容；对于一颗子树，它能在其兄弟之间移动，但不能移动到其他位置。





