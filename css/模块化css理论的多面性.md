
## 一、OOCSS

OOCSS, Object-Oriented CSS.

```html

<div class="toggle simple">
  <div class="toggle-control open">
    <h1 class="toogle-title">title</h1>
  </div>
  <div class="toogle-details open">
    ...
  </div>
</div>

```

OOCSS 有两个主要的原则：分离结构和外观，以及分离容器和内容。

- 分离结构和外观

将视觉特性部分定义为可复用的单元。

如上述例子中，可以套用很多不同的外观样式，如 `simple` 替换成 `complex`。

- 分离容器和内容

不再将元素位置作为样式的限定词，使用可复用的 CSS 类名。

在上述例子中，使用 `toogle-title`，它可以应用于任何一个文本处理上，而不管这个文本的元素是什么。


> 有个比较好的例子就是 Bootstrap ，它是一个自带各种皮肤的组件系统。



## 二、SMACSS

SMACSS, Scalable and Modular Architecture for CSS.

模块化架构的可扩展CSS。

```html

<div class="toggle toggle-simple">
  <div class="toggle-control is-active">
    <h1 class="toogle-title">title</h1>
  </div>
  <div class="toogle-details is-active">
    ...
  </div>
</div>
  
```

SMACSS将样式系统分了五个具体的类别：

- 基础

不添加 CSS 类名，标记以默认外观呈现。

- 布局

把页面分成一些区域。

- 模块

模块化、可复用的单元。

如上述例子中模块 `toggle`, `toggle-title`, `toggle-details`, 子模块 `toggle-simple`

- 状态

描述在特定条件下模块或布局的显示方式。

如上述例子中状态 `is-active`

- 主题

一个可选的视觉外观层，支持更换不同主题。

> 对于如何创建功能的小模块，OOCSS 和 SMACSS 有许多相似之处。它们都把样式作用域限定到根节点的 CSS 类名上，然后通 过皮肤(OOCSS)或者子模块(SMACSS)进行修改。两者之间最显著的差异是使用皮肤而不是子模块，以及带 is 前缀的状态类名。
