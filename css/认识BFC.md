BFC, block formatting context.

BFC 是 Web 页面的可视化 css 渲染的一部分.

BFC 是 block boxes 布局过程中产生的区域，也是 floats element 和其他元素交互的区域。这样说可能不太好理解，可以从使用场景入门。

### 一、两种场景

BFCs are important for the positioning and clearing of floats.

The rules for positioning and clearing of floats apply only to things within the same block formatting contexts. 

换句话说，只要将需要进行处理的 elements 分在不同的 BFCs 里，就可以解决由于同处一个 BFC 所造成的问题。

#### 1.1 场景一

解决 margin collapsing 问题。

margin collapsing occurs only between blocks that belong to the same BFC.

```html
<div class="blue"></div>
<div class="red-outer">
  <div class="red-inner">red inner</div>
</div>
```

```css
.blue, .red-inner {
  height: 50px;

  margin: 10px 0;
}

.blue {
  background: blue;
}

.red-outer {
  overflow: hidden;

  background: red;
}
```

### 1.2 场景二

解决父元素因内部元素浮动而塌陷的问题。

示例1：

```html
<div class="box">
    <div class="float">I am a floated box!</div>
    <p>I am content inside the container.</p>
</div>
```

```css
.box {
    background-color: rgb(224, 206, 247);
    border: 5px solid rebeccapurple;
}
.float {
    float: left;
    width: 200px;
    height: 150px;
    background-color: white;
    border:1px solid black;
    padding: 10px;
}
```

在上述代码，我们发现父元素高度并不由内部 float 元素决定。

解决：

```css
.box {
    background-color: rgb(224, 206, 247);
    border: 5px solid rebeccapurple;

    overflow: auto; /*触发bfc*/
}
.float {
    float: left;
    width: 200px;
    height: 150px;
    background-color: white;
    border:1px solid black;
    padding: 10px;
}
```

Floats don't affect the layout of the content inside other BFC, and clear only clears past floats in the same BFC.

So to create a new BFC for parent element and let it become a mini-layout which any child element will be contained inside it.


### 二、那么，如何触发bfc

#### 2.1 common using

- html element

- elements where float isn's none

- elements where position is absolute or fixed

- elements where with display: inline-block

- block elements where overflow has a value other than visible

- flex items(display: flex/inline-flex)

- …


#### 2.2 using display: flow-root

A newer value of display lets us create a new BFC without any other potentially problematic side-effects.

因为，使用一些 common using 触发 bfc 可能会有副作用。

如使用 `overflow: auto`，the `overflow` property is meant to telling the browser how you want to deal with overflowing content. There are some occasions in which you will find you get unwanted scrollbars or clipped shadows when you just want to create a BFC.

因此也有人建议：在使用含有默认意义的 property 来创建 BFC 时，记得注释。

```html
<div class="box">
    <div class="float">I am a floated box!</div>
    <p>I am content inside the container.</p>
</div>
```

```css
.box {
    background-color: rgb(224, 206, 247);
    border: 5px solid rebeccapurple;
    display: flow-root
}
.float {
    float: left;
    width: 200px;
    height: 150px;
    background-color: white;
    border:1px solid black;
    padding: 10px;
}     
```

注意：Note: display: flow-root; is not supported by Safari.