
在开发中，对于用户的浏览器不支持字体的场景，可以使用 css3 @font-face 来下载并应用自定义的字体。

Internet Explorer 9, Firefox, Opera, Chrome, 和 Safari 支持 @font-face 规则。

注意，Internet Explorer 8 及更早IE版本不支持@font-face 规则。

### 一、@font-face 语法规则：

```css

div
{
  font-family: yourfontname;
}

@font-face {

  /*指定字体名称*/
  font-family: yourfontname;
  
  /*指定字体URL*/
  src: <source> [<format>][, <source> [format]]*;
  
}

```


#### 1.1 src / source

自定义字体的存放路径。


### 1.2 src / format

自定义字体的格式。

用于帮助浏览器识别字体文件格式，如 truetype、opentype、web open font format、embedded-opentype、avg 等。

1. TrueType / .ttf

true type format.

.ttf 是 widows 和 mac 最常见的字体。


2. OpenType / .otf

open type format.

.otf 是一种原始的字体格式，其在 .ttf 的基础上提供了更多的功能。


3. Web Open Font Format / .woff

.woff 是 web 最佳格式，是 .ttf/.otf 的压缩版本，同时也支持元数据包的分离。


4. Embedded Open Type / .eot

.eot是 IE9 专用字体。


5. Svg / .svg

.svg 是基于 SVG 字体渲染的一种格式。
  

### Bulletproof @font-face

为了使 @font-face 更多的浏览器支持，Paul Irish 写了一个独特的语法

```javascript

@font-size {
  font-family: 'YourWebFontName'; 
  src: url('YourWebFontName.eot'); /* IE9 */ 
  src: url('YourWebFontName.eot?#iefix') format('embedded-opentype'), /* IE6-IE8 */              
          url('YourWebFontName.woff') format('woff'), /* Modern Browsers */                    
          url('YourWebFontName.ttf')  format('truetype'), /* Safari, Android, iOS */              
          url('YourWebFontName.svg#YourWebFontName') format('svg'); /* Legacy iOS */ 
}

```
