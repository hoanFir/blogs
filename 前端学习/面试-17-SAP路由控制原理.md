首先我们要知道，在单页面应用 SPA 开发模式中，路由切换时会替换 DOM Tree 中最小修改的部分 DOM，以来减少原先因为多页应用的页面跳转带来的巨量性能损耗。
目前React、Vue对应的典型路由解决方案，即react-router、vue-router。一般来说，这些路由插件总是提供两种不同方式的路由方式：Hash 和 History，有时也会提供非浏览器环境下的路由方式 Abstract。

在 vue-router 中是使用了**外观模式**将几种不同的路由方式提供了一个一致的高层接口，让我们可以更解耦的在不同路由方式中切换。值得一提的是，Hash 和 History 除了**外观**上的不同之外，还一个区别是：Hash 方式的状态保存需要另行传递，而 HTML5 History 原生提供了自定义状态传递的能力，我们可以直接利用其来传递信息。

下面的内容包括：Hash 和 History 区别和简单实现、懒加载、动态路径匹配、嵌套路由、路由别名

## Hash
Hash 方法是在路由中带有一个 #，主要原理是通过监听 # 后的 URL 路径标识符的更改而触发的浏览器 **hashchange** 事件，然后通过获取 **location.hash**（返回 URL 的锚部分） 得到当前的路径标识符，再进行一些路由跳转的操作。注意，由于 Hash 方法是利用了相当于**页面锚点**的功能，所以可能会与原来的通过锚点定位来进行页面滚动定位的方式产生冲突，导致定位到错误的路由路径。

 1. 简单实例

```
class RouterClass {
  constructor() {
    this.routes = {}        // 记录路径标识符对应的cb function
    this.currentUrl = ''    // 记录hash只为方便执行cb function
    
    window.addEventListener('load', () => this.render())
    
    window.addEventListener('hashchange', () => this.render())
  }
  
  /* 初始化 */
  static init() {
    window.Router = new RouterClass()
  }
  
  /* 注册路由和回调 */
  route(path, cb) {
    this.routes[path] = cb || function() {} //保存当前路径并执行对应的回调
  }
  
  /* 记录当前hash，执行cb function */
  render() {
    this.currentUrl = location.hash.slice(1) || '/'
    this.routes[this.currentUrl]() //通过当前路径获取对应的回调并执行
  }
}
```

2. 路由后退存在的问题
如果希望使用脚本来控制 Hash 路由的后退，可以将经历的路由记录下来，路由后退跳转的实现是对 location.hash 进行赋值。但是这样会引发重新引发 hashchange 事件，再次 render 。重复必然会导致性能浪费。
解决方案：
添加一个标志位，用于表明当前执行 render 方法是因为回退回来的还是跳转过来的。如果是回退，则不重复执行当前路由页面对应的回调。

```
class RouterClass {
  constructor() {
    this.isBack = false
    
    this.routes = {}        // 记录路径标识符对应的cb
    this.currentUrl = ''    // 记录hash只为方便执行cb
    
    this.historyStack = []  // hash栈，保存经历的路由记录
    
    window.addEventListener('load', () => this.render())
    
    window.addEventListener('hashchange', () => this.render())
  }
  
  /* 初始化 */
  static init() {
    window.Router = new RouterClass()
  }
  
  /* 记录path对应cb */
  route(path, cb) {
    this.routes[path] = cb || function() {}
  }
  
  /* 入栈当前hash，执行cb */
  render() {
    if (this.isBack) {      // 如果是由backoff进入，则置false之后return
      this.isBack = false   // 其他操作在backoff方法中已经做了
      return
    }
    this.currentUrl = location.hash.slice(1) || '/'
    this.historyStack.push(this.currentUrl) //入栈
    this.routes[this.currentUrl]()
  }
  
  /* 路由后退 */
  back() {
    this.isBack = true
    this.historyStack.pop()                   // 移除当前hash，回退到上一个
    
    const { length } = this.historyStack
    if (!length) return
    
    let prev = this.historyStack[length - 1]  // 拿到要回退到的目标hash
    location.hash = `#${ prev }`
    
    this.currentUrl = prev
    this.routes[prev]()                       // 执行对应cb
  }
}
```

## HTML5 History API
HTML5 原生提供了一些路由操作的 Api，如
路由桟：保存经历的路由历史记录
- history.go(n)：路由跳转，比如 n为 2 是根据路由桟往前移动2个页面，n为 -2 是根据路由桟向后移动2个页面，n为0是刷新当前页面
- history.back()：路由后退，相当于 history.go(-1)
- history.forward()：路由前进，相当于 history.go(1)
- history.pushState()：往路由桟添加一条路由历史记录，如果设置跨域网址则报错
- history.replaceState()：替换当前页在路由历史记录的信息
- histoey.popstate()：当活动的历史记录发生变化，就会触发 popstate 事件，在点击浏览器的前进后退按钮或者调用上面前三个方法的时候也会触发

所以直接使用浏览器的api，如 pushState() 和 replaceState()，代码更为整洁。

2. 实例

```
class RouterClass {
	constructor(path) {
		this.routes = {} //记录每个路由标识对应的callback处理函数
		history.replaceState({path}, null, path)

		window.addEventListener('popstate', e=>{
			const path = e.state && e.state.path
			this.routes[path] && this.routes[path]()
		})
	}

	//初始化
	static init() {
		window.Router = new RouterClass(location.pathname)
	}
	
	//注册路由和回调
	route(path, cb) {
		this.routes[path] = cb || function() {}
	}
	
	go(path) {
		history.pushState({path}, null, path)
		this.routes[path] && this.routes[path]()
	}
}
```

## 总结
Hash 模式是使用 URL 的 Hash 来模拟一个完整的 URL，因此当 URL 改变的时候页面并不会重载。History 模式则会直接改变 URL，所以在路由跳转的时候会丢失一些地址信息，当在**刷新**或**直接访问路由地址**的时候会匹配不到静态资源，就需要在服务器上配置一些信息，让服务器增加一个覆盖所有情况的候选资源，比如跳转 index.html 什么的。事实上 vue-router 等库也是这么做的，还提供了常见的服务器配置。
