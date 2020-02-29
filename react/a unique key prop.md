
我们经常在控制台看到如下警告：

```

Warning: Each child in an array or iterator should have a unique "key" prop. Check the render method of "xxx".

```

### key prop

在开发中，经常会利用一个循环来渲染一列元素：

```

//vue
<ul>
  <li v-for="item in items" :key="item.id">...</li>
</ul>

//react
<ul>
  {
    return data.map(item => (
      <li key={item.id}>{item.name}</li>
    ))
  }
</ul>

```

在上述代码中，为什么针对 key 属性，不直接使用 index 值呢？

这是因为，如果数据更新后，大概率所有元素的顺序会发生改变，就导致所有元素重新渲染。

tips：当然，如果数据只是作为纯展示而不会有什么变更，可以使用index作为key。


### 唯一标识

通过指定唯一 key 值，即给每个元素添加唯一的标识，就不会轻易发生改变。

因为 key 属性主要用在虚拟 DOM 的 diff 算法，在新旧节点对比时，可以最大限度减少动态元素并且尽可能地尝试修复/再利用相同类型元素。

