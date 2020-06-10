🐾 shouldComponent和pureComponent

🕘 2020.03.24 由 hoanfirst 编辑


When a component's props or state change, React decides whether an actual DOM update is necessary by comparing the newly returned element with the previously rendered one. When they are not equal, React will update the DOM.

Even though React only updates the changed DOM nodes, re-rendering still some time. In many cases it's not a problem, but if the slowdown is noticeable, you can speed all of this up by **overriding the lifecycle function `shouldComponentUpdate`**.

### 一、shouldComponentUpdate

`shouldComponentUpdate` is triggered before the re-rendering process starts.

The default implementation of the function returns `true`, leaving React to perform the update:

```javascript

shouldComponentUpdate(nextProps, nextState) {
  return true;
}

```

So, if you know that in some situations your component doesn's need to update, you can return `false` instead, to skip the whole rendering process, including calling `render()` on this component and below.

简单的说，如果不手动使用 `shouldComponentUpdate`，而默认返回 `true` 时，节点每次都会进行浅比较，比较为真执行`render()`，比较为假则跳过 `render()`；**如果手动使用 `shouldComponentUpdate` 且返回 `false`，直接跳过 `render()`，无须比较 vDOM**。

- 示例：

如果组件只有当 `props.color` 和 `state.count` 改变时才需要更新，此时可以使用 `shouldComponentUpdate` 来进行检查：只要这两个值没有改变，这个组件就不会更新。

```react

class CounterButton extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 1
    };
  }
  
  shouldComponentUpdate(nextProps, nextState) {
    if(this.props.color !== nextProps.color) { return ture };
    if(this.state.color !== nextState.color) { return ture };
    
    return false;
  }
  
  render() {
    return (
      <button
        color={this.props.color}
        onClick={()=>this.setState(state => ({ count: state.count+1 }))}>
        Count: { this.state.count }
      </button>
    );
  }
}

```


### 二、PureComponent

In most cases, instead of writing `shouldComponentUpdate` by hand, you can inherit from `React.PureComponent`.

`React.PureComponent` is equivalent to implementing `shouldComponentUpdate` with a **shallow comparsion** of current and previous props and state.

简单来说，在之前如果只是判断简单的几个值，可以在 `shouldComponentUpdate` 逐一判断，但是要比较值的个数一多，逐一判断不太现实，因此可以使用类似“浅比较”的模式来检查 props 和 state 中的所有字段，以此来决定组件是否需要更新。`React.PureComponent` 就是这个用途。

- 示例：

```react

class CounterButton extends React.PureComponent {
  constructor(props) {
    super(props);
    this.state = {
      count: 1
    };
  }

  render() {
    return (
      <button
        color={this.props.color}
        onClick={()=>this.setState(state => ({ count: state.count+1 }))}>
        Count: { this.state.count }
      </button>
    );
  }
}

```

注意，既然是“浅比较”，那么当 props 或 state 某种程度上是可变时，就会有所遗漏，也就不能用，这时候怎么办？

答案：使用拷贝后的值来setState。如：

```
this.setState(state => ({
  words: [...state.words, 'newWord'], //返回一个新数组
}))

this.setState(state => ({
  colormap = {...state.colormap, right: 'blue'}; //返回一个新对象
}))

this.setState(state => ({
  colormap = Object.assgin({}, state.colormap, {right: 'blue'}); //返回一个新对象
}))

```


### 三、What difference between `React.PureComponent` and `React.Component`

The difference is that `React.Component` doesn't implement `shouldComponentUpdate()`, but `React.PureComponent` implements it with a shallow prop and state comparison. If your component's `render()` function renders the same result given the same props and state, you can use `React.PureComponent` for a performance boost in some cases.


