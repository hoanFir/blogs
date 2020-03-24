🐾 shouldComponent和pureComponent

🕘 2020.03.24 由 hoanfirst 编辑


When a component's props or state change, React decides whether an actual DOM update is necessary by comparing the newly returned element with the previously rendered one. When they are not equal, React will update the DOM.

Even though React only updates the changed DOM nodes, re-rendering still some time. In many cases it's not a problem, but if the slowdown is noticeable, you can speed all of this up by **overriding the lifecycle function `shouldComponentUpdate`**.

### 一、shouldComponentUpdate

`shouldComponentUpdate` is triggered before the re-rendering process starts. In the lifecycle of React, `shouldComponentUpdate` is between `getDerivedStateFromProps` and `render`.

The default implementation of the function returns `true`, leaving React to perform the update:

```javascript

shouldComponentUpdate(nextProps, nextState) {
  return true;
}

```

So, if you know that in some situations your component doesn's need to update, you can return `false` instead, to skip the whole rendering process, including calling `render()` on this component and below.


### 二、PureComponent

In most cases, instead of writing `shouldComponentUpdate` by hand, you can inherit from `React.PureComponent`.

`React.PureComponent` is equivalent to implementing `shouldComponentUpdate` with a shallow comparsion of current and previous props and state.

Q：What difference between `React.PureComponent` and `React.Component`?

A：The difference is that `React.Component` doesn't implement `shouldComponentUpdate()`, but `React.PureComponent` implements it with a shallow prop and state comparison. If your component's `render()` function renders the same result given the same props and state, you can use `React.PureComponent` for a performance boost in some cases.



