
在开发中，对于用户的浏览器不支持字体的场景，可以使用 css3 引入的 @font-face 来下载并应用自定义的字体。

@font-face 语法规则：

```css

@font-face {

  font-family: yourfontname;
  
  src: <source> [<format>][, <source> [format]]*;
  
  [font-wight: <weight>];
  
  [font-style: <style>];
}

```
