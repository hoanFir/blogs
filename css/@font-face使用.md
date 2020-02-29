
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


2. OpenType / .otf

open type format.

.otf是一种原始的字体格式，其在.ttf的基础上提供了更多的功能。

支持的浏览器：Firefox3.5+,Chrome4+,Safari3.1+,Opera10+,iOS Mobile Safari4.2+



3. Web Open Font Format / .woff

.woff 是 web 最佳格式，是 .ttf/.otf 的压缩版本，同时也支持元数据包的分离

支持的浏览器：IE9+,Firefox3.5+,Chrome6+,Safari3.6+,Opera11.1+



4. Embedded Open Type / .eot

.eot是IE专用字体。



5. Svg / .svg

.svg 是基于 SVG 字体渲染的一种格式。
  
支持的浏览器：Chrome4+,Safari3.1+,Opera10.0+,iOS Mobile Safari3.2+



### Bulletproof @font-size

为了使 @font-face 更多的浏览器支持，Paul Irish写了一个独特的语法

```javascript

@font-size {
  font-family: 'YourWebFontName'; 
  src: url('YourWebFontName.eot'); /* IE9 Compat Modes */ 
  src: url('YourWebFontName.eot?#iefix') format('embedded-opentype'), /* IE6-IE8 */              
          url('YourWebFontName.woff') format('woff'), /* Modern Browsers */                    
          url('YourWebFontName.ttf')  format('truetype'), /* Safari, Android, iOS */              
          url('YourWebFontName.svg#YourWebFontName') format('svg'); /* Legacy iOS */ 
}

```
