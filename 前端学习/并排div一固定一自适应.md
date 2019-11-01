🐾 并排div一固定一自适应

🕘 2019.10.28 由 hoanfirst 编辑

### 方式一：flex+width+calc

```html
<body>
    <div class="container">
        <div class="left-item"></div>
        <div class="right-item"></div>
    </div>
</body>
```

```css
body,html {margin:0; height:100%;}
.container {
    display: flex;
    height: 100%;
}
.left-item {
    background-color: antiquewhite;
    height: 100%;
    width: 200px;
}
.right-item {
    background-color: aqua;
    height: 100%;
    width: calc(100% - 200px);
}
```

### 方式二：flex+width+flex-grow

```html
<body>
    <div class="container">
        <div class="left-item"></div>
        <div class="right-item"></div>
    </div>
</body>
```

```css
body,html {margin:0; height:100%;}
.container {
    display: flex;
    height: 100%;
}
.left-item {
    background-color: antiquewhite;
    height: 100%;
    width: 200px;
}
.right-item {
    background-color: aqua;
    height: 100%;
    flex: 1; /* flex: 1 1 0% */
}
```

### 方式三：float+BFC

```html
<body>
    <div class="container">
        <div class="left-item"></div>
        <div class="right-item"></div>
    </div>
</body>
```

```css
body,html {margin:0; height:100%;}
.container {
    height: 100%;
}
.left-item {
    background-color: antiquewhite;
    height: 100%;
    width: 200px;
    float: left;
}
.right-item {
    background-color: aqua;
    height: 100%;
    overflow: hidden; /* 触发BFC */
}
```

### 方式四：float+margin-left

```html
<body>
    <div class="container">
        <div class="left-item"></div>
        <div class="right-item"></div>
    </div>
</body>
```

```css
body,html {margin:0; height:100%;}
.container {
    height: 100%;
}
.left-item {
    background-color: antiquewhite;
    height: 100%;
    width: 200px;
    float: left;
}
.right-item {
    background-color: aqua;
    height: 100%;
    margin-left: 200px;
}
```

### 方式五：document.body.clientWidth + float

```html

<script>
    
    const CW = document.body.clientWidth;
    const rightWidth = CW - 200;
</script>
<body>
    <div class="container">
        <div class="left-item"></div>
        <div class="right-item" style={{width: rightWidth}}></div>
    </div>
</body>
```

```css
body,html {margin:0; height:100%;}
.container {
    height: 100%;
}
.left-item {
    background-color: antiquewhite;
    height: 100%;
    width: 200px;
    float: left;
}
.right-item {
    background-color: aqua;
    height: 100%;
    float: left;
}
```

