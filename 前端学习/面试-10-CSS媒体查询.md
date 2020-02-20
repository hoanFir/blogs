
# 一、media query 媒体查询是什么
CSS3 `@media` 媒体查询， media query。

示例：
```css
/* 当页面宽度<300px */
@media screen and (max-width: 300px) {
	html: { font-size: 40px; }
}
```

通过 `@media`，我们可以针对不同的媒体查询类型，获取到不同的屏幕尺寸，从而设置不同的样式进行页面元素显示的适配。

## 语法
1. 直接在 css样式文件 中设置：
```css
@media mediatype and|not|only (medita feature) {
	/*css code..*/
}
```

2. 针对不同的媒体使用不同的 css 样式文件：
```html
<link rel="stylesheet" media="mediatype and|not|only (media feature)" href="style1.css"
```

## mediatype 媒体类型
|value  |description  |
|--|--|
|all  |所有设备  |
|print  |打印机、打印机预览  |
|screen  |电脑屏幕、平板电脑、智能手机  |
|speech  |屏幕阅读器等发声设备  |

## medita feature 媒体功能
|value  |description  |
|--|--|
|aspect-ratio  |输出设备中的页面可见区域宽度与高度的比率  |
|max-aspect-ratio  |输出设备的屏幕可见宽度与高度的最大比率  |
|device-aspect-ratio  |输出设备的屏幕可见宽度与高度的比率  |
|max-device-aspect-ratio  |输出设备的屏幕可见宽度与高度的最大比率  |
|device-height/width  |输出设备的屏幕可见高度/宽度  |
|max-device-height/width  |输出设备的屏幕可见的最大高度/宽度  |
|height/width  |输出设备中的页面可见区域高度/宽度  |
|max-/height/width  |输出设备中的页面最大可见区域高度/宽度  |
|color  |输出设备每一组彩色原件的个数。如果不是彩色设备，则值等于0  |
|max-color  |输出设备每一组彩色原件的最大个数  |
|monochrome  |在一个单色框架缓冲区中每像素包含的单色原件个数。如果不是单色设备，则值等于0  |
|max-monochrome  |在一个单色框架缓冲区中每像素包含的最大单色原件个数  |
|color-index  |输出设备的彩色查询表中的条目数。如果没有使用彩色查询表，则值等于0  |
|max-color-index  |输出设备的彩色查询表中的最大条目数  |
|grid  |查询输出设备是否使用栅格或点阵  |
|resolution  |设备的分辨率，如96dpi，300dpi，118dpcm  |
|max-resolution  |设备的最大分辨率  |
|orientation  |输出设备中的页面可见区域高度是否大于或等于宽度  |
|scan  |电视类设备的扫描工序  |


# 适配多种移动设备

一般有两种方式：**根据一段 js 代码动态计算根元素的 font-size** 或者 **针对自身网站有哪些主流的屏幕设备做media query设置**。
根据浏览器当前的分辨率，改变根元素 `font-size` 的值，也页面的其他元素都不需要进行任何改变，并且页面的排版都是按照等比例切换的，布局不乱。

## 1. 根据 js 代码动态计算根元素的 font-size
略

## 2. rem & media query（重点）

```css
html { font-size: 20px; }

@media only screen and (min-width: 401px){ html{ font-size: 25px !important; 
	}}@media only screen and (min-width: 428px){ html{ font-size: 26.75px !important;
	}}@media only screen and (min-width: 481px){ html{ font-size: 35px !important;
	}}@media only screen and (min-width: 569px){ html{ font-size: 35px !important;
	}}@media only screen and (min-width: 641px){ html{ font-size: 40px !important;
```

## 3. 使用了 rem 技术的 Web app ：
淘宝首页：m.taobao.com
D X：m.dx.com
