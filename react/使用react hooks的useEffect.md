
## useEffect特性

赋值给useEffect的函数会在组件渲染到屏幕以后执行(即componentDidMoubt)，也会在每轮渲染结束后执行(即componentDidUpdate)。

注意，默认情况下它在第一次组件渲染和每次更新之后都会执行，而且可以控制。

该hook常用于在函数组件内（无状态组件）不允许改变DOM或数据获取或添加订阅或设置定时器或记录日志或执行其他包含副作用的操作。
