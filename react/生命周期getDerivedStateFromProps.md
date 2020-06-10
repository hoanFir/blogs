
在 react 挂载时constructor、更新时new props/setState()/forceUpdate()，都会通过生命周期getDerivedStateFromProps.


## static getDerivedStateFromProps(props, state)

`getDerivedStateFromProps` is invoked right before calling the render method, both on the initial mount/挂载时 and on subsequent updates/更新时。

`getDerivedStateFromProps` should return an object to update the state, or null to update nothing.

`getDerivedStateFromProps` exists for rare use cases where **the state depends on changes in props over time.**

Deriving state 会导致代码冗长，并使组件难以理解。

