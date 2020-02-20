🐾 Vue入门与使用指南

🕘 2019.12.09 由 hoanfirst 编辑（原作者为JD_mk）

### MVC和MVVC


![MVC模型](https://github.com/hoanFir/blogs/blob/master/vue/images/%E4%BC%81%E4%B8%9A%E5%92%9A%E5%92%9A%E6%88%AA%E5%9B%BE20191209155129.png?raw=true)

![MVVC模型](https://github.com/hoanFir/blogs/blob/master/vue/images/%E4%BC%81%E4%B8%9A%E5%92%9A%E5%92%9A%E6%88%AA%E5%9B%BE20191209155915.png?raw=true)


### Vue.js

Vue.js是一套用于构建用户界面的渐进式框架，其核心库只聚焦视图层，可与第三方库或既有项目整合。如：

声明式渲染(Declarative Rendering) -> 组件系统(Component System) -> 客户端路由(Client-side Routing) -> 大规模状态管理(Large Scale State Management) -> 构建工具(Build System)


### 学习建议

基础 -> 进阶 -> 高阶

基础：通读官方文档的基础篇，模仿案例代码实现

进阶：阅读官方文档的进阶篇的前半部分，着重理解Vue.js的`响应式机制`和`组件生命周期`

高阶：阅读官方文档的关于路由和状态管理部分，根据需要学习vue-router、vuex


### 掌握内容

基础：Vue.js生命周期、模板语法、计算属性和侦听、事件和表单输入

组件：组件基础、prop以及父子事件、slot、动态组件&异步组件

可复用性：混入、自定义指令、渲染函数和jsx、插件、过滤器

规模化：路由vue-router、状态管理vuex、服务端渲染ssr

内在核心：深入响应式原理等


### 深入响应式原理

响应式，即数据双向绑定。

![](https://github.com/hoanFir/blogs/blob/master/vue/images/%E4%BC%81%E4%B8%9A%E5%92%9A%E5%92%9A%E6%88%AA%E5%9B%BE20191209170103.png?raw=true)


1. 分析

响应式的关键在于使用`Object.defineProperty`把data对象中的属性转换为`getter/setter`。对象有两种类型：数据属性和访问器属性，访问器属性需要手动设置。

每个component都有一个watcher实例，它会在组件渲染过程中把“接触/Touch”过的数据属性记录为依赖/Dependency，之后当依赖项的setter触发时，就会 通知/Notify watcher，从而使它关联的组件重新渲染。


2. Vue检测数据更新时的一些注意事项

2.1 受现代JavaScript的限制，vue无法检测到对象属性的添加或删除，由于Vue.js会在初始化实例时对属性执行getter/setter转化，所以属性应该通过在data对象上定义，才能成为响应式的。

```javascript

var vm = new Vue({ 
  data: {
    a: 1
  }
})

vm.b = 2; 

//vm.a是响应式的
//vm.b是非响应式的

```

2.2 对于已经创建的实例，Vue.js不允许动态添加根级别的响应式属性。但是，可以使用Vue.set(object, propertyName, value)方法向嵌套对象添加响应式属性。

2.3 复制对象或添加合并多个属性时使用Object.assign({}, ...)或JSON.parse(JSON.stringify(obj))

```javascript

this.someObject = Object.assign(this.someObject, { a: 1, b: 2 }); //x
this.someObject = Object.assign({}, this.someObject, { a: 1, b: 2 }); //√

```

3. Vue更新数据是异步执行的

Vue.js在更新DOM时是异步执行的，只要侦听到数据变化，Vue.js就开启一个队列，并缓存在同一事件循环中发生的所有数据变更。注意，如果同一个watcher被多次触发，只会被推入队列中一次，这种在缓存时去除重复数据对于避免不必要的计算和DOM操作是非常重要的。

然后，在下一个事件循环“tick”中，Vue.js刷新队列并执行实际工作。


