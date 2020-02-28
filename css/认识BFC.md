BFC，block formatting context，块格式化上下文。

BFC is a part of a visual css rendering of a web page. it’s the region(区域) in which the layout of block boxes occurs and in which float elements interact with other elements.

是 Web 页面的可视化 css 渲染的一部分，是 block boxes 布局过程产生的区域，也是浮动元素和其他元素交互的区域。

我们首先要知道，BFC 的应用主要在两种场景：

- 解决外边距折叠问题，margin collapsing。因为外边距折叠只会发生在属于同一个BFC的块级元素之间。


```

red inner

</div>

.blue .red-inner {
  height: 50px;
  margin: 10px 0;
}
.blue {
  background-color: blue;
}
.red-outer {
  overflow: hidden;
  background: red;
}

```

- 解决父元素因内部浮动而塌陷的问题。因为浮动定位和清除浮动只会应用于同一个BFC内的元素，浮动不会影响其他BFC元素中的布局，而清除浮动只能清除同一BFC中在它前面的元素的浮动。

  
```
i am a floated div.

  
i am content inside the box.

</div>

.box {
  background-color: rgb(224, 206, 247);
  overflow: auto; //创建一个新的BFC来包含浮动，告诉浏览器要如何处理溢出的内容
}
.float {
  float: left;
  width: 200px;
  height: 150px;
  background-color: white;
}

```

再者，我们还要知道触发生成BFC常见的有哪些？

- html 根元素
- 浮动元素：float (除none)
- 绝对定位元素：position(absolute, fixed)
- display 为 inline-block, table-cells, flex, inline-flex…
- overflow 为 hidden, auto, scroll，比较常用，但可能会遇到一些不想要的问题，如滚动条或一些剪切的阴影，需要注意。
…
