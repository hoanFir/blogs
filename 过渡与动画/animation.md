🐾 animation

🕘 2019.10.18 由 hoanfirst 编辑

示例：转人工加载效果

![](https://github.com/hoanFir/blogs/blob/master/%E8%BF%87%E6%B8%A1%E4%B8%8E%E5%8A%A8%E7%94%BB/images/loading.png?raw=true)

```html
<div class="dot-loading">
  <div class="dot-wrapper-before">
    <div class="dot"></div>               
    <div class="dot"></div>               
    <div class="dot"></div>
  </div>
  <div class="dot-content">当前问题正在为您转人工客服处理</div>
  <div class="dot-wrapper-after">
    <div class="dot"></div>               
    <div class="dot"></div>               
    <div class="dot"></div>
  </div
</div>
```

```css
.dot-loading {
  display: flex; 
}
.dot-content {
  color: #999;
}
.dot-wrapper-before, .dot-wrapper-after {
  margin: 0 6px;
}
.dot {
  position: relative;
  display: inline-block;
  width: 6px;
  height: 6px;
  will-change: transform;
}
.dot:before {
  position: absolute;
  display: block;
  content: "";
  top: -2px;
  width: 6px;
  height: 6px;
  border-radius: 50%;
  background-color: #999;
}

.dot-wrapper-after .dot:first-child:before {
  animation: bubble 1.3s infinite cubic-bezier(0.455, 0.03, 0.515, 0.955);
}
.dot-wrapper-after .dot:nth-child(2):before {
  animation: bubble 1.3s infinite cubic-bezier(0.455, 0.03, 0.515, 0.955);
  animation-delay: .2s;
}
.dot-wrapper-after .dot:last-child:before {
  animation: bubble 1.3s infinite cubic-bezier(0.455, 0.03, 0.515, 0.955);
  animation-delay: .4s;
}

.dot-wrapper-before .dot:first-child:before {
  animation: bubble 1.3s infinite cubic-bezier(0.455, 0.03, 0.515, 0.955);
  animation-delay: .4s;
}
.dot-wrapper-before .dot:nth-child(2):before {
  animation: bubble 1.3s infinite cubic-bezier(0.455, 0.03, 0.515, 0.955);
  animation-delay: .2s;
}
.dot-wrapper-before .dot:last-child:before {
  animation: bubble 1.3s infinite cubic-bezier(0.455, 0.03, 0.515, 0.955);
}

@keyframes bubble {
  0% {
    -webkit-transform: scale(0.2);
            transform: scale(0.2);
    opacity: .2;
  }
  50% {
    -webkit-transform: scale(1);
            transform: scale(1);
    opacity: 1;
  }
  100% {
    -webkit-transform: scale(0.2);
            transform: scale(0.2);
    opacity: .2;
  }
}
```
