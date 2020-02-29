HTML5 os the latest evolution of the standard that defines HTML. It includes two concepts:

1. A new version of the language HTML, with new elements, attributes and behaviors.

2. A larger set of technologies that allows the building of more diverse and powerful Web

1. 语义化（semantics）

```
✔(区块和段落元素)sections and outlines: 
<section>
<article>
<nav>
<header>
<footer>
<aside>
<hgroup>
 ✔ (音频和视频)audio and video:
<audio>
<video>
<track>
 ✔ (表单的改进，如强制校验API)Forms improvements:
<input type>
<output>
 ✔ (新的语义元素)new semantic elements:
<mark>
<figure>
<figcation>
<data>
<time>
<output>
<progress>
<meter>
<main>
 ✔ (iframe的改进，精确控制iframe的安全级别以及期望的渲染)iframe improvement:
sandbox
seamless
srcdoc
 ✔ （支持直接嵌入数学公式）MathML
```
 
2. 连通性（connectivity）

```
 ✔ web sockets(允许在页面和服务器之间建立持久连接并支持交互非html数据)
 ✔ server-sent events(允许服务器向客户端推送事件)
 ✔ webRTC(其中的RTC代表的是即时通信，允许直接在浏览器中控制视频会议，而不需要插件或外部应用程序)
```

3. 离线 & 存储（offline and storage）

```
 ✔ DOM存储/WHATWG(客户端会话和持久化存储让应用程序能够在客户端存储结构化数据)
 ✔ IndexdDB(一个支持在浏览器存储大量结构化数据并且能试用索引进行高性能检索的web标准)
 ✔ 支持访问由用户选择的多个本地文件，如type file 的 input multiple属性
```

4. 2D/3D（graphics and effects）

```
 ✔ canvas
 ✔ webGL
 ✔ SVG(一个基于XML直接嵌入到HTML中的矢量图像格式，在使用中可以使用img引入)
```

5. 性能 & 集成（performance and integration）

```
 ✔ web workers
能够把JavaScript计算委托给background threads，防止interactive events变得缓慢
 ✔ XMLHttpRequest Level 2
允许异步读取页面的某些部分，允许显示动态内容，这是在Ajax背后的技术
 ✔ 即时编译的JavaScript引擎
JIT-compiling JavaScript engines，新一代js引擎
 ✔ history API
 ✔ contentEditable
 ✔ drag and drop
允许在不同web应用之间拖放 
 ✔ activeElement and hasFocus
焦点管理
 ✔ requestAnimation
允许控制动画渲染，获得更优性能
 ✔ fullscreen
支持应用使用整个屏幕
 ✔ online and offline events
支持获取当前应用程序在线或离线状态

```

6. 设备访问（device access）

```
 ✔ Camera
 ✔ Touch events
 ✔ Geolocation
 
```
