🐾 svg vs canvas

🕘 2019.10.18 由 hoanfirst 编辑

首先从通用层面描述两者各自适合的场景：

![](https://github.com/hoanFir/blogs/blob/master/%E5%8F%AF%E8%A7%86%E5%8C%96%E6%8A%80%E6%9C%AF/svgcanvas.png?raw=true)


### 概述

HTML5 提供了 `Canvas` 和 `SVG` 两种绘图技术，也是多数 Web 图表库使用的渲染技术。

`Canvas` 是基于脚本的，通过 JavaScript 来动态绘图，而 `SVG` 则是使用 XML 文档来描述矢量图。

### 特性

`Canvas` 提供的绘图能力更偏向底层，适合做**像素级**的图形处理，能动态渲染和绘制**大数据量**的图形。

`SVG` 抽象层次更高，声明描述式的接口功能更丰富，内置了大量图形、滤镜和动画等。另外，`SVG`可以导出为文件脱离浏览器环境使用。

### 性能

性能对比看场景。

从底层来看，`Canvas`的性能受画布尺寸（size of screen）影响更大，`SVG`的性能受图形元素个数（number of objects）影响更大。

- 在大画布、小数据量的情况下，采用`SVG`通常内存占用会更小（如移动端，对内存占用就非常敏感）。在做缩放、平移等高频操作的时候往往帧率也更高

- 在小画布、大数据量的场景适合采用canvas，譬如热力图、散点图。
