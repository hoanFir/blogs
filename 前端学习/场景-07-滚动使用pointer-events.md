pointer-events 是一个 CSS 属性，可以有多个不同的值，属性的一部分值仅仅与 SVG 有关联，这里我们只关注 pointer-events: none 的情况，大概的意思就是禁止鼠标行为，应用了该属性后，譬如鼠标点击，hover 等功能都将失效，即是元素不会成为鼠标事件的 target。

可以就近 F12 打开开发者工具面板，给 <body> 标签添加上 pointer-events: none 样式，然后在页面上感受下效果，发现所有鼠标事件都被禁止了。

那么它有什么用呢？

pointer-events: none 可用来提高滚动时的帧频。的确，当滚动时，鼠标悬停在某些元素上，则触发其上的 hover 效果，然而这些影响通常不被用户注意，并多半导致滚动出现问题。对 body 元素应用 pointer-events: none ，禁用了包括 hover 在内的鼠标事件，从而提高滚动性能。

```css

.disable-hover {
    pointer-events: none;
}

```

大概的做法就是在页面滚动的时候, 给 <body> 添加上 .disable-hover 样式，那么在滚动停止之前, 所有鼠标事件都将被禁止。当滚动结束之后，再移除该属性。

上面说 pointer-events: none 可用来提高滚动时的帧频 的这段话摘自 [pointer-events-MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/pointer-events) ，还专门有文章讲解过这个技术：

[使用pointer-events:none实现60fps滚动](https://www.thecssninja.com/javascript/pointer-events-60fps) 。
