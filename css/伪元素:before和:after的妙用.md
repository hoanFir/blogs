
伪元素，pseudo element。使用伪元素（pseudo-element）的核心在于更利于语义化。

CSS2 - :before 伪元素，可以在元素的内容前面插入新内容。CSS2 - :after 伪元素，可以在元素的内容之后插入新内容。

## 示例一

使用 ::after 实现 element 之间的间隔符，并使用 :not(:last-child) 排除最后一个元素

```html

<nav>
    <ul>
        <li>HTML 5</li>
        <li>|</li>
        <li>CSS3</li>
        <li>|</li>
        <li>JavaScript</li>
    </ul>
</nav>

//改进
<nav>
    <ul>
        <li>HTML 5</li>
        <li>CSS3</li>
        <li>JavaScript</li>
    </ul>
</nav>

<style>
    ul {
        list-style: none;
    }
    li {
        display: inline;
    }
    li:not(:last-child)::after {
        content: " |";
    }
</style>

```

## 示例2

为 element 添加三角图标 实现“对话框”效果

```html

<p></p>

<style>
        p{
            width: 200px;
            height: 50px;
            line-height: 50px;
            background: red;
        }
        p::before{
          content: "";
          border: 5px solid transparent;
          border-right-color: red;
          /* border-left-color: black;
          border-top-color: green;
          border-bottom-color: yellow; */
          
          /*inline-block的高度由内容撑开*/
          display: inline-block;
          
          position: relative;
          left: -10px;
        }
</style>

```


## 示例3

为 form 表单项前面添加图标

```html

    <div class="search">
        <input placeholder="请输入搜索词" />
    </div>


        .search{
            position: relative;
        }
        .search::before{
            content: "☌";
            transform: rotate(180deg);
            float: left;
            font-size: 25px;
            position: absolute;
            line-height: 30px;
            left: 5px;
        }
        .search input{
            display: block;
            padding-left: 25px;
            height: 30px;
            box-sizing: border-box;
        }

```


## 示例4 

为 form 表单项添加before-“必填” 以及 after-“单位”

```html
    <label class="required">姓名 <input /> </label>
    <label class="unit">金额 <input /> </label>

    <style>
        label{
            display: block;
        }
        .required::before {
            content: "*";
            color: red;
            display: inline-block;
            width: 0;
            position: relative;
            left: -5px;
        }
        .unit::after {
            content: "万元";
        }
    </style>
```


