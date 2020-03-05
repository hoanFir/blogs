🐾 flex

🕘 2019.10.28 由 hoanfirst 编辑


### 一、 什么时候会提出？主要目的是什么？

flex 在 2009 提出，主要目的是用来代替`传统盒状模型`。

传统盒状模型依赖 `display`,`position`,`float` 等属性，但其对于一些布局非常不方便，比如垂直居中。

flex, flexible box, 弹性布局.


### 二、 使用 flex 需要知道什么？

2.1 块级元素（flex）或行内元素（inline-flex）都可以使用 flex 布局

2.2 设置 `display: flex` 的元素（flex container）和其子元素（flex item）的 `float`, `clear`, `vertical-align` 等属性都将失效

2.3 flex 布局有两根轴，主轴（main axis）和交叉轴（cross axis）

2.4 单个 flex item 占据的主轴空间叫做 main size，单个 flex item 占据的交叉轴空间叫做 cross size


### 三、 flex container 有哪些重要属性？

- flex-direction

设置主轴的方向是水平或垂直

- flex-wrap 

设置 flex item 在一条轴线上放不下时怎么换行，默认不换行（flex: nowrap）。支持换行可以设置 `flex-wrap: wrap`

- justify-content

设置 flex items 在主轴上的位置，如 `flex-end`, `center`, `space-between`, `space-around`

- align-items

设置 flex items 在交叉轴上的位置，如 `stretch / 默认值 / item 高度为交叉轴高度`, `flex-end`, `center`, `baseline / 以 items 的第一行英文内容的基线进行对齐放置`

- align-content

设置多根轴线作为一个整体在另一条轴上的位置，如 `stretch`, `flex-end`, `center`, `space-between`, `space-around`。如果 flex items 只有根轴线，该属性不起作用


### 四、 flex item 有哪些重要属性？

- order

设置 item 的顺序，数值越小，越靠前。默认为 0

- flex

设置 `flex-grow`, `flex-shrink`, `flex-basis`。默认为`flex: 0 1 auto`

- flex-grow 

设置 flex item 的放大比例。默认为0。如果所有 flex item 都设置为1，将等分剩余空间。如果只有一个 flex item 设置为1，将铺满剩余空间

- flex-shrink

设置 flex item 的缩小比例。默认为1，即在容器空间缩小时所有 flex item 等比例缩小。如果想要某个 flex item 保持原来大小，将其设置为0

- flex-basis

设置在分配多余空间之前，flex item 占据的 main size。浏览器根据该属性计算 main axis 是否有多余空间。默认为 auto，即 flex item 本来大小

```

默认，flex: 0 1 auto

注意，当 flex: none，则计算值为flex: 0 0 auto；即不等比例缩小，取消弹性

注意，当 flex: auto，则计算值为 1 1 auto；即设置 items 等分空间

注意，当 flex 取值为一个非负数字，则该数字为 flex-grow， flex-shrink 取 1，flex-basis 取 0%

注意，当 flex 取值为一个长度或百分比，则视为 flex-basis 值，flex-grow 取 1，flex-shrink 取 1

注意，当 flex 取值为两个非负数字，则分别视为 flex-grow 和 flex-shrink 的值，flex-basis 取 0%

注意，当 flex 取值为一个非负数字和一个长度或百分比，则分别视为 flex-grow 和 flex-basis 的值，flex-shrink 取 1

```
