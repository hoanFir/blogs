🐾 svg

🕘 2019.10.18 由 hoanfirst 编辑

Scalable Vector Graphics(SVG), is an `XML-based markup language` for describing two dimensional based `vector graphics`.

Scalable Vector Graphics is a text-based open web standard.

Scalable Vector Graphics images and their related behaviors are defined in XML text files. **So they can be searched, indexed, scripts and compressed, and they can be created and edited with any text editor and with drawing software**.


1. **Scalable Vector Graphics element**

category|elements|
-|-|
Animations elements|animate, animateColor, animateMotion, animateTransform, discard, mpath, set|
Basic shapes|circle, ellipse, line, polygon, polyline, recr|
Container elements|a, defs, g, marked, mask, missing-glyph, pattern, svg, switch, symbol|
Descriptive elements|desc, metadata, title|
Font element|font, font-face, font-face-format, font-face-name, font-face-src, font-face-uri, hkern, vkern|
Gradient elements|linearGradient, meshgradient, radialGradient, stop|
Light source elements|feDistantLight, fePointLight, feSpotLight|
...|...|


2. Scalable Vector Graphics attribute

SVG elements can be modified using attributes that specify details about exactly how the element should be handled or rendered. 

3. Scalable Vector Graphics DOM interface

Details about the SVG DOM API, for interaction with JavaScript.

4. **Applying Scalable Vector Graphics effects to HTML content**

Use SVG to works togethe with HTML, CSS and Javascript to enhance a regular HTML page or web application.

Modern browsers support using SVG within CSS styles to apply graphical effects to HTML content.

There are 3 properties we can use: 

property name|desc|
-|-|
mask|with the svg mask element|
clip-path|to clip HTML content|
filter|apply a fiter to HTML content|

示例：模糊效果filter

```html

<p class="blur">blur text.</p> 

<svg height="0">
  <defs>
    <filter id="wherearemyglasses" x="0" y="0">
    <feGaussianBlur in="SourceGraphic" stdDeviation="1"/>
    </filter>
  </defs>
</svg> 


```

```css

.blur { filter: url(#wherearemyglasses); }

```
