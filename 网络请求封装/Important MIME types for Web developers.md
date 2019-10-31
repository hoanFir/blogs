🐾 Important MIME types for Web developers

🕘 2019.10.31 由 hoanfirst 编辑

A media type, known as a MIME types or Multipurpose Internet Mail Extensions. 多用途因特网邮件扩展

a media type is a standard that indicates the nature and format of a document, file, or assortment of bytes.

tips：

**`Browsers` use the MIME type(not the file extension) to determine `how to process a URL`**, so it's important that web servers send the correct MIME type in the response's `Content-Type` header. If this is not correctly configured, browsers are likely to misinterpret the contents of files and sites will not work correctly, and downloaded files may be mishandled.


---

the simplest MIME type's structure: `type/subtype`.

type: represents the general category. such as `video`, `text`

subtype: identifies the exact kind of data of the specified type. such as `plain`, `html`

---

the complex MIME type's structure: `type/subtype;parameter=value`.

For example, for any MIME type whose type is `text`, the optional charset parameter can be used to specify the `character set` used for the characters in the data. If no charset is specified, the default is `ASCII (US-ASCII)`. To specify a `UTF-8` text file, the MIME type `text/plain;charset=UTF-8` is used.

---

discrete type|multipart type|
--|--|
application, video, audio, font, image, text... |multipart...|

text	表明文件是普通文本，理论上是人类可读
image	表明是某种图像。不包括视频，但是动态图（比如动态gif）也使用image类型
audio	表明是某种音频文件
video	表明是某种视频文件
application	表明是某种二进制数据

multipart/form-data
multipart/byteranges

### text/plain

### text/javascript

### text/html

### text/css

### application/x-www-form-urlencoded

用于表单数据编码为key/value格式发起请求

### application/octet-stream

用于二进制流数据

### multipart/form-data

用于在表单中上传文件时

### multipart/byteranges

### 'Image types'

### 'Audio and video types'


application/json
application/pdf
