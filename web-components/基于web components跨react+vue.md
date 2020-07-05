
[Web Compoennt](https://developer.mozilla.org/en-US/docs/Web/Web_Components)

### 一、react and Web Component

[React and Web Components](https://reactjs.org/docs/web-components.html)

React and Web Components are built to solve different problems:

- Web Components provide strong encapsulation for reusable components

- React provides a declarative library that keeps the DOM in sync with your data


React and Web Components' goals are complementary（互补）.

As a developer, you are free to use React in your Web Components, or to use Web Components in React, or both. Most people who use React don’t use Web Components, but you may want to, especially if you are using third-party UI components that are written using Web Components.


### 二、vue and Web Component

Vue 组件非常类似于**自定义元素**——它是 Web Component 规范的一部分，这是因为 Vue 的组件语法部分参考了该规范。例如 Vue 组件实现了 Slot API 与 `is` attribute。

但是，Vue and Web Components 还是有几个关键差别：

Web Component 规范已经完成并通过，但未被所有浏览器原生实现。目前 Safari 10.1+、Chrome 54+ 和 Firefox 63+ 原生支持 Web Components。相比之下，Vue 组件不需要任何 polyfill，并且在所有支持的浏览器 (IE9 及更高版本) 之下表现一致。必要时，Vue 组件也可以包装于原生自定义元素之内。

Vue 组件提供了纯自定义元素所不具备的一些重要功能，最突出的是**跨组件数据流**、**自定义事件通信**以及**构建工具集成**。

虽然 Vue 内部没有使用自定义元素，不过在应用使用自定义元素、或以自定义元素形式发布时，依然有很好的互操作性。Vue CLI 也支持将 Vue 组件构建成为原生的自定义元素。
