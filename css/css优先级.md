
- 无视规则

属性后插有 !important 的属性拥有最高优先级。

**若同时插有 !important，则再利用规则 3、4 判断优先级。**

---

- 规则1

由于 CSS 的继承特性，当前标签其最近的祖先样式比其他祖先样式优先级高

---

- 规则2

“标签自身样式”比“祖先样式”优先级高

---

- 规则3

对于标签，有如下几种选择器：

```

#id{}, ID选择器

.class{}, 类选择器

a[href="segmentfault.com"]{}, 属性选择器

:hover{}, 伪类选择器

::before{}, 伪元素选择器

span{}, 标签选择器

*{}, 通配选择器

```

优先级由高到低：内联样式, ID 选择器, 类选择器 / 属性选择器, 伪类选择器, 标签选择器 / 伪元素选择器


---

- 规则4

所有的 CSS 的选择符由上述 7 种基础的选择器或组合而成。组合方式有 3 种：

```

.father .child{}, 后代选择符

.father > .child{}, 子选择符

.bro1 + .bro2{}, 相邻选择符

```

当一个标签同时被多个选择符选中，规则如下：

计算选择符中 ID 选择器的个数（a），计算选择符中类选择器、属性选择器以及伪类选择器的个数之和（b），计算选择符中标签选择器和伪元素选择器的个数之和（c）。按 a、b、c 的顺序依次比较大小，大的则优先级高，相等则比较下一个。若最后两个的选择符中 a、b、c 都相等，则按照"就近原则/后覆盖前原则"来判断。

示例：

```

//1
// HTML
<div id="con-id">
    <span class="con-span"></span>
</div>

// CSS
#con-id span {
    color: red; //true
}
div .con-span {
    color: blue;
}


//2
// HTML
<div class="con-id">
    <span class="con-span">112</span>
</div>

// CSS
.con-id span {
    color: red;
}
div .con-span {
    color: blue; // true
}


//3

// HTML
<div class="con-id">
    <span class="con-span">112</span>
</div>

// CSS
div .con-span {
    color: blue;
}
.con-id span {
    color: red; // true
}

```

Q：注意，如果外部样式表和内部样式表的样式发生冲突会出现什么情况？

A：与样式表在 HTML 文件中所处的位置有关，**样式被应用的位置越在下面则优先级越高**，其实这和上述规则4一致。
