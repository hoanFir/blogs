
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


### 三、BEM

BEM, Block Element Modifier.

块元素修饰符。

```

<div class="toggle toggle--simple">
  <div class="toggle__control toggle__control--active">
    <h1 class="toogle__title">title</h1>
  </div>
  <div class="toogle__details toggle__details--active">
    ...
  </div>
</div>

```

BEM 其实只是一个 css 类名命名规则。它不涉及如何书写你的 CSS 的结构，而只是建议每个元素都添加带有如下内容的 CSS 类名：

- 块名

所属组件的名称，如 `toggle`

- 元素

组件里元素的名称，如 `toggle__title`

- 修饰符

任何与块或元素相关联的修饰符，如 `toggle__details--active`

> BEM 使用非常简洁的约定来创建 CSS 类名，而这些字符串可能会相当长。但每一个 CSS 类名都详细地描述了它实现了什么。

## 四、组合

当然，最重要的还是要找到一个适合的解决方案。不要因为一套规范很流行或者别的团队正在使用就选择它。比如使用 SMACSS 和 BEM 相混合的方案。



## 五、应用

在较早的 css，几乎总是从全局作用域开始开发，一层一层增加细节，如使用通用样式开始，比如页首和段落的标签，然后给页面里的各个部分应用更具体的样式：

```html

<body>
  <div class="main">
    <h2>I'm a Header</h2>
  </div>
  <div id="sidebar">
    <h2>I'm a Sidebar Header</h2>
  </div>
</body>

<style> 
  h2 {
    font-size: 24px;
    color: red;
  }
  #sidebar h2 {
    font-size: 20px;
    background: red;
    color: white;
  }
</style>

```

对于上述代码，如果有一个新需求，在侧边栏添加一个日历组件，并要求其内部 H2 显示的样式和页首一样，就会写成下面的代码：

```html

<body>
  <div class="main">
    <h2>I'm a Header</h2>
  </div>
  <div id="sidebar">
    <h2>I'm a Sidebar Header</h2>
    <div class="calendar">
      <h2>I'm a Calendar Header</h2>
    </div>
  </div>
</body>

<style> 
  h2 {
    font-size: 24px;
    color: red;
  }
  #sidebar h2 {
    font-size: 20px;
    background: red;
    color: white;
  }
  #sidebar .calendar h2 {
    background: none;
    color: red;
  }  
</style>

```

这样的开发模式存在很多问题：

- 选择器优先级

无论你处理带 ID 的标签还是长选择器，重写一个选择器时，总是需要注意它的优先级。 

- 颜色重置

要恢复到原来的 H2 颜色，我们必须再次指定样式，并且要覆盖当前的背景颜色。 

- 位置依赖

现在我们的日历样式依赖于侧边栏的样式。如果将日历移到页首或者页尾，样式将会改变。

- 多重继承

现在这个 H2 的样式来源多达三个，这意味着只要改变主体或侧边栏的样式，都会影响到日历的呈现。

- 深层嵌套

日历控件里的日历条目可能还有其他的 H2，而它们也需要一个更具体的选择器，这样 一来，H2 的样式来源就增加到了四个。


### 5.1 OOCSS

OOCSS 带来分离容器和内容的思想，我们学会不再使用位置作为样式的限定词。你完全 可以在网站上放一个侧边栏，并且给这个侧边栏使用任何你喜欢的样式，但是，侧边栏的样式不应该影响侧边栏的内容。

比如用 `.my-sidebar-widget-heading` 代替 `#sidebar h2`。`#sidebar h2`意味着，放在侧边栏的每一个H2元素，要么接受这个样式，要么就重写来避免使用这个样式。而 `.my-sidebar-widget-heading` 意味着 样式只限定于这一个标题，完全不会影响其他标题。

### 5.2 SMACSS

SMACSS 给我们带来把布局和组件分离到不同文件夹的思想，进一步将侧边栏的角色和日 历模块划分开。现在我们只是定义了侧边栏的角色是布局，甚至不允许元素样式在那部分 Sass 语法的代码里出现。如果你要在侧边栏里放一些代码，并且向它们添加样式，那么这 些元素需要是某个组件的一部分，并且需要在组件的文件夹里定义。

### 5.3 BEM
虽然严格来说，BEM 并不算是一种 CSS 方法论，但它让我们知道，给标记中每个 CSS 类 名一个独一无二的标识是有价值的。这是因为这样会使每个 BEM 风格的 CSS 类名都可以 对应到某一组独属于该元素的 CSS 属性，而不会随着具体情境或选择器的使用而变化:

```html

<body>
  <div class="main">
    <h2 class="content__title">"I'm a Header"</h2>
  </div>
  <div class="sidebar">
    <h2 class="content__title--reversed">"I'm a Sidebar Header" </h2>
    <div class="calendar">
      <h2 class="calendar__title">"I'm a Calendar Header"</h2>
    </div>
  </div>
</body>

<style>
/* 组件文件夹 */
.content__title {
  font-size: 24px;
  color: red; 
}
  
.content__title--reversed { 
  font-size: 20px; background: red;
  color: white;
}
  
.calendar__title {
  font-size: 20px;
  color: red;
}
  
/* 布局文件夹 */ 
.main {
  float: left; ...
}
  
.sidebar {
  float: right; ...
} 

</style>
```

这就解决了刚才的由于依赖位置而造成 CSS 样式混乱的问题。
