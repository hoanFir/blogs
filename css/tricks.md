
### 一、border-radius box-shadow

```html
    <div id="plat-to-cooperation">
      <div class="fb-btn-blue">
        <div class="fb-btn-content">我要合作</div>
      </div>
    </div>
```

```css
    #plat-to-cooperation {
        width: 208px;
        position: relative;
      
        border-radius: 35px;
        box-shadow: 0 4px 0 0 #2aa7e5; /*搭配border-radius实现椭圆阴影效果*/
    }
    
    .fb-btn-blue {
        padding: 10px 28px;
        border-radius: 35px;
        background-color: #59c4f9;
        border-color: #59c4f9; /*解决div白边问题*/
      
        color: #fefefe;
        text-align: center;
        white-space: nowrap;
        vertical-align: middle;
        cursor: pointer;
    }
```

![在这里插入图片描述](https://github.com/hoanFir/blogs/blob/master/css/images/%E6%88%AA%E5%B1%8F2020-03-05%E4%B8%8B%E5%8D%881.29.18.png?raw=true)


### 二、text-align vertical-align

```html
    <div id="view">
      <div class="title">平台合作</div>  
      <div class="subtitle">
        <div class="line"></div>
        cooperation
        <div class="line"></div>
      </div>
    </div>
```

```css
    #view {
        position: relative;
        width: 100%;
        text-align: center;
        padding: 85px 0;
        border-top: 1px solid #f3f3f3;
    }
    .title {
        font-family: Arial Regular;
        color: #4a565c;
        font-size: 28px;
    }
    .subtitle {
        font-family: Arial Regular;
        color: #4a565c;
        opacity: .7;
        font-size: 20px;
    }
    .line {
        display: inline-block;
        margin: 0 15px;
        height: 1px;
        width: 66px;
        vertical-align: middle; /*垂直居中*/
        background-color: #cccfd1;
    }
```


### 三、text-align right

```html
    <div class="form-group"><label for="name"><i class="star"></i> 姓名：</label></div>
    <div class="form-group"><label for="name"><i class="star"></i> 电子邮箱：</label></div>
```

```css
    label {
        display: inline-block;
        font-size: 14px;
        font-weight: 500;
        width: 100px;
        text-align: right;
    }
    /*设置icon*/
    .star {
        display: inline-block;
        height: 10px;
        width: 10px;
        background-image: url(/public/img/customized_star.png);
        background-repeat: no-repeat;
        margin-right: 3px;
    }
```

![在这里插入图片描述](https://github.com/hoanFir/blogs/blob/master/css/images/%E6%88%AA%E5%B1%8F2020-03-05%E4%B8%8B%E5%8D%881.39.04.png?raw=true)

### 四、text-shadow

```html
    <span>标题</span>

```

```css
    span {
        position: absolute;
        white-space: nowrap;
        font-size: 26px;
        transition: all .3s ease-in-out;
        cursor: default;
        color: #80888d;
    }
    span:hover {
      color: blue;
      text-shadow: 0 0 0.2em blue, 0 0 0.2em blue, 0 0 0.2em blue; 
      /* 偏距为0时, 实现一个周围会发光的字母. 如果单一的阴影不够强烈, 那就重复同样的阴影几次 */
    }
```

### 五、...
