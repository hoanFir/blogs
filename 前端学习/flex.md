🐾 flex

🕘 2019.10.28 由 hoanfirst 编辑


### 1. **什么时候会提出？主要目的是什么？**

flex 在2009提出，用来代替`传统盒状模型`。传统盒状模型依赖`display`,`position`,`float`属性，该模型对于一些布局非常不方便，比如垂直居中。


### 2. **使用flex需要知道什么？**

- 任何一个元素（块级元素`flex`或行内元素`inline-flex`）都可以使用flex布局。

- 设置为flex的元素（flex container）和其子元素（flex item）的`float`,`clear`,`vertical-align`属性将失效。

- flex布局有两根轴，主轴（main axis）和交叉轴（cross axis），单个flex item占据的主轴空间叫做main size，单个flex item占据的交叉轴空间叫做cross size。


### 3. **知道flex container哪些重要属性？**

- flex-direction

设置主轴的方向是水平或垂直

- flex-wrap 

设置flex item在一条轴线上放不下时怎么换行，默认不换行。当超出时想支持换行可以设置`flex-wrap: wrap`

- justify-content 

设置flex item在主轴上的对齐方式，如`flex-end`,`center`,`space-between两端对齐`,`space-around每个item之间间隔相等`

- align-items 

设置flex item在交叉轴上的对齐方式，如`stretch默认铺满交叉轴高度`,`flex-end`,`center`,`baseline以flex item的第一行文字的基线对齐`

- align-content

设置当有多根轴线的对齐方式（一般设置只有一行或者一列，所以暂不分析）


### 4. **知道flex item哪些重要属性？**

- flex 

设置`flex-grow`,`flex-shrink`,`flex-basis`。默认为`flex: 0 1 auto`

- flex-grow 

设置flex item本身的放大比例，默认为0。如果所有flex item都设置为1，将等分剩余空间

- flex-shrink 

设置flex item本身的缩小比例，默认为1，即在容器空间缩小时所有flex item等比例缩小。如果想要某个flex item保持原来大小，将其设置为0

- flex-basis 

设置flex item的main size（在主轴上的占据空间），默认为auto（还可以为number），即flex item长度等于指定的长度，如果没有指定则长度将根据内容决定。


注意，当 flex: none，则计算值为 0 0 auto；

注意，当 flex: auto，则计算值为 1 1 auto；

注意，当 flex 取值为一个非负数字，则该数字为 flex-grow， flex-shrink 取 1，flex-basis 取 0%；

注意，当 flex 取值为一个长度或百分比，则视为 flex-basis 值，flex-grow 取 1，flex-shrink 取 1；

注意，当 flex 取值为两个非负数字，则分别视为 flex-grow 和 flex-shrink 的值，flex-basis 取 0%；

注意，当 flex 取值为一个非负数字和一个长度或百分比，则分别视为 flex-grow 和 flex-basis 的值，flex-shrink 取 1；
