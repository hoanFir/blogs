a 标签的点击事件有四个状态，分别是 link、visited、hover、active.

如果同时设置多个状态且不按照一定顺序，会让一些效果被覆盖。如：

先设置 hover，再设置 visited，会发现鼠标移上去后 a 的颜色是 grey.

```css

a:hover {
    color: blue;
}

a:visited {
    color: grey;
}

```

这是因为同样的选择器，定义同一个规则，后面的规则会覆盖前面的规则，这也是 css 之所以叫做层叠样式表的原因。

分析：

从用户体验来说，当鼠标悬浮在 a 标签上时要提供相应的样式变化从而让用户知道这是可以访问的超链接，当鼠标点击 a 标签未从松开时，也要提供相应的样式变化提示用户，最后，对于访问过的 a 标签要和未访问过的样式有区别。


解决办法：

```css

a:link {
    color: black;
}

a:visited {
    color: grey;
}

a:hover {
    color: blue;
}

a:active {
    color: yellow;
}

```

在上述代码中：

1. a 标签 在没有任何操作时具备 :link 的状态
2. 当 a 标签访问过，同时会具备 :link :visited 两种状态；由于访问过的 a 标签要用 :visited 表示，所以 :visited 应该在 :link 之后
3. 当 a 标签访问过，且鼠标悬浮到上面时，同时会具备 :link :visited :hover 三种状态；由于鼠标悬浮在 a 标签上要用 :hover 表示，所以 :hover 应该在 :link, :visited 之后
4. 当 a 标签访问过，且鼠标点击未松开时，同时会具备 :link :visited :hover :active 四种状态；由于鼠标点击 a 标签未松开要用 :active 表示，所以 :active 应该在 :link, :visited :hover 之后
