
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

### src / source

自定义字体的存放路径

### src / format

自定义字体的格式。

用于帮助浏览器识别字体文件格式，如truetype、opentype、web open font format、embedded-opentype、avg等。

1. TrueType / .ttf

true type format.

.ttf是 widows 和 mac 最常见的字体，是一种 raw 格式，因此不为网站优化。

支持的浏览器：IE9+,Firefox3.5+,Chrome4+,Safari3+,Opera10+,iOS Mobile Safari4.2+


2. 


